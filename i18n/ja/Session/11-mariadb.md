# 11. MariaDB

[한국어](../../../Session/11-mariadb.md) | [English](../../en/Session/11-mariadb.md) | **日本語** | [简体中文](../../zh-CN/Session/11-mariadb.md) | [Español](../../es/Session/11-mariadb.md) | [Français](../../fr/Session/11-mariadb.md)

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

DB に NAT アダプターを一時的に付けて IP を取得し、ホストPCの PowerShell から送ります。

```powershell
scp .\configs\mariadb\shop.sql root@<DB_NAT_IP>:/root/
```

NAT アダプターは再び取り外します。

### [ DB ]

```bash
mariadb < /root/shop.sql
```

> コンソールにハングルを貼り付けると文字化けするため、SQL は必ずファイルで転送します。
