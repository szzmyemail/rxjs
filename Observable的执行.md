

**Observables新建后不会立即执行**，必须subscribe它才会执行，与promise不同。

函数和Observables都是惰性计算。如果你不调用函数，函数体内的代码就不会执行。

Observables也是一样，如果你不调用它(使用 `subscribe`)，只是新建一个Observable对象，里面的console.log也不会执行。

Promise却不一样。Promise新建后就不立马执行。

```js
commonFunction() {
  console.log(111);
}
commonFunction(); //111

observableDemo() {
    new Observable(observer => {
      console.log(111, '新建Observable对象');
      setTimeout(() => {
        observer.next(4);
      },1000);
    });
  }
 observableDemo();//不会打印


  promiseDemo() {
   new Promise((resolve) => {
     console.log(111, '新建Promise对象');
     setTimeout(() => {
       resolve(4);
     },1000);
   });
  }
	promiseDemo();//111, '新建Promise对象'
```

