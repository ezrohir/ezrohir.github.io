## mangoDB

### 查询
```
db.users.find(
    { age:{$gt: 18 }},
    { name: 1,address: 1}
).limit(5)
相当于
SELECT _id,name,address
FROM users
WHERE age > 18
LIMIT 5
```
