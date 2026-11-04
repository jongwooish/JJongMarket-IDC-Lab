# 09. SSH

[한국어](../../../Session/09-ssh.md) | [English](../../en/Session/09-ssh.md) | [日本語](../../ja/Session/09-ssh.md) | [简体中文](../../zh-CN/Session/09-ssh.md) | [Español](../../es/Session/09-ssh.md) | **Français**

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

> Copier tout `/etc/ssl` copie aussi la **clé privée** de la CA (`ssh-CA/ca`). La façon de corriger cela se trouve dans [Verification - Revue de sécurité](../docs/verification.md#revue-de-sécurité).
