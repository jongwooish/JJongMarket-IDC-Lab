# Day 04 - 2026.10.19 (周一) DHCP、nftables

[한국어](../../../daily-log/Day-04.md) | [English](../../en/daily-log/Day-04.md) | [日本語](../../ja/daily-log/Day-04.md) | **简体中文** | [Español](../../es/daily-log/Day-04.md) | [Français](../../fr/daily-log/Day-04.md)

## 今天做了什么

- 在 MGMT 上安装 isc-dhcp-server，确认 CLIENT 获取到 10.10.30.100~150
- REMOTE 在另一个网段，广播到不了 MGMT → 在 FW 上用 dnsmasq 做 DHCP 中继
- 编写 nftables 规则 (默认 drop，只允许必要的流量，外部 80·443·53 做 DNAT)

## 学到的东西

- 经过中继时，DHCP 服务器会根据数据包中的中继地址(giaddr)选择从哪个子网分配地址。所以 dhcpd.conf 中必须有 203.0.113.0 子网。
- REMOTE 的地址池里我故意没有写 `option routers`。没有网关，不连 VPN 就根本没有通往内网的路径。
- 在做 nftables 时，有几次被自己写的规则挡住，scp 无法使用。于是养成了只在操作时 `systemctl stop nftables`、结束后再打开的习惯。
