# Resources

[한국어](../../../Resources/README.md) | [English](../../en/Resources/README.md) | [日本語](../../ja/Resources/README.md) | **简体中文** | [Español](../../es/Resources/README.md) | [Français](../../fr/Resources/README.md)

搭建时从宿主机用 `scp` 发送到各台 VM 的部署包。与我放在 Notion Resources 页签中使用的文件结构相同，在此文件夹中运行 `scripts/make-resources.sh` 即可重新生成。

| 文件 | 内容 | 使用章节 |
| --- | --- | --- |
| `jjong-idc-lab_WEB.zip` | 商城 PHP、Apache 站点配置、db.ini 示例、shop.sql | 11 MariaDB, 14 WEB |
| `jjong-idc-lab_Streaming.zip` | JJong LIVE 视频(3.6MB)、jjonglive.service | 15 Streaming |
| `jjong-idc-lab_Monitoring.zip` | blackbox、prometheus、告警规则、Grafana provisioning、仪表盘 JSON | 16 Monitoring |
| `jjong-idc-lab_Ansible.zip` | 直接解压到 MGMT `/etc/ansible` 的 Playbook | 17 Ansible |

zip 内的原始文件也按文件夹一并保留。

| 文件夹 | 内容 |
| --- | --- |
| `configs/` | 各章节的配置文件 (network, dhcp, nftables, dns, wireguard, ssh, chrony, mariadb, backup, keepalived, web, streaming, monitoring) |
| `ansible/` | Playbook (`common.yml`, `web.yml`, `site.yml`) |
| `web/` | 商城 PHP |
| `live/` | JJong LIVE 视频与制作素材 |
| `scripts/` | zip 生成脚本 |

未放入仓库的外部文件需另行下载。

| 文件 | 获取地址 |
| --- | --- |
| `grafana-enterprise_8.3.4_amd64.deb` | [grafana.com 下载](https://grafana.com/grafana/download/8.3.4?edition=enterprise) |
| `hls.min.js` | [jsDelivr hls.js](https://cdn.jsdelivr.net/npm/hls.js@1/dist/hls.min.js) |

Notion 单个文件限制为 5MB，因此 Grafana 的 deb 包按 4MB 分卷压缩后上传。
