#### Observable是惰性运算

```typescript

function foo() {
  console.log('hello');
}
foo(); //'hello
foo(); //'hello'




foo = new Observable(observer => {
  console.log('init');
  observer.next(1);
});

test07051() {
  this.foo.subscribe(res => {
    console.log(res);
  });
}
test07051();
  // 'init'
  // 1
test07051();
  // 'init'
  // 1


//函数和Observable都是惰性运算。如果不调用函数，console.log('hello')就不会执行。Observable也是如此，如果不subscribe,console.log('init')也不会执行。
//此外，‘调用’或‘订阅’是独立的操作：两个函数调用会触发两个单独的副作用，两个Observable订阅同样也是触发两个单独的副作用。EventEmitters 共享副作用并且无论是否存在订阅者都会尽早执行，Observables 与之相反，不会共享副作用并且是延迟执行。
```

#### Observable可以是同步，也可以是异步

```typescript
foo = new Observable(observer => {
  console.log('init');
  observer.next(1);
});

test07051() {
  console.log('before');
  this.foo.subscribe(res => {
    console.log(res);
  });
  console.log('after');
}
test07051();
  // 'before'
  // 'init'
  // 1
  // 'after'
//这证明foo的订阅完全是同步的，就像函数一样。
//二者的区别是：Observable可以随着时间的推移“返回”多个值，这是函数做不到的。

foo() {
  console.log(1);
  return 42;
  return 100;//这行永远无法执行
}

foo = new Observable(observer => {
  console.log(1);
  observer.next(42);
  observer.next(100); // “返回”另外一个值
  observer.next(200); // 还可以再“返回”值
});
console.log('before');
foo.subscribe(res => console.log(res));
console.log('after');
//"before"
// 1
//42
//100
//200
//"after"


//也可以异步地“返回”值：
foo = new Observable(observer => {
  console.log(1);
  observer.next(42);
  observer.next(100); // “返回”另外一个值
  observer.next(200); // 还可以再“返回”值
  setTimeout(() => {
    observer.next(300); // 异步执行
  }, 1000);
});
console.log('before');
foo.subscribe(res => console.log(res));
console.log('after');
//"before"
//"Hello"
//42
//100
//200
//"after"
//300
```

#### 在 Observable 执行中, 可能会发送零个到无穷多个 "Next" 通知。如果发送的是 "Error" 或 "Complete" 通知的话，那么之后不会再发送任何通知了。

```typescript
foo = new Observable(observer => {
  console.log(1);
  observer.next(42);
  observer.next(100); // “返回”另外一个值
  observer.next(200); // 还可以再“返回”值
  observer.complete();
  observer.next(4);//因为违反规约，所以不会发送
});
```

#### 清理Observable执行

##### 当你订阅了 Observable，你会得到一个 Subscription ，它表示进行中的执行。只要调用 `unsubscribe()` 方法就可以取消执行。

```typescript
var observable = Rx.Observable.from([10, 20, 30]);
var subscription = observable.subscribe(x => console.log(x));
// 稍后：
subscription.unsubscribe();
```

##### Subscription 还可以合在一起，这样一个 Subscription 调用 `unsubscribe()` 方法，可能会有多个 Subscription 取消订阅 。你可以通过把一个 Subscription 添加到另一个上面来做这件事：

```typescript
subscription.add(childSubscription);
subscription.unsubscribe();
```

