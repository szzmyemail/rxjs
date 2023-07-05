### Observable的使用

```javascript
//observableLearnService service  
//创建Observable对象
import { Observable } from 'rxjs';
observable() {
  return new Observable(observer => {
  	setTimeout(() => {
  	observer.next('过了1秒钟');
  }, 1000)
}
                        
                        
// component
// 订阅                        
                     
subscribeObservable() {
    this.observableLearnService.observable().subscribe(res => {
      this.msg += res;
      console.log(res);  //过了1秒钟
    })
}
```



### angular中的httpClient模块

Angular自带有http模块可以方便的进行Http请求。不必像Vue那样安装配置axios。

![image-20220302150556920](/Users/lilysong/Library/Application Support/typora-user-images/image-20220302150556920.png)



其中的http模块是对rxjs的封装。