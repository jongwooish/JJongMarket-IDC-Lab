# 06. Certificate Authority

[한국어](../../../Session/06-certificate-authority.md) | [English](../../en/Session/06-certificate-authority.md) | [日本語](../../ja/Session/06-certificate-authority.md) | **简体中文** | [Español](../../es/Session/06-certificate-authority.md) | [Français](../../fr/Session/06-certificate-authority.md)

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

- 先按 Enter，输入 CA 密钥密码，其余保持默认值
- `Common Name []: RootCA`

```bash
cp /etc/ssl/RootCA/cacert.pem /etc/ssl/RootCA.crt
```

### [ All Devices except MGMT ]

```bash
scp 10.10.30.10:/etc/ssl/RootCA.crt /usr/local/share/ca-certificates
```

> 如果 scp 被 FW 的 nftables 拦截，先在 FW 上执行 `systemctl stop nftables` 再操作，完成后用 `systemctl start nftables` 恢复。

### [ All Devices ]

```bash
update-ca-certificates
```

Firefox 使用自己的证书库而非系统列表，因此需要在 CLIENT、REMOTE 的 Firefox 中单独导入 RootCA。
