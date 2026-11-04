# 14. WEB

[한국어](../../../Session/14-web.md) | [English](../../en/Session/14-web.md) | [日本語](../../ja/Session/14-web.md) | **简体中文** | [Español](../../es/Session/14-web.md) | [Français](../../fr/Session/14-web.md)

### [ MGMT ] 签发 Web 证书

```bash
cd /etc/ssl
vim openssl.cnf
```

```
[ v3_req ]
subjectAltName = DNS:www.jjongmarket.kr, DNS:jjongmarket.kr
```

```bash
openssl req -out www.req -newkey rsa:2048 -nodes -keyout www.key     # CN: www.jjongmarket.kr
openssl ca -in www.req -out www.crt
scp /etc/ssl/www.* /etc/ssl/RootCA.crt 10.10.10.21:/etc/ssl
scp /etc/ssl/www.* /etc/ssl/RootCA.crt 10.10.10.22:/etc/ssl
```

### [ WEB1, WEB2 ]

分别将 `/etc/hosts` 中的 `127.0.1.1` 改为 WEB1、WEB2 后再安装。

```bash
apt install -y apache2 libapache2-mod-php php-mysql php-mbstring
a2enmod ssl rewrite
```

### [ WEB1 ]

```bash
mkdir -p /var/www/jjongmarket/css /etc/jjongmarket
vim /etc/jjongmarket/db.ini
```

```
; /etc/jjongmarket/db.ini  (WEB1, WEB2) - outside the web root
; chown root:www-data, chmod 640
host     = 10.10.20.10
dbname   = shop
user     = shop
password = "ChangeMe-Shop1"
```

给 WEB1 临时添加 NAT 网卡，从宿主机发送商城文件。

```powershell
scp -r .\web\* root@<WEB1_NAT_IP>:/var/www/jjongmarket/
```

```bash
rm -f /var/www/jjongmarket/index.html
cp /etc/ssl/RootCA.crt /var/www/jjongmarket/ca.crt
vim /etc/apache2/sites-available/jjongmarket.conf
```

```
# =====================================================================
#  /etc/apache2/sites-available/jjongmarket.conf  (WEB1、WEB2 相同)
#  所需模块: ssl, rewrite, headers  (安装 libapache2-mod-php 时 php 会自动启用)
# =====================================================================

# ---- HTTP(80): 跳转到 HTTPS，但下面两个路径也通过 HTTP 响应 ----
#   /health.html   : 供 keepalived 健康检查使用
#   /ca.crt        : 客户电脑下载私有 Root CA 的地址
<VirtualHost *:80>
    ServerName  www.jjongmarket.kr
    ServerAlias jjongmarket.kr
    DocumentRoot /var/www/jjongmarket

    RewriteEngine On
    RewriteCond %{REQUEST_URI} !^/health\.html$
    RewriteCond %{REQUEST_URI} !^/ca\.crt$
    RewriteRule ^ https://www.jjongmarket.kr%{REQUEST_URI} [R=301,L]

    ErrorLog  ${APACHE_LOG_DIR}/jjongmarket_error.log
    CustomLog ${APACHE_LOG_DIR}/jjongmarket_access.log combined
</VirtualHost>

# ---- HTTPS(443): 商城主服务 ----
<VirtualHost *:443>
    ServerName  www.jjongmarket.kr
    ServerAlias jjongmarket.kr
    DocumentRoot /var/www/jjongmarket

    SSLEngine on
    SSLCertificateFile    /etc/ssl/www.crt
    SSLCertificateKeyFile /etc/ssl/www.key

    DirectoryIndex index.php

    <Directory /var/www/jjongmarket>
        Options -Indexes
        AllowOverride None
        Require all granted
    </Directory>
    # JJong LIVE — 将本机 ffmpeg(jjonglive.service)生成的 HLS 通过同一 HTTPS 地址(/hls/)提供 (a2enmod headers)
    Alias /hls/ /var/www/hls/
    <Directory /var/www/hls>
        Require all granted
        AddType application/vnd.apple.mpegurl .m3u8
        AddType video/mp2t .ts
        Header set Cache-Control "no-cache"
    </Directory>

    # 公共 PHP(数据库连接代码)禁止通过浏览器直接访问
    <Directory /var/www/jjongmarket/inc>
        Require all denied
    </Directory>

    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "SAMEORIGIN"

    ErrorLog  ${APACHE_LOG_DIR}/jjongmarket_error.log
    CustomLog ${APACHE_LOG_DIR}/jjongmarket_access.log combined
</VirtualHost>
```

```bash
chmod 755 /etc/jjongmarket
chown root:www-data /etc/jjongmarket/db.ini
chmod 640 /etc/jjongmarket/db.ini
chmod -R a+rX /var/www/jjongmarket
```

### [ WEB2 ]

```bash
scp -r 10.10.10.21:/var/www/jjongmarket/* /var/www/jjongmarket/
scp 10.10.10.21:/etc/apache2/sites-available/jjongmarket.conf /etc/apache2/sites-available/
rm -f /var/www/jjongmarket/index.html
mkdir -p /etc/jjongmarket
scp 10.10.10.21:/etc/jjongmarket/db.ini /etc/jjongmarket/
chmod 755 /etc/jjongmarket
chown root:www-data /etc/jjongmarket/db.ini
chmod 640 /etc/jjongmarket/db.ini
chmod -R a+rX /var/www/jjongmarket
```

### [ WEB1, WEB2 ]

```bash
a2dissite 000-default
a2ensite jjongmarket
hostname > /var/www/jjongmarket/health.html
apache2ctl configtest
systemctl restart apache2
```

### [ CLIENT, REMOTE ]

```bash
apt install -y fonts-noto-cjk ibus-hangul
```

在 Firefox 中导入并信任 RootCA 后访问 `https://www.jjongmarket.kr`。演示账号: `demo@jjongmarket.kr` / `Jjong1234!`

> 从 Windows 用 scp 发送的文件夹权限可能变成 700。忘记执行 `chmod -R a+rX` 会导致 500 错误。
