# Day 02 - 2026.10.15 (周四) 设计

[한국어](../../../daily-log/Day-02.md) | [English](../../en/daily-log/Day-02.md) | [日本語](../../ja/daily-log/Day-02.md) | **简体中文** | [Español](../../es/daily-log/Day-02.md) | [Français](../../fr/daily-log/Day-02.md)

## 今天做了什么

- 按用途把网络分成 4 个: OUTSIDE / SERVER / DB / MGMT
- 确定地址规则: 每个网段的 `.254` 是 FW，服务器为 `.10` `.21` `.22`，VIP 和客户端为 `.100`
- 画了拓扑图，写了 README 初稿，在 Notion 中创建了 JJongMarket IDC 页面

## 设计时考虑的问题

- **为什么把数据库单独分开**: 为了让 Web 服务器被攻破时也无法直接接触数据库。FW 上只需开放 SERVER 网段 → DB 3306。
- **203.0.113.0/24**: 这是为文档保留的地址段(RFC 5737)，不会与真实互联网地址冲突，正好适合扮演外部网络。
- **VPN**: 最初计划使用 OpenVPN。(在 Day 05 更换)

![topology](../../../docs/images/topology.png)

## 明天要做的事

创建 VM 并配置 IP
