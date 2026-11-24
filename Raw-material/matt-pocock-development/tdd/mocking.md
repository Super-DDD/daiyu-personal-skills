# 什么时候用 mock

只在系统边界使用 mock，例如外部 API（付款、邮件等）、时间或随机数，有时也包括文件系统和数据库。数据库优先用 test DB。自己的 class、module、内部协作者及其他能控制的东西都使用实际实现。

## 为 mock 设计 interface

**1. 使用 dependency injection。** 把外部依赖传进来。

```typescript
// Easy to mock
function processPayment(order, paymentClient) {
  return paymentClient.charge(order.total);
}

// Hard to mock
function processPayment(order) {
  const client = new StripeClient(process.env.STRIPE_KEY);
  return client.charge(order.total);
}
```

**2. 优先采用 SDK 风格的 interface。** 给每个外部操作定义专门的函数。若只提供一个通用 fetch 函数，mock 就得用条件分支区分操作。

```typescript
// GOOD: Each function is independently mockable
const api = {
  getUser: (id) => fetch(`/users/${id}`),
  getOrders: (userId) => fetch(`/users/${userId}/orders`),
  createOrder: (data) => fetch('/orders', { method: 'POST', body: data }),
};

// BAD: Mocking requires conditional logic inside the mock
const api = {
  fetch: (endpoint, options) => fetch(endpoint, options),
};
```

这样每个 mock 都返回特定形状的数据，测试准备过程省去了分支，能看清测试实际经过哪些 endpoint，每个 endpoint 也都有自己的类型保障。

[英文原文](https://github.com/mattpocock/skills/blob/abeeaf9/skills/engineering/tdd/mocking.md)
