# Day 07 - 2026.10.22 (周四) MariaDB、备份、keepalived

[한국어](../../../daily-log/Day-07.md) | [English](../../en/daily-log/Day-07.md) | [日本語](../../ja/daily-log/Day-07.md) | **简体中文** | [Español](../../es/daily-log/Day-07.md) | [Français](../../fr/daily-log/Day-07.md)

## 今天做了什么

- 在 DB 上安装 MariaDB，`bind-address = 10.10.20.10`，utf8mb4
- 备份脚本 + cron，由 MGMT 通过 rsync 拉取
- 在 WEB1·WEB2 上用 keepalived 配置 VIP 10.10.10.100
- 在 MGMT 上签发 Web 证书(www.crt)，在 WEB 上安装 Apache

## 学到的东西

- 备份脚本中如果没有 `set -o pipefail`，即使 `mariadb-dump` 失败，后面的 gzip 也会成功，空备份就会被记为“成功”。
- 为了不让 MGMT 拉走正在写入的文件，先写成 `.tmp`，完成后再 `mv`。rsync 也加上 `--exclude '*.tmp'`。
- `/etc/cron.d` 中文件名带点时 cron 会忽略它。像 `mariadb-backup.cron` 这样的名字不行。
- keepalived 每 2 秒检查一次 Apache 状态，失败时把 priority 降低 20。110 - 20 = 90 低于 WEB2 的 100，VIP 就会切换过去。用数字来设计很有意思。

## 明天要做的事

上线商城页面
