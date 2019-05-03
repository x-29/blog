---
ID: 31
post_title: Nginx 配置 WordPress
author: hemi
post_excerpt: ""
layout: post
permalink: http://hemi.im/archives/31
published: true
post_date: 2019-03-14 11:42:58
---
<!-- wp:paragraph -->
<p>本文记录一下在 64 位 CentOS 7 上用 Nginx、PHP 7.3、MySQL 5.7 配置 Wordpress 的过程及碰到的一些问题。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>安装 PHP 7.3</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Wordpress 5.1 要求 PHP 7.3 及以上版本，在  CentOS 7 中用 yum 安装默认是 5.6，所以要先更新 rpm </p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
yum install yum-utils
yum-config-manager --enable remi-php73</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>然后，安装 PHP 及相关依赖组件</p>
<!-- /wp:paragraph -->

<!-- wp:shortcode -->
yum install php php-fpm php-common php-mysql php-gd php-xml php-mbstring php-mcrypt
<!-- /wp:shortcode -->

<!-- wp:paragraph -->
<p>安装完成后，检查是否安装成功 : php -v</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">[xxxxxx@xxxxx ~]# php -v
PHP 7.3.3 (cli) (built: Mar  5 2019 13:50:38) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.3, Copyright (c) 1998-2018 Zend Technologies </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>安装 MySQL</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>从官网上下载 MySQL</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>wget <a href="https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.23-linux-glibc2.12-x86_64.tar.gz">https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.23-linux-glibc2.12-x86_64.tar.gz</a><br><br>官方建议二进制文件放置在 /usr/local 下</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ tar xzvf mysql-5.7.23-linux-glibc2.12-x86_64.tar.gz -C /usr/local</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>解压完后，重命名目录为 mysql<br><br>创建 data 目录<br><br>$ mkdir -p /data/mysql<br><br>配置 /etc/my.cnf，主要是下面两项 : </p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>[mysqld]
basedir=/usr/local/mysql
datadir=/data/mysql</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>初始化 MySQL</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ /usr/local/mysql/bin/mysqld --initialize --user=&lt;当前用户> --datadir=/data/mysql --basedir=/usr/local/mysql

记住最后面的 root 密码</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>启动 MySQL</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ /usr/local/mysql/support-files/mysql.server start</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>登录msyql，输入密码（密码为上一步骤初始化生成的密码</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ /usr/local/mysql/bin/mysql -u root -p

修改密码 ：

msql>alter user 'root'@'localhost' identified by '修改后的密码';
mysql>use mysql;
msyql>update user set user.Host='%' where user.User='root';
mysql>flush privileges;
mysql>quit</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>MySQL 安装配置完成。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>编译安装 Nginx </strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>先安装编译时依赖的库</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ yum install -y gcc gcc-c++ openssl openssl-devel</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>下载并解压源码包</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// 下载nginx源码包
wget http://nginx.org/download/nginx-1.13.9.tar.gz

// 下载pcre源码包
wget https://ftp.pcre.org/pub/pcre/pcre-8.41.tar.gz

// 下载zlib源码包
wget http://www.zlib.net/zlib-1.2.11.tar.gz

// 解压
tar zxvf nginx-1.13.9.tar.gz
tar zxvf pcre-8.41.tar.gz
tar zxvf zlib-1.2.11.tar.gz</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>开始安装</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// 进入nginx目录
cd nginx-1.13.9

// 配置， -with-pcre=../pcre-8.41 --with-zlib=../zlib-1.2.11 这么写是因为我把zlib和pcre都放在nginx源码包同一级目录下

./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-pcre=../pcre-8.41 --with-zlib=../zlib-1.2.11

// 配置成功之后，编译并安装
make &amp;&amp; make install</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>测试是否安装成功</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>启动 nginx</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>/usr/local/nginx/sbin/nginx</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>访问 ip ，看到 nginx 欢迎字样，则安装成功。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>配置 Wordpress</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>在 MySQL 中创建 wordpress 使用的数据库和用户</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'new_password_here';
GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>下载 wordpress 最新版 ( 5.1.1 )</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>cd /tmp &amp;&amp; wget https://wordpress.org/latest.tar.gz
tar -zxvf latest.tar.gz
sudo mv wordpress /var/www/html/wordpress</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>设置 workpress 目录和文件权限</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo chown -R www-data:www-data /var/www/html/wordpress/
sudo chmod -R 755 /var/www/html/wordpress/</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>配置 wp-config.php</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpressuser');

/** MySQL database password */
define('DB_PASSWORD', 'user_password_here');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>配置 Nginx，在 nginx.cnf 中配置如下内容</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>server {
    listen 8080;
    root /var/www/html/wordpress;
    index  index.php index.html index.htm;
    server_name  example.com www.example.com;

    client_max_body_size 100M;

    location / {
        try_files $uri $uri/ /index.php?$args;        
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>最后，启动 php-fpm 和 nginx。<br></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>$ service php-fpm start
$ service nginx start</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><br></p>
<!-- /wp:paragraph -->