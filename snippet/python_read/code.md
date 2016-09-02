+ list 遍历操作
```
>>> params = {"server":"mpilgrim", "database":"master", "uid":"sa", "pwd":"secret"}
>>> params.items()
[('server', 'mpilgrim'), ('uid', 'sa'), ('database', 'master'), ('pwd', 'secret')]
>>> [k for k, v in params.items()]                
['server', 'uid', 'database', 'pwd']
>>> [v for k, v in params.items()]                
['mpilgrim', 'sa', 'master', 'secret']
>>> ["%s=%s" % (k, v) for k, v in params.items()]
['server=mpilgrim', 'uid=sa', 'database=master', 'pwd=secret']
```
+ list 过滤
```
[mapping-expression for element in source-list if filter-expression]
li = ["a", "mpilgrim", "foo", "b", "c", "b", "d", "d"]
[elem for elem in li if li.count(elem) == 1
```

### and 和 or
'a' and 'b'
'b'
'' and 'b'
''
'a' and 'b' and 'c'
'c'

### 类的定义
+ Python 支持多重继承.
