# 14. WEB

[한국어](../../../Session/14-web.md) | [English](../../en/Session/14-web.md) | [日本語](../../ja/Session/14-web.md) | [简体中文](../../zh-CN/Session/14-web.md) | [Español](../../es/Session/14-web.md) | **Français**

### [ MGMT ] Émission du certificat web

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

Remplacez `127.0.1.1` dans `/etc/hosts` par WEB1 et WEB2 respectivement, puis installez.

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

Branchez brièvement un adaptateur NAT sur WEB1 et envoyez les fichiers de la boutique depuis le PC hôte.

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
#  /etc/apache2/sites-available/jjongmarket.conf  (identique sur WEB1 et WEB2)
#  Modules requis : ssl, rewrite, headers  (php est activé automatiquement à l'installation de libapache2-mod-php)
# =====================================================================

# ---- HTTP(80) : redirection vers HTTPS, mais ces deux chemins répondent aussi en HTTP ----
#   /health.html   : pour le contrôle de santé de keepalived
#   /ca.crt        : adresse où les PC clients téléchargent la Root CA privée
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

# ---- HTTPS(443) : service principal de la boutique ----
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
    # JJong LIVE — sert le HLS produit par le ffmpeg de ce serveur (jjonglive.service) à la même adresse HTTPS (/hls/) (a2enmod headers)
    Alias /hls/ /var/www/hls/
    <Directory /var/www/hls>
        Require all granted
        AddType application/vnd.apple.mpegurl .m3u8
        AddType video/mp2t .ts
        Header set Cache-Control "no-cache"
    </Directory>

    # Le PHP commun (code de connexion à la base) ne doit pas être ouvert directement dans un navigateur
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

Importez la RootCA dans Firefox comme autorité de confiance, puis ouvrez `https://www.jjongmarket.kr`. Compte de démonstration : `demo@jjongmarket.kr` / `Jjong1234!`

> Les dossiers envoyés depuis Windows avec scp peuvent se retrouver en permissions 700. Si vous oubliez `chmod -R a+rX`, vous obtiendrez une erreur 500.
