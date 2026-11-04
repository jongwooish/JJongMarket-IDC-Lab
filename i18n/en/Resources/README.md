# Resources

[한국어](../../../Resources/README.md) | **English** | [日本語](../../ja/Resources/README.md) | [简体中文](../../zh-CN/Resources/README.md) | [Español](../../es/Resources/README.md) | [Français](../../fr/Resources/README.md)

The deployment bundles I copied from the host PC to each VM with `scp` during the build. They match the files I kept in the Notion Resources tab, and can be rebuilt from this folder with `scripts/make-resources.sh`.

| File | Contents | Used in chapter |
| --- | --- | --- |
| `jjong-idc-lab_WEB.zip` | Shop PHP, Apache site config, db.ini example, shop.sql | 11 MariaDB, 14 WEB |
| `jjong-idc-lab_Streaming.zip` | JJong LIVE video (3.6MB), jjonglive.service | 15 Streaming |
| `jjong-idc-lab_Monitoring.zip` | blackbox, prometheus, alert rules, Grafana provisioning, dashboard JSON | 16 Monitoring |
| `jjong-idc-lab_Ansible.zip` | Playbooks that unzip directly into MGMT `/etc/ansible` | 17 Ansible |

The original files inside the zips are also included as folders.

| Folder | Contents |
| --- | --- |
| `configs/` | Per-chapter configuration files (network, dhcp, nftables, dns, wireguard, ssh, chrony, mariadb, backup, keepalived, web, streaming, monitoring) |
| `ansible/` | Playbooks (`common.yml`, `web.yml`, `site.yml`) |
| `web/` | Shop PHP |
| `live/` | JJong LIVE video and production source |
| `scripts/` | zip build scripts |

External files not included in the repository must be downloaded separately.

| File | Where to get it |
| --- | --- |
| `grafana-enterprise_8.3.4_amd64.deb` | [grafana.com download](https://grafana.com/grafana/download/8.3.4?edition=enterprise) |
| `hls.min.js` | [jsDelivr hls.js](https://cdn.jsdelivr.net/npm/hls.js@1/dist/hls.min.js) |

Notion limits each file to 5MB, so I uploaded the Grafana deb as a split archive in 4MB parts.
