### Introduction
+ But unlike KVO notifications,signals can be chained together and operated.
+ ReactiveCocoa 包含了两个重要概念,这些概念是得程序不再需要私有属性.
  1. Spliting: 信号可以有多个订阅做,且作为资源服务于序列化管道的多个步骤.
  2. Combining 多个信号可以组合起来创建新的型号.


#### 常用宏
`RAC` 可以看作某个属性的值与一些型号的联动
```
RAC(self.submitButton.enabled) = [RACSignal combineLatest:@[self.usernameField.rac_textSignal, self.passwordField.rac_textSignal] reduce:^id(NSString *userName, NSString *password) {
    return @(userName.length &gt;= 6 &amp;&amp; password.length &gt;= 6);
}];
```
