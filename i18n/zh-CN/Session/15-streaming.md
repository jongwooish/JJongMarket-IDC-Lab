# 15. Streaming

[한국어](../../../Session/15-streaming.md) | [English](../../en/Session/15-streaming.md) | [日本語](../../ja/Session/15-streaming.md) | **简体中文** | [Español](../../es/Session/15-streaming.md) | [Français](../../fr/Session/15-streaming.md)

### [ WEB1 ]

```bash
apt install -y ffmpeg
```

添加 NAT 网卡，从宿主机发送视频、网页文件和 hls.js。
`hls.min.js` 未放入仓库，请从 [jsDelivr](https://cdn.jsdelivr.net/npm/hls.js@1/dist/hls.min.js) 下载。

```powershell
scp .\live\jjonglive.mp4 root@<WEB1_NAT_IP>:/root/
scp -r .\web root@<WEB1_NAT_IP>:/root/
scp .\hls.min.js root@<WEB1_NAT_IP>:/root/
```

```bash
mkdir -p /opt/jjonglive
mv /root/jjonglive.mp4 /opt/jjonglive/
vim /etc/systemd/system/jjonglive.service
```

```
# =====================================================================
#  /etc/systemd/system/jjonglive.service (WEB1, WEB2)
#  无限循环播放 1 分钟的视频，切成 2 秒的 HLS 分片并保存到 /var/www/hls
#  → 由 Apache 通过 https://www.jjongmarket.kr/hls/ 对外提供
# =====================================================================
[Unit]
Description=JJONG LIVE - loop broadcast (HLS)
After=network.target

[Service]
ExecStartPre=/bin/mkdir -p /var/www/hls
ExecStart=/usr/bin/ffmpeg -nostdin -loglevel warning -re -stream_loop -1 -i /opt/jjonglive/jjonglive.mp4 -c copy -f hls -hls_time 2 -hls_list_size 5 -hls_flags delete_segments+omit_endlist /var/www/hls/stream.m3u8
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl enable --now jjonglive

cp -r /root/web/* /var/www/jjongmarket/
mkdir -p /var/www/jjongmarket/js
cp /root/hls.min.js /var/www/jjongmarket/js/
chmod -R a+rX /var/www/jjongmarket
a2enmod headers
vim /etc/apache2/sites-available/jjongmarket.conf
```

在 `<VirtualHost *:443>` 中添加:

```apache
    Alias /hls/ /var/www/hls/
    <Directory /var/www/hls>
        Require all granted
        AddType application/vnd.apple.mpegurl .m3u8
        AddType video/mp2t .ts
        Header set Cache-Control "no-cache"
    </Directory>
```

```bash
apache2ctl configtest
systemctl restart apache2
```

### [ WEB2 ]

```bash
apt install -y ffmpeg
mkdir -p /opt/jjonglive
scp 10.10.10.21:/opt/jjonglive/jjonglive.mp4 /opt/jjonglive/
scp 10.10.10.21:/etc/systemd/system/jjonglive.service /etc/systemd/system/
scp -r 10.10.10.21:/var/www/jjongmarket/* /var/www/jjongmarket/
scp 10.10.10.21:/etc/apache2/sites-available/jjongmarket.conf /etc/apache2/sites-available/
chmod -R a+rX /var/www/jjongmarket
hostname > /var/www/jjongmarket/health.html
systemctl daemon-reload
systemctl enable --now jjonglive
a2enmod headers
systemctl restart apache2
```
