# 06. Certificate Authority

[한국어](../../../Session/06-certificate-authority.md) | [English](../../en/Session/06-certificate-authority.md) | [日本語](../../ja/Session/06-certificate-authority.md) | [简体中文](../../zh-CN/Session/06-certificate-authority.md) | [Español](../../es/Session/06-certificate-authority.md) | **Français**

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

- Appuyez d'abord sur Entrée, saisissez le mot de passe de la clé de la CA, et laissez le reste par défaut
- `Common Name []: RootCA`

```bash
cp /etc/ssl/RootCA/cacert.pem /etc/ssl/RootCA.crt
```

### [ All Devices except MGMT ]

```bash
scp 10.10.30.10:/etc/ssl/RootCA.crt /usr/local/share/ca-certificates
```

> Si scp est bloqué par le nftables du FW, lancez `systemctl stop nftables` sur le FW, faites le travail, puis rétablissez-le avec `systemctl start nftables`.

### [ All Devices ]

```bash
update-ca-certificates
```

Firefox utilise son propre magasin de certificats et non la liste du système : il faut donc importer la RootCA séparément dans le Firefox de CLIENT et REMOTE.
