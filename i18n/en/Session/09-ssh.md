# 09. SSH

[한국어](../../../Session/09-ssh.md) | **English** | [日本語](../../ja/Session/09-ssh.md) | [简体中文](../../zh-CN/Session/09-ssh.md) | [Español](../../es/Session/09-ssh.md) | [Français](../../fr/Session/09-ssh.md)

### [ FW ]

```bash
systemctl stop nftables
```

### [ MGMT ]

```bash
mkdir /etc/ssl/ssh-CA
ssh-keygen -f /etc/ssl/ssh-CA/ca -t ecdsa
ssh-keygen -t ecdsa
ssh-keygen -s /etc/ssl/ssh-CA/ca -I "jjong root key" -n root -V +52w /root/.ssh/id_ecdsa.pub
vim /etc/ssh/sshd_config
```

```
TrustedUserCAKeys /etc/ssl/ssh-CA/ca.pub
```

```bash
systemctl restart sshd
```

### [ All Devices except MGMT ]

```bash
scp -r 10.10.30.10:/etc/ssl /etc
```

### [ FW ]

```bash
systemctl start nftables
systemctl restart nftables
```

> Copying all of `/etc/ssl` also copies the CA **private key** (`ssh-CA/ca`). How to clean it up is in [Verification - Security Review](../docs/verification.md#security-review).
