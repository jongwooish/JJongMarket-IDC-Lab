# 11. MariaDB

[한국어](../../../Session/11-mariadb.md) | **English** | [日本語](../../ja/Session/11-mariadb.md) | [简体中文](../../zh-CN/Session/11-mariadb.md) | [Español](../../es/Session/11-mariadb.md) | [Français](../../fr/Session/11-mariadb.md)

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

Briefly attach a NAT adapter to DB to get an IP, then send the file from PowerShell on the host PC.

```powershell
scp .\configs\mariadb\shop.sql root@<DB_NAT_IP>:/root/
```

Remove the NAT adapter again afterwards.

### [ DB ]

```bash
mariadb < /root/shop.sql
```

> Pasting Korean text into the console breaks it, so always transfer the SQL as a file.
