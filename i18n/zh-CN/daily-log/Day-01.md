# Day 01 - 2026.10.14 (周三) 策划

[한국어](../../../daily-log/Day-01.md) | [English](../../en/daily-log/Day-01.md) | [日本語](../../ja/daily-log/Day-01.md) | **简体中文** | [Español](../../es/daily-log/Day-01.md) | [Français](../../fr/daily-log/Day-01.md)

## 今天做了什么

- 实习期间第一次外出，去瑞草洞的 SK Broadband IDC 安装服务器
- 下午回到办公室，确定了个人项目的方向
- 订阅了 Claude Pro，创建了项目用的 Claude 项目“쫑(JJong)”，并关联了 Notion 资料(IT Network System)

## 为什么是 IDC

第一次走进 IDC。亲眼看到机柜里插着服务器、防火墙后面 Web 服务器和数据库被分开，我才真正体会到，过去在技能竞赛训练中按题目分别搭建的那些服务，在实际中是连在一起共同运行的。
所以我决定不再“做题”，而是从头到尾搭建“一家公司的基础设施”。

## 策划书

### 背景

JJongMarket 股份有限公司是一家约 20 名员工的网上商城。由于云费用不断上涨，公司决定租用一个 IDC 机柜自行运营服务器。我是基础设施负责人。

### 需求

| ID | 需求 | 实现方式 |
| --- | --- | --- |
| R1 | 从互联网通过 `https://www.jjongmarket.kr` 访问商城 | FW DNAT → Web VIP → Apache |
| R2 | 一台 Web 服务器宕机时商城仍可用 | keepalived VRRP |
| R3 | 只开放必要端口 | nftables 默认拒绝 |
| R4 | 服务器只能从管理网管理，远程办公走 VPN | VPN → 仅允许 MGMT |
| R5 | 内外网返回不同的 DNS 应答 | BIND9 view |
| R6 | 商品数据的保存与备份 | MariaDB + 每日转储 + 保存在另一台服务器 |
| R7 | 在一个页面查看状态 | Prometheus + Grafana |
| R8 | 让下一任负责人能重新搭建 | 文档 + Ansible |
| R9 | 无 HTTPS 警告 | 私有 Root CA |
| R10 | 所有服务器时间一致 | chrony |

### 约束

- VMware Workstation、Debian 13.2、8 台 VM (6 台 CLI 服务器、2 台 GUI 客户端)
- 只使用 Host-only 网络。假设没有互联网，只在安装软件包时临时挂上 NAT 网卡
- 不能像竞赛练习那样“能跑就行”，必须能解释为什么这样做

## 感想

第一次用 AI 做项目。我决定不是照抄答案，而是把不懂的地方问清楚，自己理解之后再亲手输入。
