# Day 09 - 2026.10.26 (周一) JJong LIVE、监控、Ansible

[한국어](../../../daily-log/Day-09.md) | [English](../../en/daily-log/Day-09.md) | [日本語](../../ja/daily-log/Day-09.md) | **简体中文** | [Español](../../es/daily-log/Day-09.md) | [Français](../../fr/daily-log/Day-09.md)

## 今天做了什么

- **JJong LIVE**: 制作了主播“Piyo”介绍特价商品的 1 分钟视频，在 WEB1·WEB2 上用 ffmpeg 以 HLS 无限循环直播
- **监控**: 6 台 node-exporter，用 blackbox 像顾客一样检查商城、JJong LIVE、数据库，6 条 Prometheus 告警规则，Grafana 仪表盘
- **Ansible**: 公共配置和 Web 部署的 Playbook，第二次执行 `changed=0`
- **Verification**: 整理了各章节检查和故障测试

![jjonglive](../../../docs/images/jjonglive.gif)

## 学到的东西

- 没有单独搭建直播服务器，而是让两台 Web 服务器各自直播。使用 `-c copy` 不重新编码，几乎不占 CPU。WEB1 宕机时由 WEB2 的直播接续。
- blackbox 看的不是“服务器是否开机”，而是“服务是否响应”。只有 Apache 挂掉的情况，node-exporter 是发现不了的。
- 一开始 Ansible 除 MGMT 外全部 UNREACHABLE。手动 ssh 时是我自己输入 yes 和密码，但 Ansible 做不到。通过 keyscan 和分发密钥解决。
- 接着又因为 WEB1 上证书路径不同而中断。多亏预先让它在覆盖配置前先检查文件，商城没有宕掉。

![ansible](../../../docs/images/troubleshooting/07-ansible-cert-path.png)

## 感想

一开始手动操作时犯过的错误(700 权限、mbstring)，现在 Ansible 每次都会检查并修复。这一天我切身体会到了为什么要自动化。
