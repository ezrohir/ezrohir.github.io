### this

`this`  只能在函数内部使用.
随着函数使用场合的不同,this的值会发生变化.但是有一个总的原则,那就是this指的是,调用函数的那个对象.

####　纯粹的的函数调用
```
var x = 1;
　　function test(){
　　　　this.x = 0;
　　}
　　test();
　　alert(x); //0


```

####  作为对象方法的调用
函数还可以作为某个对象的方法调用，这时this就指这个上级对象。
```
function test(){
　　　　alert(this.x);
　　}
　　var o = {};
　　o.x = 1;
　　o.m = test;
　　o.m(); // 1
```


####  作为构造函数调用
所谓构造函数，就是通过这个函数生成一个新对象（object）。这时，this就指这个新对象。
```
// code1
function test(){
　　　　this.x = 1;
　　}
　　var o = new test();
　　alert(o.x); // 1

// code2
var x = 2;
　　function test(){
　　　　this.x = 1;
　　}
　　var o = new test();
　　alert(x); //2
```

#### apply调用
apply()是函数对象的一个方法，它的作用是改变函数的调用对象，它的第一个参数就表示改变后的调用这个函数的对象。因此，this指的就是这第一个参数。
```
var x = 0;
　　function test(){
　　　　alert(this.x);
　　}
　　var o={};
　　o.x = 1;
　　o.m = test;
　　o.m.apply(); //0

　　o.m.apply(o); //1
```
