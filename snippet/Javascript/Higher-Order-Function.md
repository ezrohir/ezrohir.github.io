## 高阶函数
高阶函数,就是让函数的参数能接收别的函数.

Array的`map`方法
```
function pow(x){
    return x*x
}
var arr = [1,2,3,4,5,6,7,8,9];
arr.map(pow);     // [1,4,9,16,25,36,49,64,81]

var arr2 = [1,2,3,4,5];
arr.map(String);  // ['1','2','3','4','5']
```

Array的`reduce`方法

Array的`reduce()`把一个函数作用在这个Array的`[x1,x2,x3...]`上,
效果就是;
```
[x1,x2,x3,x4].reduce(f)=f(f(f(x1,x2),x3),x4)

var arr = [1,3,5,7,9];
arr.reduce(function(x,y){
    return x+y;
});   //25
```

高阶函数除了可以接受函数作为参数外,还可以把函数作为结果值返回.
```
function sum(arr) {
    return arr.reduce(function (x, y) {
        return x + y;
    });
}

sum([1, 2, 3, 4, 5]); // 15

function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}
var f = lazy_sum([1, 2, 3, 4, 5]); // function sum()
f(); // 15
```
