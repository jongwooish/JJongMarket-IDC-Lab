# Day 07 - 2026.10.22 (木) MariaDB、バックアップ、keepalived

[한국어](../../../daily-log/Day-07.md) | [English](../../en/daily-log/Day-07.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-07.md) | [Español](../../es/daily-log/Day-07.md) | [Français](../../fr/daily-log/Day-07.md)

## 今日やったこと

- DB に MariaDB、`bind-address = 10.10.20.10`、utf8mb4
- バックアップスクリプト + cron、MGMT が rsync で取得するように
- WEB1・WEB2 の keepalived で VIP 10.10.10.100
- MGMT で Web 証明書(www.crt)を発行、WEB に Apache をインストール

## 学んだこと

- バックアップスクリプトに `set -o pipefail` がないと、`mariadb-dump` が失敗しても後ろの gzip が成功して、空のバックアップが「成功」として残る。
- 書き込み中のファイルを MGMT が持っていかないように、`.tmp` で書いて終わったら `mv` した。rsync にも `--exclude '*.tmp'`。
- `/etc/cron.d` のファイル名にドットがあると cron は無視する。`mariadb-backup.cron` のような名前はだめだ。
- keepalived は Apache の状態を2秒ごとに確認し、失敗すると priority を 20 下げる。110 - 20 = 90 が WEB2 の 100 より低くなるので VIP が移る。数字で設計するのが楽しかった。

## 明日やること

ショップのページを公開する
