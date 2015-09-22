## Git相关

### Git Tag
列出所有标签
```
git tag
```
标签分为两中,轻量级的和富含标注的.轻量级的标签像不会变化的分支,实际上它就是个指向特定提交对象的引用.
而含附注标签,实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明.
```
// 创建轻量级标签
git tag tagName
// 创建含注释的标签
git tag -a tagName -m 'here is annotation'
```
查看标签信息
```
git show tagName
```
分享标签,默认情况下```git push```不会把标签推到远端服务器上,
只能通过显式命令```git push orgin [tagName]```推送.

### 跟踪远程仓库
`git checkout -b [分支名] [远程名]/[分支名]`
1.6以上Git版本的简化命令
`git checkout --track origin/serverfix`

### 删除远程仓库
`git push [远程名] [本地分支]:[远程分支]`.如果省略[本地分支],可以删除远程仓库
```git push origin :serverfix```

或者
```git branch -r -d origin/my_remote_branch```
