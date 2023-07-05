一些人声称Observables是异步的。那不是真的。

Observables 传递值可以是同步的，也可以是异步的。

```js
subscribeObservable1() {
    let O = new Observable(observer => {
      observer.next(1);
      observer.next(2);
      observer.next(3);
      setTimeout(() => {
        observer.next(4);
        observer.complete();
      },1000);
    });
  
  	console.log('before');
    O.subscribe({
      next: x => console.log('got value ' + x),
      error: err => console.log('got error ' + err),
      complete: () => console.log('done'),
    })
    
    console.log('after');
    // before
    // got value 1
    // got value 2
    // got value 3
    // after
    // got value 4
    // done
  }
```

