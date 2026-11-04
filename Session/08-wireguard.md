# 08. Wireguard

**한국어** | [English](../i18n/en/Session/08-wireguard.md) | [日本語](../i18n/ja/Session/08-wireguard.md) | [简体中文](../i18n/zh-CN/Session/08-wireguard.md) | [Español](../i18n/es/Session/08-wireguard.md) | [Français](../i18n/fr/Session/08-wireguard.md)

### [ FW ]

```bash
apt install -y wireguard
cd /etc/wireguard/
wg genkey | tee server-privatekey | wg pubkey > server-publickey
wg genkey | tee client-privatekey | wg pubkey > client-publickey
cat client-publickey server-privatekey >> wg0.conf
vim wg0.conf
```

```ini
[Interface]
Address = 10.8.0.1/24
PrivateKey = <server-privatekey>
ListenPort = 51820

[Peer]
PublicKey = <client-publickey>
AllowedIPs = 10.8.0.2/32
```

```bash
systemctl enable --now wg-quick@wg0.service
```

### [ REMOTE ]

```bash
apt install -y wireguard
scp 203.0.113.10:/etc/wireguard/* /etc/wireguard
cd /etc/wireguard/
cat server-publickey client-privatekey >> /etc/wireguard/wg0.conf
vim wg0.conf
```

```ini
[Interface]
Address = 10.8.0.2/24
PrivateKey = <client-privatekey>
DNS = 10.10.10.10, jjong.internal

[Peer]
PublicKey = <server-publickey>
Endpoint = 203.0.113.10:51820
AllowedIPs = 10.10.0.0/16, 10.8.0.0/24
PersistentKeepalive = 25
```

```bash
nmcli connection import type wireguard file /etc/wireguard/wg0.conf
```

> scp가 막히면 FW에서 `systemctl stop nftables` 후 작업하고 다시 `start` 합니다.
