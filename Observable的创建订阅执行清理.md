- ###### 创建

  Observable可以使用create或者使用New来创建，但通常我们使用所谓的创建操作符，比如of， from, interval等。

  ```js
  let a = new Observable(observer => {
        console.log(111, '新建Observable对象');
        setTimeout(() => {
          observer.next(4);
        },1000);
      });
  ```

  

- ###### 订阅

  subscribe

  ```
  a.subscribe(val => {
   console.log(val);
  });
  ```

- ###### 执行

  ```
  //这个代码表示observable的执行
  //是惰性计算，只有在每个观察者订阅后才会执行
  setTimeout(() => {
  	observer.next(4);
  },1000);
        
  ```

  Observable执行可以传递三种类型的值：

  next:  发送一个值

  error：发送一个JS错误或异常

  complete: 不再发送任何值 

  ```js
  new Observable(observer => {
    observer.next(1);
    observer.next(2);
    observer.next(3);
    observer.complete();
    observer.next(4); // 因为违反规约，所以不会发送
  });
  ```

  

- ###### 清理

  ```js
  let subscription = a.subscribe(val => {
   console.log(val);
  });
  subscription.unsubscibe();
  ```

  