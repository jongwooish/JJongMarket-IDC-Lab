# 14. WEB

[한국어](../../../Session/14-web.md) | [English](../../en/Session/14-web.md) | [日本語](../../ja/Session/14-web.md) | [简体中文](../../zh-CN/Session/14-web.md) | **Español** | [Français](../../fr/Session/14-web.md)

### [ MGMT ] Emitir el certificado web

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

Cambia `127.0.1.1` en `/etc/hosts` a WEB1 y WEB2 respectivamente y luego instala.

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

Conecta temporalmente un adaptador NAT a WEB1 y envía los archivos de la tienda desde el PC anfitrión.

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
#  /etc/apache2/sites-available/jjongmarket.conf  (igual en WEB1 y WEB2)
#  Módulos necesarios: ssl, rewrite, headers  (php se activa solo al instalar libapache2-mod-php)
# =====================================================================

# ---- HTTP(80): redirige a HTTPS, pero estas dos rutas también responden por HTTP ----
#   /health.html   : para la comprobación de estado de keepalived
#   /ca.crt        : dirección donde los PC de los clientes descargan la Root CA privada
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

# ---- HTTPS(443): servicio principal de la tienda ----
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
    # JJong LIVE — sirve el HLS generado por el ffmpeg de este servidor (jjonglive.service) en la misma dirección HTTPS (/hls/) (a2enmod headers)
    Alias /hls/ /var/www/hls/
    <Directory /var/www/hls>
        Require all granted
        AddType application/vnd.apple.mpegurl .m3u8
        AddType video/mp2t .ts
        Header set Cache-Control "no-cache"
    </Directory>

    # El PHP común (código de conexión a la BD) no se puede abrir directamente desde el navegador
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

Importa la RootCA en Firefox como de confianza y abre `https://www.jjongmarket.kr`. Cuenta de demostración: `demo@jjongmarket.kr` / `Jjong1234!`

> Las carpetas enviadas desde Windows con scp pueden quedar con permisos 700. Si olvidas `chmod -R a+rX`, obtendrás un error 500.
