# Day 06 - 2026.10.21 (周三) WireGuard、SSH CA、chrony

[한국어](../../../daily-log/Day-06.md) | [English](../../en/daily-log/Day-06.md) | [日本語](../../ja/daily-log/Day-06.md) | **简体中文** | [Español](../../es/daily-log/Day-06.md) | [Français](../../fr/daily-log/Day-06.md)

## 今天做了什么

- 在 FW 上配置 wg0 (10.8.0.1)，REMOTE 用 `nmcli connection import` 连接隧道
- 在 MGMT 上创建 SSH CA，并为 root 密钥签名
- 以 DNS 作为时间基准(local stratum 8)，其余所有设备都与 DNS 同步

## 学到的东西

- WireGuard 的 `AllowedIPs` 在不同方向上含义不同。FW 端表示“从这个对端接收的地址”，REMOTE 端表示“要送进隧道的目的地”。REMOTE 上只写了 `10.10.0.0/16`，所以只有内网流量走隧道。
- 内部服务器的默认网关是 FW，所以发往 10.8.0.2 的回包即使不额外加路由，也会经 FW → wg0 返回。
- 使用 SSH CA 后，不需要在每台服务器上放公钥，只要信任一个 CA 公钥即可。不过，整体复制 `/etc/ssl` 导致 CA 私钥也被扩散出去，这是后来检查时才发现的。
- 因为没有互联网，必须注释掉 chrony 默认的 `pool` 行，否则它会一直尝试连接外部。
