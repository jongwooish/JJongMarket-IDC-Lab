# 11. MariaDB

[한국어](../../../Session/11-mariadb.md) | [English](../../en/Session/11-mariadb.md) | [日本語](../../ja/Session/11-mariadb.md) | **简体中文** | [Español](../../es/Session/11-mariadb.md) | [Français](../../fr/Session/11-mariadb.md)

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

给 DB 临时添加 NAT 网卡获取 IP 后，从宿主机的 PowerShell 发送。

```powershell
scp .\configs\mariadb\shop.sql root@<DB_NAT_IP>:/root/
```

之后再移除 NAT 网卡。

### [ DB ]

```bash
mariadb < /root/shop.sql
```

> 在控制台中粘贴韩文会出现乱码，所以 SQL 一定要以文件形式传输。
