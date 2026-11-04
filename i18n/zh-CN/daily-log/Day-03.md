# Day 03 - 2026.10.16 (周五) 创建 VM、IP、转发

[한국어](../../../daily-log/Day-03.md) | [English](../../en/daily-log/Day-03.md) | [日本語](../../ja/daily-log/Day-03.md) | **简体中文** | [Español](../../es/daily-log/Day-03.md) | [Français](../../fr/daily-log/Day-03.md)

## 今天做了什么

- 在 VMware 中创建 Host-only 的 VMnet0~3 并关闭 DHCP (VMnet0 默认是 Bridged，需要修改)
- 创建 8 台 VM，Basic Configuration (软件包、允许 root SSH、主机名)
- 用 `/etc/network/interfaces` 配置静态 IP
- 在 FW 上开启 IP 转发

## 学到的东西

- 即使在 FW 上设置了 `net.ipv4.ip_forward=1`，重启后有时转发仍是关闭的。于是让 `rc.local` 在每次开机时重新执行 `sysctl -p`。
- 下午在公司接受了各厂商服务器操作系统安装和挂载的培训。在实体服务器上学到的 RAID、iLO，帮助我想象这个项目中“服务器插在哪里、怎么插”。
