### 创建symfony command
```
sudo curl -LsS http://symfony.com/installer -o /usr/local/bin/symfony
sudo chmod a+x /usr/local/bin/symfony
```
### 创建symfony Appplication
```
symfony new my_project_name
# use the most recent version in any Symfony branch
symfony new my_project_name 2.6
# use the most recent LTS (Long Term Support) version
symfony new my_project_name lts
```

### Composer
`Composer` 是 PHP application 的包依赖管理工具,也可以
用来创建 Symfony Framework.

### 用Composer创建Symfony Application
```composer create-project symfony/framework-standard-edition my_project_name```

### 启动Symfony Aopplication
Symfony 内置了 web server,启动时候:
```
cd my_project_name/
php app/console server:run

php app/console server:stop
```

### 检查Symfony Application 配置
Product: @{@“id”: @“123456”,
                   @“name”: @“little”,
                   @“model”: @{@“no”: @“model 1586”,  @“desc”: @“a short description”} }
映射为：
@interface Product : JSONModel
@property (copy, nonatomic) NSString * id;
@property (copy, nonatomic) NSString * name;
@property (copy, nonatomic) NSString * modelNo;
@property (copy, nonatomic) NSString * modelDesc;
@end

现在需要你 Fork JSONModel 这个库，对 Key Mapping 特性的机制进行修改，使之支持：
将
Product: {@“id”: @“123456”,
                @“name”: @“little”,
                @“infoNo”: @“model 1586”,  
                @“infoDesc”: @“a short description”}

映射为：
@interface Product : JSONModel
@property (copy, nonatomic) NSString * id;
@property (copy, nonatomic) NSString * name;
@property (strong, nonatomic) Product Info  * info;
@end

@interface ProductInfo : JSONModel
@property (copy, nonatomic) NSString * no;
@property (copy, nonatomic) NSString * desc;
<!-- @end -->
```
http://localhost:8000/config.php
```

###　设置权限
`app/cache` `app/logs` 文件夹需要写权限,在UNIX system:
```
rm -rf app/cache/*
rm -rf app/logs/*

HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
sudo chmod +a "$HTTPDUSER allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
sudo chmod +a "`whoami` allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
```


### 更新Symofony Applications
```
cd my_project_name/
composer update
```
