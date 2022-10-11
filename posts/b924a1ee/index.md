# 手动安装Nextcloud

试过docker、snpa等方式部署nextcloud，但是存在系统匹配、资源占用大等各种问题。而且配置都是写死的，看似安装部署简单，其实折腾起来更痛苦。

尤其是在内网环境里,不涉及升级的问题,手动安装是更好的选择，而且超级简单。

本文假设nextcloud的服务器地址是**192.168.8.14**

## 安装各种依赖软件

### Mariadb

1. 安装

```
sudo apt update

sudo apt -y install mariadb-server mariadb-client
```

2. 配置

```
sudo mysql_secure_installation 

sudo mysql -u root -p
```
输入密码以登录

3. 创建nextcloud的用户和数据库

```
CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY '密码';

CREATE DATABASE nextcloud;

GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost';

FLUSH PRIVILEGES;

QUIT
```
以上，创建了用户名为`nextcloud`、密码是自定义的密码、数据库为`nextcloud`，且用户`nextcloud`拥有数据库`nextcloud`所有权限。

### 安装php和Apache2

1. 安装

```
sudo apt -y install php php-{cli,xml,zip,curl,gd,cgi,mysql,mbstring,intl}

sudo apt -y install apache2 libapache2-mod-php
```

2. 修改php配置

```
sudo vim /etc/php/*/apache2/php.ini

```

下面是修改后的内容：

```
date.timezone = Asia/Shanghai

memory_limit = 1000M

upload_max_filesize = 16000M

post_max_size = 16000M

max_execution_time = 300
```

## 安装nextcloud

1. 下载
```
wget https://download.nextcloud.com/server/releases/latest.zip
```

2. 解压
```
unzip latest.zip
```
3. 下面的操作，是把`/var/www/html`目录改名，把解压出来的`nextcloud`改名为`html`,并把它放在`/var/www/`下面
```
cd /var/www

sudo mv html html_origin

cd ~

mv nextcloud html

sudo mv html /var/www/
```

4. 更改权限
```
sudo chown -R www-data:www-data /var/www/html/

sudo sudo chmod -R 755 /var/www/html/
```

### 重启apache2

```
sudo systemctl restart apache2
```

输入http://ip，就可以访问页面来安装了

## 启用全站https

有些插件，像是`Passwords`等，要求必须使用https来连接，而且https连接也更安全，下面使用`nginx`的反向代理功能来启用nextcloud的全站https

1. 生成自签名证书
> 内网环境，无法使用Let's Crypt等证书，这里使用自签名证书
当前用户目录下新建一个sslcert目录
```
cd ~

mkdir sslcert

cd sslcert
```

生成证书的命令： 

```
openssl req -newkey rsa:4096 \
            -x509 \
            -sha256 \
            -days 36500 \
            -nodes \
            -out nc.crt \
            -keyout nc.key
```
根据提示输入内容，最后生成名为`nc.crt`和`nc.key`2个证书。

2. 更改apache2的端口号
由于需要使用`nginx`，为避免端口冲突导致apache2无法启动，这里先把`apache2`的端口由`80`改成`81`
```
sudo systemctl stop apache2   ##先关闭apache2服务

sudo vi  /etc/apache2/ports.conf
```
将`Listen 80`改成`Listen 81`

3. 安装nginx

```
sudo apt install nginx
```
4. 新建nginx配置文件
```
sudo vi /etc/nginx/conf.d/nextcloud.conf
```

内容如下:

```
server {
        listen          443 ssl;
        server_name     192.168.8.14;  #改成你的服务器IP或者域名
        ssl_certificate         /home/myserver/sslcert/nc.crt; #改成你的证书路径
        ssl_certificate_key     /home/myserver/sslcert/nc.key; #改成你的证书路径
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;

        location ~ {
                client_max_body_size 16000M;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header X-Forwarded-Port $server_port;
                proxy_set_header X-Forwarded-Host $server_name;
                add_header Front-End-Https on;
                proxy_set_header X-Nginx-Proxy true;
                proxy_pass http://192.168.8.14:81; #这里要改成81,和apache2修改过的Listen 端口对应
                proxy_buffering off;
                proxy_max_temp_file_size 0;
                proxy_redirect off;
    }
}

```
5. 修改nextcloud配置文件

```
sduo vi /var/www/html/config/config.php

```
照着下面的内容自己修改:
```
  'trusted_domains' => 
  array (
    0 => '127.0.0.1',
    1 => '192.168.8.14',
  ),

  'trusted_proxies'   => ['127.0.0.1:443'],
  'overwritehost'     => '192.168.8.14:443',
  'overwriteprotocol' => 'https',
  'overwritewebroot'  => '/',
  'htaccess.RewriteBase' => '/',
  'overwrite.cli.url' => 'https://192.168.8.14',
```

重启服务器,会看到nextcloud强制全站走https了。

