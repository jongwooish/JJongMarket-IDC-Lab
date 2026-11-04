# 07. DNS

[한국어](../../../Session/07-dns.md) | **English** | [日本語](../../ja/Session/07-dns.md) | [简体中文](../../zh-CN/Session/07-dns.md) | [Español](../../es/Session/07-dns.md) | [Français](../../fr/Session/07-dns.md)

### [ DNS ]

```bash
apt install -y bind9 bind9-doc lynx gpm
cd /etc/bind/
vim named.conf.options
```

```
// /etc/bind/named.conf.options  (DNS)
options {
        directory "/var/cache/bind";
        listen-on { any; };
        listen-on-v6 { none; };
        allow-query { any; };
        recursion no;              // closed network: answer own zones only
        dnssec-validation no;
        version none;
};
```

```bash
vim named.conf
```

```
// /etc/bind/named.conf  (DNS)
include "/etc/bind/named.conf.options";
// default zones are not loaded: every zone must live inside a view
//include "/etc/bind/named.conf.local";
//include "/etc/bind/named.conf.default-zones";

view internal {
        match-clients { 10.10.0.0/16; 10.8.0.0/24; 127.0.0.1; };

        zone "jjong.internal" {
                type master;
                file "jjong.internal.zone";
        };
        zone "jjongmarket.kr" {
                type master;
                file "jjongmarket.kr.zone";
        };
        zone "10.10.10.in-addr.arpa" {
                type master;
                file "server.zone";
        };
        zone "20.10.10.in-addr.arpa" {
                type master;
                file "db.zone";
        };
        zone "30.10.10.in-addr.arpa" {
                type master;
                file "mgmt.zone";
        };
};

view external {
        match-clients { any; };

        zone "jjongmarket.kr" {
                type master;
                file "jjongmarket.kr.ext.zone";
        };
};
```

```bash
named-checkconf
```

The zone file templates are copied from the ARM documentation in bind9-doc.

```bash
lynx /usr/share/doc/bind9-doc/arm/index.html
```

- Copy the 3.1.4 example with gpm into `/etc/bind/db.local` and the 3.1.5 example into `/etc/bind/db.127`, then clean up the leading line numbers.

```bash
apt autoremove --purge gpm
cp db.local /var/cache/bind/jjong.internal.zone
cp db.local /var/cache/bind/jjongmarket.kr.zone
cp db.local /var/cache/bind/jjongmarket.kr.ext.zone
cp db.127   /var/cache/bind/server.zone
cp db.127   /var/cache/bind/db.zone
cp db.127   /var/cache/bind/mgmt.zone
cd /var/cache/bind
```

For forward zones, rename the names in vim and then fill in the records.

```
:%s/ns1.example.com/ns.jjongmarket.kr
:%s/hostmaster.example.com/admin.jjongmarket.kr
```

For reverse zones, change them like this.

```
:%s/localhost/ns.jjong.internal
:%s/root.localhost/admin.jjong.internal
```

```bash
vim jjong.internal.zone
```

```
$TTL    604800
@   IN  SOA  ns.jjong.internal. admin.jjong.internal. (
            2026101501  ; serial
            3600        ; refresh
            900         ; retry
            604800      ; expire
            300 )       ; negative cache TTL
@    IN  NS   ns.jjong.internal.
ns   IN  A    10.10.10.10
```

```bash
vim jjongmarket.kr.zone
```

```
$TTL    604800
@   IN  SOA  ns.jjongmarket.kr. admin.jjongmarket.kr. (
            2026101501  ; serial
            3600        ; refresh
            900         ; retry
            604800      ; expire
            300 )       ; negative cache TTL
@    IN  NS   ns.jjongmarket.kr.
ns   IN  A    10.10.10.10
@    IN  A    10.10.10.100
www  IN  A    10.10.10.100
vpn  IN  A    203.0.113.10
```

```bash
vim jjongmarket.kr.ext.zone
```

```
$TTL    604800
@   IN  SOA  ns.jjongmarket.kr. admin.jjongmarket.kr. (
            2026101501  ; serial
            3600        ; refresh
            900         ; retry
            604800      ; expire
            300 )       ; negative cache TTL
@    IN  NS   ns.jjongmarket.kr.
ns   IN  A    203.0.113.10
@    IN  A    203.0.113.10
www  IN  A    203.0.113.10
vpn  IN  A    203.0.113.10
```

```bash
vim server.zone
```

```
$TTL    604800
@   IN  SOA  ns.jjong.internal. admin.jjong.internal. (
            2026101501  ; serial
            3600        ; refresh
            900         ; retry
            604800      ; expire
            300 )       ; negative cache TTL
@    IN  NS   ns.jjong.internal.
10   IN  PTR  dns.jjong.internal.
21   IN  PTR  web1.jjong.internal.
22   IN  PTR  web2.jjong.internal.
100  IN  PTR  web.jjong.internal.
254  IN  PTR  fw.jjong.internal.
```

```bash
vim db.zone
```

```
$TTL    604800
@   IN  SOA  ns.jjong.internal. admin.jjong.internal. (
            2026101501  ; serial
            3600        ; refresh
            900         ; retry
            604800      ; expire
            300 )       ; negative cache TTL
@    IN  NS   ns.jjong.internal.
10   IN  PTR  db.jjong.internal.
254  IN  PTR  fw.jjong.internal.
```

```bash
vim mgmt.zone
```

```
$TTL    604800
@   IN  SOA  ns.jjong.internal. admin.jjong.internal. (
            2026101501  ; serial
            3600        ; refresh
            900         ; retry
            604800      ; expire
            300 )       ; negative cache TTL
@    IN  NS   ns.jjong.internal.
10   IN  PTR  mgmt.jjong.internal.
100  IN  PTR  client.jjong.internal.
254  IN  PTR  fw.jjong.internal.
```

```bash
chown root:bind /etc/bind/named.conf*
chown bind:bind /var/cache/bind/*.zone
systemctl restart bind9
```

### [ DNS ]

```bash
echo 'nameserver 127.0.0.1' > /etc/resolv.conf
```

### [ FW, WEB1, WEB2, DB, MGMT ]

```bash
echo 'nameserver 10.10.10.10' > /etc/resolv.conf
```
