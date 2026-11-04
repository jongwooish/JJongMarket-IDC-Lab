# 11. MariaDB

[한국어](../../../Session/11-mariadb.md) | [English](../../en/Session/11-mariadb.md) | [日本語](../../ja/Session/11-mariadb.md) | [简体中文](../../zh-CN/Session/11-mariadb.md) | [Español](../../es/Session/11-mariadb.md) | **Français**

### [ DB ]

```bash
apt install -y mariadb-server
systemctl enable --now mariadb
vim /etc/mysql/mariadb.conf.d/60-jjong.cnf
```

```ini
[mysqld]
bind-address         = 10.10.20.10
character-set-server = utf8mb4
collation-server     = utf8mb4_unicode_ci
```

```bash
systemctl restart mariadb
```

### [ Host PC ] → [ DB ]

Branchez brièvement un adaptateur NAT sur DB pour obtenir une IP, puis envoyez le fichier depuis PowerShell sur le PC hôte.

```powershell
scp .\configs\mariadb\shop.sql root@<DB_NAT_IP>:/root/
```

Retirez ensuite l'adaptateur NAT.

### [ DB ]

```bash
mariadb < /root/shop.sql
```

> Coller du coréen dans la console le corrompt : transférez toujours le SQL sous forme de fichier.
