# 09. SSH

[한국어](../../../Session/09-ssh.md) | [English](../../en/Session/09-ssh.md) | [日本語](../../ja/Session/09-ssh.md) | [简体中文](../../zh-CN/Session/09-ssh.md) | **Español** | [Français](../../fr/Session/09-ssh.md)

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

> Copiar todo `/etc/ssl` también copia la **clave privada** de la CA (`ssh-CA/ca`). Cómo limpiarlo está en [Verification - Revisión de seguridad](../docs/verification.md#revisión-de-seguridad).
