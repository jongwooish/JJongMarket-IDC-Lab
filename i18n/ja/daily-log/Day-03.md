# Day 03 - 2026.10.16 (金) VM 作成、IP、フォワーディング

[한국어](../../../daily-log/Day-03.md) | [English](../../en/daily-log/Day-03.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-03.md) | [Español](../../es/daily-log/Day-03.md) | [Français](../../fr/daily-log/Day-03.md)

## 今日やったこと

- VMware に Host-only の VMnet0~3 を作り DHCP をオフ (VMnet0 はデフォルトが Bridged なので変更が必要だった)
- VM 8台を作成、Basic Configuration (パッケージ、root SSH 許可、ホスト名)
- `/etc/network/interfaces` で固定 IP
- FW に IP フォワーディング

## 学んだこと

- FW に `net.ipv4.ip_forward=1` を入れても、再起動するとフォワーディングがオフになっている場合があった。`rc.local` で起動のたびに `sysctl -p` を再実行するようにした。
- 午後は会社でメーカー別のサーバー OS のインストールとマウントの研修を受けた。実機のサーバーで学んだ RAID や iLO が、このプロジェクトで「サーバーがどこにどう挿さっているか」を想像するのに役立った。
