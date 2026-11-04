# Day 10 - 2026.10.27 (周二) 整理、GitHub

[한국어](../../../daily-log/Day-10.md) | [English](../../en/daily-log/Day-10.md) | [日本語](../../ja/daily-log/Day-10.md) | **简体中文** | [Español](../../es/daily-log/Day-10.md) | [Français](../../fr/daily-log/Day-10.md)

## 今天做了什么

- 把在 Notion 中整理的 README、Topology、Session、Verification、Resources 迁移到 GitHub 仓库
- 按照 Notion 页签结构分成 Session(Configuration)和 Resources(zip、配置文件)，并再次核对与实际搭建的值是否一致
- 收集卡住时的截图，整理成 Troubleshooting
- 所有密码都替换为 `<ROOT_PASSWORD>` 之类的占位符

## 回顾

| 最初计划 | 实际 |
| --- | --- |
| OpenVPN | WireGuard (配置简单) |
| nginx 后面的 Grafana(https) | Grafana 3000 http (留作改进项) |
| apache·mysqld exporter | 用 blackbox 直接检查服务响应 |
| 8 件商品的商品列表 | 160 件商品的商城 + 直播带货 |

## 接下来要做的事

- 整理为只能用 CA 签名的密钥进行 SSH 连接 (删除通过 ssh-copy-id 添加的密钥)
- 把 FW、DNS、DB 的配置也迁移到 Ansible
- Grafana https、数据库复制、FW 冗余
