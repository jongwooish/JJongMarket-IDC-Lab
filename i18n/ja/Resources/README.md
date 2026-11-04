# Resources

[한국어](../../../Resources/README.md) | [English](../../en/Resources/README.md) | **日本語** | [简体中文](../../zh-CN/Resources/README.md) | [Español](../../es/Resources/README.md) | [Français](../../fr/Resources/README.md)

構築時にホストPCから各VMへ `scp` で送っていた配布パッケージです。NotionのResourcesタブに置いて使っていたファイルと同じ構成で、このフォルダーで `scripts/make-resources.sh` を実行すると作り直せます。

| ファイル | 内容 | 使用する章 |
| --- | --- | --- |
| `jjong-idc-lab_WEB.zip` | ショップ PHP、Apache サイト設定、db.ini の例、shop.sql | 11 MariaDB, 14 WEB |
| `jjong-idc-lab_Streaming.zip` | チョンライブ動画(3.6MB)、jjonglive.service | 15 Streaming |
| `jjong-idc-lab_Monitoring.zip` | blackbox、prometheus、アラートルール、Grafana provisioning、ダッシュボード JSON | 16 Monitoring |
| `jjong-idc-lab_Ansible.zip` | MGMT の `/etc/ansible` にそのまま展開されるプレイブック | 17 Ansible |

zip内の元ファイルもフォルダーごと一緒に置いています。

| フォルダー | 内容 |
| --- | --- |
| `configs/` | 章ごとの設定ファイル (network, dhcp, nftables, dns, wireguard, ssh, chrony, mariadb, backup, keepalived, web, streaming, monitoring) |
| `ansible/` | プレイブック (`common.yml`, `web.yml`, `site.yml`) |
| `web/` | ショップ PHP |
| `live/` | チョンライブの動画と制作ソース |
| `scripts/` | zip 作成スクリプト |

リポジトリに含めていない外部ファイルは別途ダウンロードします。

| ファイル | 入手先 |
| --- | --- |
| `grafana-enterprise_8.3.4_amd64.deb` | [grafana.com ダウンロード](https://grafana.com/grafana/download/8.3.4?edition=enterprise) |
| `hls.min.js` | [jsDelivr hls.js](https://cdn.jsdelivr.net/npm/hls.js@1/dist/hls.min.js) |

Notionは1ファイル5MBの制限があるため、Grafanaのdebは4MBずつ分割圧縮してアップロードしました。
