# Day 10 - 2026.10.27 (火) 整理、GitHub

[한국어](../../../daily-log/Day-10.md) | [English](../../en/daily-log/Day-10.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-10.md) | [Español](../../es/daily-log/Day-10.md) | [Français](../../fr/daily-log/Day-10.md)

## 今日やったこと

- Notion にまとめた README、Topology、Session、Verification、Resources を GitHub リポジトリへ移した
- Notion のタブ構成どおり Session(Configuration)と Resources(zip、設定ファイル)に分け、実際に構築した値と一致しているかを改めて確認
- 詰まった画面を集めて Troubleshooting を整理
- パスワードはすべて `<ROOT_PASSWORD>` のようなプレースホルダーに置き換え

## 振り返り

| 最初の計画 | 実際 |
| --- | --- |
| OpenVPN | WireGuard (設定がシンプル) |
| nginx の後ろの Grafana(https) | Grafana 3000 http (改善課題として残す) |
| apache・mysqld exporter | blackbox でサービスの応答を直接確認 |
| 商品8件の商品一覧 | 商品160件のショップ + ライブコマース |

## 次にやること

- SSH 接続を CA 証明書付きの鍵だけにするよう整理 (ssh-copy-id で入れた鍵を削除)
- FW、DNS、DB の設定も Ansible へ移す
- Grafana https、DB レプリケーション、FW の冗長化
