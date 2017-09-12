
### 模块路径解析规则
`require`函数支持`/`开头的绝对路径,也支持`./`开头的相对路径.

1. 内置模块 <br />
   如果传递给`require`的函数是NodeJS内置模块,不在路径 解析,直接返回导出对象.如`require('fs')`
2. node_modules目录
   NodeJS定义了一个特殊的`node_modules`目录用于存放模块.如果某个模块的绝对路径是`/home/user/hello.js`,在该模块中使用`require('foo/bar')`方式加载模块时,NodeJS以此尝试使用以下路径

   ```
   /home/user/node_modules/foo/bar
   /home/node_modules/foo/bar
   /node_modules/foo/bar
   ```

3. NODE_PATH环境变量
  与PATH环境变量类似,NodeJS允许通过NODE_PATH环境变量来指定额外的模块搜索路径.NODE_PATH环境变量中包含一到多个目录路径,路径之间在Linux下使用:分隔,在Windows下使用;分隔.例如定义了以下NODE_PATH环境变量:
  ```
  NODE_PATH=/home/user/lib:/home/lib
  ```
  当使用`require('foo/bar')`方式加载模块时,NodeJS依次尝试以下路径.
  ```
  /home/user/lib/foo/bar
  /home/lib/foo/bar
  ```

## 文件操作
### Buffer
JS语言自身只有字符串数据类型,没有二进制数据类型,因为NodeJS提供了一个与String对等的全局构造函数Buffer来提供二进制数据的操作.
