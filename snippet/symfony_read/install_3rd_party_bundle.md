## How to Install 3rd Party Bundles
### Add Composer Dependencies
Composer eg.

### Enable the Bundle
Register bundles in `app/Appkernel.php`.

### Configure the Bundle
+ configuration in `app/config/config.yml`
+ The bundle's documentation will tell you about the configuration.
+ `app/console config:dump-reference AsseticBundle` 会输出默认的配置


### How to Load Service configuration inside a Bundle
#### Createing an Extension Class


###　其他
Retrieve the configuration parameters in your code from the container:
```
# app/config/config.yml
parameters:
    acme_blog.author.email: fabien@example.com
```
```
$container->getParameter('acme_blog.author.email');
```
