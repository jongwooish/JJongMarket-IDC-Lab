# Day 10 - 2026.10.27 (Tue) Wrap-up, GitHub

[한국어](../../../daily-log/Day-10.md) | **English** | [日本語](../../ja/daily-log/Day-10.md) | [简体中文](../../zh-CN/daily-log/Day-10.md) | [Español](../../es/daily-log/Day-10.md) | [Français](../../fr/daily-log/Day-10.md)

## What I did today

- Moved the README, Topology, Session, Verification and Resources I'd organized in Notion to a GitHub repository
- Split into Session (Configuration) and Resources (zips, config files) following the Notion tab structure, and re-checked that everything matches the values actually built
- Collected the screens where I got stuck and wrote up Troubleshooting
- Replaced every password with placeholders like `<ROOT_PASSWORD>`

## Looking back

| Original plan | Actual |
| --- | --- |
| OpenVPN | WireGuard (simpler config) |
| Grafana (https) behind nginx | Grafana 3000 http (left as an improvement) |
| apache·mysqld exporter | Check service responses directly with blackbox |
| Product list with 8 items | 160-product shop + live commerce |

## Next steps

- Make SSH access use only CA-signed keys (remove keys added with ssh-copy-id)
- Move FW, DNS and DB configuration to Ansible too
- Grafana https, DB replication, FW redundancy
