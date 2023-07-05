观察者是由Observable发送的值的消费者。

是一组回调函数的集合，每个回调函数对应一种observable发送的通知类型： next、error、complete。

```js
var observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};
```

要使用观察者，需要把它提供给Observable的subscribe方法：

```js
observable.subscribe(observer)
```







观察者也可以是部分的。如果你没有提供某个回调函数，Observable 的执行也会正常运行，只是某些通知类型会被忽略，因为观察者中没有相对应的回调函数。

下面是没有complete回调函数的观察者：	

```js
var observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
};
```



三种类型的回调函数都可以直接作为参数来提供：

```js
observable.subscribe(
  x => console.log('Observer got a next value: ' + x),
  err => console.error('Observer got an error: ' + err),
  () => console.log('Observer got a complete notification')
);
```

