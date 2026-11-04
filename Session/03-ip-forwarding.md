# 03. IP Forwarding

**한국어** | [English](../i18n/en/Session/03-ip-forwarding.md) | [日本語](../i18n/ja/Session/03-ip-forwarding.md) | [简体中文](../i18n/zh-CN/Session/03-ip-forwarding.md) | [Español](../i18n/es/Session/03-ip-forwarding.md) | [Français](../i18n/fr/Session/03-ip-forwarding.md)

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
