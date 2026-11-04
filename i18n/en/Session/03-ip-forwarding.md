# 03. IP Forwarding

[한국어](../../../Session/03-ip-forwarding.md) | **English** | [日本語](../../ja/Session/03-ip-forwarding.md) | [简体中文](../../zh-CN/Session/03-ip-forwarding.md) | [Español](../../es/Session/03-ip-forwarding.md) | [Français](../../fr/Session/03-ip-forwarding.md)

### [ FW ]

```bash
echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
sysctl -p
vim /etc/rc.local
```

```bash
#!/bin/bash
sysctl -p
```

```bash
chmod +x /etc/rc.local
```
