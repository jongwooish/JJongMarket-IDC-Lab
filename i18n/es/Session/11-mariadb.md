# 11. MariaDB

[한국어](../../../Session/11-mariadb.md) | [English](../../en/Session/11-mariadb.md) | [日本語](../../ja/Session/11-mariadb.md) | [简体中文](../../zh-CN/Session/11-mariadb.md) | **Español** | [Français](../../fr/Session/11-mariadb.md)

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

Conecta temporalmente un adaptador NAT a DB para obtener una IP y envía el archivo desde PowerShell en el PC anfitrión.

```powershell
scp .\configs\mariadb\shop.sql root@<DB_NAT_IP>:/root/
```

Después vuelve a quitar el adaptador NAT.

### [ DB ]

```bash
mariadb < /root/shop.sql
```

> Pegar texto coreano en la consola lo corrompe, así que transfiere siempre el SQL como archivo.
