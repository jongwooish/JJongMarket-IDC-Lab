# Day 05 - 2026.10.20 (周二) CA、DNS、更换 VPN

[한국어](../../../daily-log/Day-05.md) | [English](../../en/daily-log/Day-05.md) | [日本語](../../ja/daily-log/Day-05.md) | **简体中文** | [Español](../../es/daily-log/Day-05.md) | [Français](../../fr/daily-log/Day-05.md)

## 今天做了什么

- 在 MGMT 上建立私有 Root CA (`CA.pl -newca`)，在所有设备上将 RootCA.crt 登记为受信任
- 用两个 BIND9 view(internal / external)实现 split-horizon DNS
- 将 VPN 从 OpenVPN 更换为 **WireGuard**
- 开始在 Notion 的 Session 页签中按章节折叠整理 Configuration

## 为什么换成 WireGuard

OpenVPN 需要服务器和客户端证书、tls-crypt 密钥，配置文件也很多。WireGuard 只要双方知道彼此的公钥即可，配置也就十来行。这个项目的目标是“安全进入管理网的通道”，所以选择了更简单的方案。

## 学到的东西

- 外部查询由 FW DNAT 到 DNS，但源地址不变。因此 DNS 会把该查询识别为外部请求，并用 external view 应答。一开始我以为做了 DNAT 之后看起来都会像是来自内部。
- 因为设置了 `recursion no`，从外部查询 `jjong.internal` 会返回 REFUSED，内部名称不会泄露到外面。
- zone 文件模板是用 gpm 从 bind9-doc 的 ARM 文档中复制的，和比赛时的做法一样。

## 感想

和做题不同，按实际工作的方式搭建时错误要多得多。但也因此不断追问“为什么”，学到的更多。
