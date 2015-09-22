## nginx
homebrew安装完成后提示.
>Docroot is: /usr/local/var/www

>The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

>To have launchd start nginx at login:
    ln -sfv /usr/local/opt/nginx/*.plist ~/Library/LaunchAgents
Then to load nginx now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
Or, if you don't want/need launchctl, you can just run:
    nginx


### 80端口被占总问题
Short version which you can pass to kill command:
```
lsof -i:80 -t
```
查看80端口被占用情况
```
sudo lsof -i :80
sudo lsof -n -P| grep :80
```
某个输出情况
```
httpd       80            root    4u     IPv6 0x56d407d8182d9269        0t0      TCP *:80 (LISTEN)
httpd      244            _www    4u     IPv6 0x56d407d8182d9269        0t0      TCP *:80 (LISTEN)
Shadowsoc  711         Ezrohir   19u     IPv4 0x56d407d81a04c779        0t0      TCP *:8090 (LISTEN)
```
httpd  是 Apache 进程.

关闭 Apache 服务.
```
# 永久关闭Apache
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```

### nginx命令
`sudo nginx -s stop && sudo nginx`


###　其他问题

```
autoload.php No such file or directory

Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for doctrine/mongodb 1.1.6 -> satisfiable by doctrine/mongodb[1.1.6].
    - doctrine/mongodb 1.1.6 requires ext-mongo >=1.2.12,<1.6-dev -> the requested PHP extension mongo is missing from your system.
  Problem 2
    - doctrine/mongodb 1.1.6 requires ext-mongo >=1.2.12,<1.6-dev -> the requested PHP extension mongo is missing from your system.
    - doctrine/mongodb-odm 1.0.0-BETA11 requires doctrine/mongodb >=1.1.5,<2.0 -> satisfiable by doctrine/mongodb[1.1.6].
    - Installation request for doctrine/mongodb-odm 1.0.0-BETA11 -> satisfiable by doctrine/mongodb-odm[1.0.0-BETA11].

是缺少php55-mongo,brew install php55-mongo 即可.
```

```
Warning: Backing up all known pear.conf and .pearrc files
Warning: If you have a pre-existing pear install outside
         of homebrew-php, or you are using a non-standard
         pear.conf location, installation may fail.
==> ./configure --prefix=/usr/local/Cellar/php55/5.5.28 --localstatedir=/usr/local/var --sysconfdir=/usr/local/etc/php/
checking whether to enable the SQLite3 extension... yes
checking bundled sqlite3 library... yes
checking for ZLIB support... yes
checking if the location of ZLIB install directory is defined... no
configure: error: Cannot find libz

READ THIS: https://git.io/brew-troubleshooting
If reporting this issue please do so at (not Homebrew/homebrew):
  https://github.com/josegonzalez/homebrew-php/issues
```

## Configule
when install by homebrew,it remind.
> The php.ini file can be found in:
    /usr/local/etc/php/5.5/php.ini
    ✩✩✩✩ Extensions ✩✩✩✩

    If you are having issues with custom extension compiling, ensure that
    you are using the brew version, by placing /usr/local/bin before /usr/sbin in your PATH:

          PATH="/usr/local/bin:$PATH"

    PHP55 Extensions will always be compiled against this PHP. Please install them
    using --without-homebrew-php to enable compiling against system PHP.

    ✩✩✩✩ PHP CLI ✩✩✩✩

    If you wish to swap the PHP you use on the command line, you should add the following to ~/.bashrc,
    ~/.zshrc, ~/.profile or your shell's equivalent configuration file:

          export PATH="$(brew --prefix homebrew/php/php55)/bin:$PATH"

    ✩✩✩✩ FPM ✩✩✩✩

    To launch php-fpm on startup:
        mkdir -p ~/Library/LaunchAgents
        cp /usr/local/opt/php55/homebrew.mxcl.php55.plist ~/Library/LaunchAgents/
        launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.php55.plist

    The control script is located at /usr/local/opt/php55/sbin/php55-fpm

    OS X 10.8 and newer come with php-fpm pre-installed, to ensure you are using the brew version you need to make sure /usr/local/sbin is before /usr/sbin in your PATH:

      PATH="/usr/local/sbin:$PATH"

    You may also need to edit the plist to use the correct "UserName".

    Please note that the plist was called 'homebrew-php.josegonzalez.php55.plist' in old versions
    of this formula.

    To have launchd start josegonzalez/php/php55 at login:
      ln -sfv /usr/local/opt/php55/*.plist ~/Library/LaunchAgents
    Then to load josegonzalez/php/php55 now:
      launchctl load ~/Library/LaunchAgents/homebrew.mxcl.php55.plist


> To finish installing mongo for PHP 5.5:
  * /usr/local/etc/php/5.5/conf.d/ext-mongo.ini was created,
    do not forget to remove it upon extension removal.
  * Validate installation via one of the following methods:
  *
  * Using PHP from a webserver:
  * - Restart your webserver.
  * - Write a PHP page that calls "phpinfo();"
  * - Load it in a browser and look for the info on the mongo module.
  * - If you see it, you have been successful!
  *
  * Using PHP from the command line:
  * - Run "php -i" (command-line "phpinfo()")
  * - Look for the info on the mongo module.
  * - If you see it, you have been successful!


### php-fpm
/usr/local/Cellar/php55/5.5.28/sbin/php55-fpm {start|stop|force-quit|restart|reload|status}
