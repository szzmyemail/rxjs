subscription表示可清理资源的对象，通常是Observable的执行。

Subscription 有一个重要的方法，即 `unsubscribe`，它不需要任何参数，只是用来清理由 Subscription 占用的资源。

```js
var observable = Rx.Observable.interval(1000);
var subscription = observable.subscribe(x => console.log(x));
// 稍后：
// 这会取消正在进行中的 Observable 执行
// Observable 执行是通过使用观察者调用 subscribe 方法启动的
subscription.unsubscribe();
```





Subscription 还可以合在一起，这样一个 Subscription 调用 `unsubscribe()` 方法，可能会有多个 Subscription 取消订阅 。你可以通过把一个 Subscription 添加到另一个上面来做这件事：

```js
var observable1 = Rx.Observable.interval(400);
var observable2 = Rx.Observable.interval(300);

var subscription = observable1.subscribe(x => console.log('first: ' + x));
var childSubscription = observable2.subscribe(x => console.log('second: ' + x));

subscription.add(childSubscription);

setTimeout(() => {
  // subscription 和 childSubscription 都会取消订阅
  subscription.unsubscribe();
}, 1000);
```





Subscriptions 还有一个 `remove(otherSubscription)` 方法，用来撤销一个已添加的子 Subscription 。