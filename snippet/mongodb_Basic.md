### brew安装完提升
>To have launchd start mongodb at login:
    ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
Then to load mongodb now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
Or, if you don't want/need launchctl, you can just run:
    mongod --config /usr/local/etc/mongod.conf


### 启动mongodb
执行 `mongo' 启动 shell.

在shell中,有一个全局变量db,使用哪个数据库 ,那个数据库就会被复制给
这个全局变量db,如果哪个数据库不在,就会新建.


```
> use mydb
switched to db mydb
> db
mydb
```

## db操作
查看服务器上的数据库
```
show dbs
```
切换到 test 数据库
```
use test
```
查看当前数据库中所有合集
```
show collections
```
创建数据库
```
use test2
```
删除数据库
```
db.dropDatabase()
```

### collection级操作
新建collection
```
db.createCollection("hello")
db.hello.insert({"name":"abcdef"})
```
删除collection
```
db.hello.drop()
```
重命名collection
将hello2集合重命名为hello
```
db.hello2.renameCollection("hello")
```
查看当前数据库中所有的collection
```
show collections
```

### CURD操作
#### 查询操作
插入操作
```
db.user.insert({'name':'Gal Gadot','gender':'female','age':28,'salary':10000})
```

条件查找
```
"date": datetime.datetime.utcnow()}


查询age为23的数据
db.user.find({"age":23})

查询salary大于5000的数据
db.user.find({salary:{$gt:500}})

查询name中包含`a`的数据
db.user.find({name:/a/})

查询name以G大头的数据
db.user.find({name:/^G/})
```

多条件"与"

查询age小玉30,salary大于6000的数据
db.user.find(age:{$lt:30},salary:{$gt:6000})

多条件"或"

查询age小于25,或者salary大于10000的记录
```
db.user.find({$or:[{salary:{$ge:10000}},{age:{$lt:25}}]})
```
不等于查询

查询年龄不等于23的记录
```
db.user.find({"age":{ $ne:23 }})
```

查询第一条记录

查询符合条件的第一条记录
```
db.user.findOne({"age":23})
```

查询记录的指定字段
```
db.user.find({},{name:1,age:1,salary:1})
```

对查询结果格式化
```
db.user.find().pretty()
```

对结果集排序
```
升序
db.user.find().sort({salary:1})
降序
db.user.find().sort({salary:-1})
```

#### 删除操作
删除集合中所有数据
```
db.test.remove()
```
参数符合条件的某条数据
```
db.user.remove({name:'lfqy'})
```
