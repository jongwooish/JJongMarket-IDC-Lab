# 06. Certificate Authority

[한국어](../../../Session/06-certificate-authority.md) | [English](../../en/Session/06-certificate-authority.md) | [日本語](../../ja/Session/06-certificate-authority.md) | [简体中文](../../zh-CN/Session/06-certificate-authority.md) | **Español** | [Français](../../fr/Session/06-certificate-authority.md)

### [ MGMT ]

```bash
vim /etc/ssl/openssl.cnf
```

```
dir             = /etc/ssl/RootCA
...
x509_extensions = v3_req
...
policy          = policy_anything

[ req_distinguished_name ]
countryName_default          = KR
stateOrProvinceName_default  =
0.organizationName_default   =
```

```bash
vim /usr/lib/ssl/misc/CA.pl
```

```perl
my $CATOP = "/etc/ssl/RootCA";
```

```bash
/usr/lib/ssl/misc/CA.pl -newca
```

- Pulsa Enter primero, introduce la contraseña de la clave de la CA y deja el resto por defecto
- `Common Name []: RootCA`

```bash
cp /etc/ssl/RootCA/cacert.pem /etc/ssl/RootCA.crt
```

### [ All Devices except MGMT ]

```bash
scp 10.10.30.10:/etc/ssl/RootCA.crt /usr/local/share/ca-certificates
```

> Si el nftables del FW bloquea scp, ejecuta `systemctl stop nftables` en el FW, haz el trabajo y restáuralo con `systemctl start nftables`.

### [ All Devices ]

```bash
update-ca-certificates
```

Firefox usa su propio almacén de certificados, no la lista del sistema, así que hay que importar la RootCA por separado en el Firefox de CLIENT y REMOTE.
