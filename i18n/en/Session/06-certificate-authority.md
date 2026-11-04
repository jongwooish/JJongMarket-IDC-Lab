# 06. Certificate Authority

[한국어](../../../Session/06-certificate-authority.md) | **English** | [日本語](../../ja/Session/06-certificate-authority.md) | [简体中文](../../zh-CN/Session/06-certificate-authority.md) | [Español](../../es/Session/06-certificate-authority.md) | [Français](../../fr/Session/06-certificate-authority.md)

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

- Press Enter first, enter the CA key password, and keep the defaults for the rest
- `Common Name []: RootCA`

```bash
cp /etc/ssl/RootCA/cacert.pem /etc/ssl/RootCA.crt
```

### [ All Devices except MGMT ]

```bash
scp 10.10.30.10:/etc/ssl/RootCA.crt /usr/local/share/ca-certificates
```

> If scp is blocked by the FW's nftables, run `systemctl stop nftables` on the FW, do the work, and restore it with `systemctl start nftables`.

### [ All Devices ]

```bash
update-ca-certificates
```

Firefox uses its own certificate store, not the system list, so import the RootCA into Firefox on CLIENT and REMOTE separately.
