# 手动安装Nextcloud

试过docker、snpa等方式部署nextcloud，但是存在系统匹配、资源占用大等各种问题。而且配置都是写死的，看似安装部署简单，其实折腾起来更痛苦。

尤其是在内网环境里,不涉及升级的问题,手动安装是更好的选择，而且超级简单。

本文假设nextcloud的服务器地址是**192.168.8.14**

操作系统： ubuntu 22.04 server

## 安装apache2、mariadb、php、redis

```
sudo apt update && sudo apt upgrade

sudo apt install apache2 mariadb-server libapache2-mod-php php-gd php-mysql php-curl php-mbstring php-intl php-gmp php-bcmath php-xml php-imagick php-zip php-readline php-apcu redis-server php-redis
```

## 配置mysql

```
sudo mysql_secure_installation 

sudo mysql -u root -p
```
输入密码以登录

创建nextcloud的用户和数据库

```
CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY '密码';

CREATE DATABASE nextcloud;

GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost';

FLUSH PRIVILEGES;

QUIT
```
以上，创建了用户名为`nextcloud`、密码是自定义的密码、数据库为`nextcloud`，且用户`nextcloud`拥有数据库`nextcloud`所有权限。

## 配置apache
- 启用apache功能模块
```
sudo a2enmod rewrite headers env dir mime ssl
```

- 新建2个网站配置文件 

1. 新建`/etc/apache2/sites-available/nextcloud.conf`,
```
<IfModule mod_ssl.c>
   <VirtualHost _default_:443>

     ServerName 192.168.8.14
     DocumentRoot /var/www/nextcloud

     <Directory /var/www/nextcloud/>
       Options +FollowSymlinks
       AllowOverride All

      <IfModule mod_dav.c>
        Dav off
      </IfModule>

       SetEnv HOME /var/www/nextcloud
       SetEnv HTTP_HOME /var/www/nextcloud
     </Directory>

     <IfModule mod_headers.c>
          Header always set Strict-Transport-Security "max-age=15768000; preload"
     </IfModule>

     SSLEngine on
     SSLCertificateFile /etc/ssl/certs/ssl-cert-snakeoil.pem
     SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key

   </VirtualHost>
</IfModule>

```
这是网站的主配置文件,使用了apache自带的证书。

2. 新建 `/etc/apache2/sites-available/nextcloud-redirect.conf`,内容如下:

```
<VirtualHost *:80>
   ServerAdmin 192.168.8.14

   RewriteEngine On
   RewriteCond %{HTTPS} off
   RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]
</VirtualHost>

```
这是http到https的跳转配置文件

3. 启用网站配置
```
sudo a2ensite nextcloud.conf
sudo a2ensite nextcloud-redirect.conf
#禁用默认的配置文件
sudo a2dissite 000-default.conf  
```

## 修改php配置

```
sudo vim /etc/php/*/apache2/php.ini

```

找到下面的内容，对应修改。下面是修改后的内容：

```
date.timezone = Asia/Shanghai

memory_limit = 2048M

upload_max_filesize = 16000M

post_max_size = 16128M

max_execution_time = 300
```
`post_max_size`的值最好比`upload_max_filesize`大一些。

## 安装nextcloud

1. 下载
```
wget https://download.nextcloud.com/server/releases/latest-24.zip
```
nextcloud 25版本刚出，app还没适配完成，这里用24版本。

2. 解压
```
unzip latest-24.zip
```
3. 下面的操作，是把解压出来的`nextcloud`放在`/var/www/`下面,并且修改权限
```
sudo mv nextcloud /var/www/

sudo chown -R www-data:www-data /var/www/nextcloud
```

### 重启apache2

```
sudo systemctl restart apache2
```

输入http://192.168.8.14，应该会自动跳转到https://192.168.8.14 ,

根据页面提示安装即可.

## 优化nextcloud

###  设置redis缓存
1. 修改redis配置文件
```
sudo vim /etc/redis/redis.conf
```
打到对应位置,修改成下面的配置:

```
port 0
unixsocket /var/run/redis/redis-server.sock
unixsocketperm 770
```
2. 添加用户www-data使用redis的权限
```
sudo usermod -a -G redis www-data
```
3. 重启apache
```
sudo systemctl restart apache2
```
4. 修改nextcloud配置文件
在`/var/www/html/nextcloud/config/config.php`里添加如下配置:
```
'memcache.local' => '\\OC\\Memcache\\Redis',
'memcache.locking' => '\\OC\\Memcache\\Redis',
'filelocking.enabled' => 'true',
'redis' => 
array (
'host' => '/var/run/redis/redis-server.sock',
'port' => 0,
'timeout' => 0.0,
),
```
5. 开机启动redis
```
sudo systemctl enable redis-server
```

### 使用Pretty Links
即隐藏URL中的*.php的内容

修改nextcloud的配置文件:
```
sudo vim /var/www/html/nextcloud/config/config.php
```
添加如下内容:
```
'htaccess.RewriteBase' => '/',
```
在nextcloud的主目录`/var/www/nextcloud`下运行命令：
```
sudo -u www-data php occ maintenance:update:htaccess
```
### PHP Opcache设置

修改php配置文件
```
sudo vim /etc/php/*/apache2/php.ini
```
添加如下内容:
```
opcache.enable=1
opcache.enable_cli=1
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=10000
opcache.memory_consumption=128
opcache.save_comments=1
opcache.revalidate_freq=1
```
### 电话区域和本地服务跳转配置
修改nextcloud的配置文件:
```
sudo vim /var/www/html/nextcloud/config/config.php
```
添加如下内容:
```
'default_phone_region' => 'CN',
'allow_local_remote_servers' => true,
```

### Collabora无法在线打开.docx等文档

现象：一直显示“正在加载 xxxx.docx”，但是一直显示不出来。

原因：这是由于系统缺少相关依赖造成的。

解决：根据Collabora官方文档，至少需要下面两个软件
`fontconfig`、`fuse`，安装即可

```
sudo apt install fontconfig fuse
```

### Collabora无法显示中文字体

解决方法：把需要用到的中文字体，拷贝到`/usr/local/share/fonts/`目录下。

```
sudo cp SomeFont.ttf /usr/local/share/fonts/
```
