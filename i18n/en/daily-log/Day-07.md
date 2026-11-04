# Day 07 - 2026.10.22 (Thu) MariaDB, backup, keepalived

[한국어](../../../daily-log/Day-07.md) | **English** | [日本語](../../ja/daily-log/Day-07.md) | [简体中文](../../zh-CN/daily-log/Day-07.md) | [Español](../../es/daily-log/Day-07.md) | [Français](../../fr/daily-log/Day-07.md)

## What I did today

- MariaDB on DB, `bind-address = 10.10.20.10`, utf8mb4
- Backup script + cron, with MGMT pulling it via rsync
- VIP 10.10.10.100 with keepalived on WEB1·WEB2
- Issued the web certificate (www.crt) on MGMT, installed Apache on WEB

## What I learned

- Without `set -o pipefail` in the backup script, even if `mariadb-dump` fails, the gzip after it succeeds, leaving an empty backup marked as "success".
- To keep MGMT from pulling a file still being written, I wrote it as `.tmp` and `mv`'d it when done. rsync also uses `--exclude '*.tmp'`.
- cron ignores files in `/etc/cron.d` whose names contain a dot. A name like `mariadb-backup.cron` won't work.
- keepalived checks Apache every 2 seconds and lowers the priority by 20 on failure. 110 - 20 = 90 drops below WEB2's 100, so the VIP moves over. Designing with numbers was fun.

## Tomorrow

Put up the shop pages
