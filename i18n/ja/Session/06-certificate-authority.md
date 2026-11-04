# 06. Certificate Authority

[한국어](../../../Session/06-certificate-authority.md) | [English](../../en/Session/06-certificate-authority.md) | **日本語** | [简体中文](../../zh-CN/Session/06-certificate-authority.md) | [Español](../../es/Session/06-certificate-authority.md) | [Français](../../fr/Session/06-certificate-authority.md)

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

- 最初に Enter、CA キーのパスワードを入力し、残りはデフォルト値
- `Common Name []: RootCA`

```bash
cp /etc/ssl/RootCA/cacert.pem /etc/ssl/RootCA.crt
```

### [ All Devices except MGMT ]

```bash
scp 10.10.30.10:/etc/ssl/RootCA.crt /usr/local/share/ca-certificates
```

> FW の nftables のせいで scp がブロックされる場合は、FW で `systemctl stop nftables` してから作業し、`systemctl start nftables` で元に戻します。

### [ All Devices ]

```bash
update-ca-certificates
```

Firefox はシステムの一覧ではなく独自の証明書ストアを使うため、CLIENT、REMOTE の Firefox には RootCA を別途インポートします。
