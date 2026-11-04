# 14. WEB

[한국어](../../../Session/14-web.md) | [English](../../en/Session/14-web.md) | **日本語** | [简体中文](../../zh-CN/Session/14-web.md) | [Español](../../es/Session/14-web.md) | [Français](../../fr/Session/14-web.md)

### [ MGMT ] Web 証明書の発行

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

`/etc/hosts` の `127.0.1.1` をそれぞれ WEB1、WEB2 に直してからインストールします。

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

WEB1 に NAT アダプターを一時的に付け、ホストPCからショップのファイルを送ります。

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
#  /etc/apache2/sites-available/jjongmarket.conf  (WEB1、WEB2 共通)
#  必要なモジュール: ssl, rewrite, headers  (php は libapache2-mod-php のインストール時に自動で有効化)
# =====================================================================

# ---- HTTP(80): HTTPS へリダイレクト。ただし下の2つのパスは HTTP でも応答 ----
#   /health.html   : keepalived のヘルスチェック用
#   /ca.crt        : 顧客PCがプライベート Root CA をダウンロードするアドレス
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

# ---- HTTPS(443): ショップ本サービス ----
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
    # チョンライブ — このサーバーの ffmpeg(jjonglive.service)が作った HLS を同じ HTTPS アドレス(/hls/)で配信 (a2enmod headers)
    Alias /hls/ /var/www/hls/
    <Directory /var/www/hls>
        Require all granted
        AddType application/vnd.apple.mpegurl .m3u8
        AddType video/mp2t .ts
        Header set Cache-Control "no-cache"
    </Directory>

    # 共通 PHP(DB接続コード)はブラウザから直接開けないように
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

Firefox で RootCA を信頼するようにインポートしてから `https://www.jjongmarket.kr` にアクセス。デモアカウント: `demo@jjongmarket.kr` / `Jjong1234!`

> Windows から scp で送ったフォルダーは権限が 700 になることがあります。`chmod -R a+rX` を忘れると 500 エラーになります。
