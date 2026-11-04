# Day 09 - 2026.10.26 (Mon) JJong LIVE, monitoring, Ansible

[한국어](../../../daily-log/Day-09.md) | **English** | [日本語](../../ja/daily-log/Day-09.md) | [简体中文](../../zh-CN/daily-log/Day-09.md) | [Español](../../es/daily-log/Day-09.md) | [Français](../../fr/daily-log/Day-09.md)

## What I did today

- **JJong LIVE**: made a 1-minute video of show host "Piyo" introducing deals, and broadcast it as an endless HLS loop with ffmpeg on WEB1·WEB2
- **Monitoring**: node-exporter on 6 servers, blackbox checking the shop, JJong LIVE and DB like a customer, 6 Prometheus alert rules, Grafana dashboard
- **Ansible**: playbooks for common settings and web deployment; second run `changed=0`
- **Verification**: organized per-chapter checks and failure tests

![jjonglive](../../../docs/images/jjonglive.gif)

## What I learned

- Instead of a separate streaming server, both web servers broadcast on their own. With `-c copy` there's no re-encoding, so it uses almost no CPU. If WEB1 dies, WEB2's broadcast continues.
- blackbox checks "does the service respond", not "is the server on". A case where only Apache died can't be caught by node-exporter.
- At first Ansible was UNREACHABLE for everything except MGMT. When I ssh'd by hand, I typed yes and the password myself, but Ansible can't. Fixed with keyscan and key distribution.
- Next it stopped on WEB1 because the certificate path was different. Since I'd made it check files before overwriting settings, the shop never went down.

![ansible](../../../docs/images/troubleshooting/07-ansible-cert-path.png)

## Thoughts

Ansible now checks and fixes, every time, the mistakes I made by hand at first (700 permissions, mbstring). The day I understood with my whole body why we automate.
