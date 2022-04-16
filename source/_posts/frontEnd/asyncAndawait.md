---
title: JavaScript 异步代码的几个推荐做法
date: 2022-04-10
tags:
 - JavaScript
categories: 
 - frontEnd
---
今天给大家来推荐几个写好 JavaScript 异步代码的推荐做法，每种场景都有一个对应的 eslint 规则，大家可以选择去配置一下。

<!--more-->

## no-async-promise-executor

不建议将 `async` 函数传递给 `new Promise` 的构造函数。

```
// ❌
new Promise(async (resolve, reject) => {});

// ✅
new Promise((resolve, reject) => {});
```

首先，你在 `Promise` 的构造函数里去使用 `async` ，那么包装个 `Promise` 可能就是没啥必要的。另外，如果 `async` 函数抛出了异常，新构造的 `promise` 实例并不会 `reject` ，那么这个错误就捕获不到了。

## no-await-in-loop

不建议在循环里使用 `await`，有这种写法通常意味着程序没有充分利用 `JavaScript` 的事件驱动。

```
// ❌
for (const url of urls) {
  const response = await fetch(url);
}
```

建议将这些异步任务改为并发执行，这可以大大提升代码的执行效率。

```

// ✅
const responses = [];
for (const url of urls) {
  const response = fetch(url);
  responses.push(response);
}

await Promise.all(responses);
```

## no-promise-executor-return

不建议在 `Promise` 构造函数中返回值，`Promise` 构造函数中返回的值是没法用的，并且返回值也不会影响到 `Promise` 的状态。

```

// ❌
new Promise((resolve, reject) => {
  return result;
});
```

正常的做法是将返回值传递给 `resolve`，如果出错了就传给 `reject`。

```
// ✅
new Promise((resolve, reject) => {
  resolve(result);
});
```

## require-atomic-updates

不建议将赋值操作和 `await` 组合使用，这可能会导致条件竞争。

看看下面的代码，你觉得 `totalPosts` 最终的值是多少？

```
// ❌
let totalPosts = 0;

async function getPosts(userId) {
  const users = [{ id: 1, posts: 5 }, { id: 2, posts: 3 }];
  await sleep(Math.random() * 1000);
  return users.find((user) => user.id === userId).posts;
}

async function addPosts(userId) {
  totalPosts += await getPosts(userId);
}

await Promise.all([addPosts(1), addPosts(2)]);
console.log('Post count:', totalPosts);
```

`totalPosts` 会打印 3 或 5，并不会打印 8，你可以在浏览器里自己试一下。

问题在于读取 `totalPosts` 和更新 `totalPosts` 之间有一个时间间隔。这会导致竞争条件，当值在单独的函数调用中更新时，更新不会反映在当前函数范围中。因此，两个函数都会将它们的结果添加到 `totalPosts` 的初始值0。

避免竞争条件正确的做法：

```
// ✅
let totalPosts = 0;

async function getPosts(userId) {
  const users = [{ id: 1, posts: 5 }, { id: 2, posts: 3 }];
  await sleep(Math.random() * 1000);
  return users.find((user) => user.id === userId).posts;
}

async function addPosts(userId) {
  const posts = await getPosts(userId);
  totalPosts += posts; // variable is read and immediately updated
}

await Promise.all([addPosts(1), addPosts(2)]);
console.log('Post count:', totalPosts);
```

## max-nested-callbacks

防止回调地狱，避免大量的深度嵌套：

```
/* eslint max-nested-callbacks: ["error", 3] */

// ❌
async1((err, result1) => {
  async2(result1, (err, result2) => {
    async3(result2, (err, result3) => {
      async4(result3, (err, result4) => {
        console.log(result4);
      });
    });
  });
});

// ✅
const result1 = await asyncPromise1();
const result2 = await asyncPromise2(result1);
const result3 = await asyncPromise3(result2);
const result4 = await asyncPromise4(result3);
console.log(result4);
```

回调地狱让代码难以阅读和维护，建议将回调都重构为 `Promise` 并使用现代的 `async/await` 语法。

## no-return-await

返回异步结果时不一定要写 `await` ，如果你要等待一个 `Promise`，然后又要立刻返回它，这可能是不必要的。

```
// ❌
async () => {
  return await getUser(userId);
}
```

从一个 `async` 函数返回的所有值都包含在一个 `Promise` 中，你可以直接返回这个 `Promise`。

```
// ✅
async () => {
  return getUser(userId);
}
```

当然，也有个例外，如果外面有 `try...catch` 包裹，删除 `await` 就捕获不到异常了，在这种情况下，建议明确一下意图，把结果分配给不同行的变量。

```
// 👎
async () => {
  try {
    return await getUser(userId);
  } catch (error) {
    // Handle getUser error
  }
}

// 👍
async () => {
  try {
    const user = await getUser(userId);
    return user;
  } catch (error) {
    // Handle getUser error
  }
}
```

## prefer-promise-reject-errors

建议在 `reject Promise` 时强制使用 `Error` 对象，这样可以更方便的追踪错误堆栈。

```
// ❌
Promise.reject('An error occurred');

// ✅
Promise.reject(new Error('An error occurred'));
```

## node/handle-callback-err

强制在 `Node.js` 的异步回调里进行异常处理。

```
// ❌
function callback(err, data) {
  console.log(data);
}

// ✅
function callback(err, data) {
  if (err) {
    console.log(err);
    return;
  }

  console.log(data);
}
```

在 `Node.js` 中，通常将异常作为第一个参数传递给回调函数。忘记处理这些异常可能会导致你的应用程序出现不可预知的问题。

> 如果函数的第一个参数命名为 `err` 时才会触发这个规则，你也可以去 `.eslintrc` 文件里自定义异常参数名。

## node/no-sync

不建议在存在异步替代方案的 `Node.js` 核心 `API` 中使用同步方法。

```
// ❌
const file = fs.readFileSync(path);

// ✅
const file = await fs.readFile(path);
```

在 `Node.js` 中对 `I/O` 操作使用同步方法会阻塞事件循环。大多数场景下，执行 `I/O` 操作时使用异步方法是更好的选择。

## @typescript-eslint/await-thenable

不建议 `await` 非 `Promise` 函数或值。

```
// ❌
function getValue() {
  return someValue;
}

await getValue();

// ✅
async function getValue() {
  return someValue;
}

await getValue();
```

## @typescript-eslint/no-floating-promises

建议 `Promise` 附加异常处理的代码。

```
// ❌
myPromise()
  .then(() => {});

// ✅
myPromise()
  .then(() => {})
  .catch(() => {});
```

养成个好的习惯，永远做好异常处理！

## @typescript-eslint/no-misused-promises

不建议将 `Promise` 传递到并非想要处理它们的地方，例如 if 条件。

```
// ❌
if (getUserFromDB()) {}

// ✅ 👎
if (await getUserFromDB()) {}
```

更推荐抽一个变量出来提高代码的可读性。

```
// ✅ 👍
const user = await getUserFromDB();
if (user) {}
```
