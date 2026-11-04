# Day 06 - 2026.10.21 (水) WireGuard、SSH CA、chrony

[한국어](../../../daily-log/Day-06.md) | [English](../../en/daily-log/Day-06.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-06.md) | [Español](../../es/daily-log/Day-06.md) | [Français](../../fr/daily-log/Day-06.md)

## 今日やったこと

- FW に wg0 (10.8.0.1)、REMOTE は `nmcli connection import` でトンネル接続
- MGMT に SSH CA を作り、root キーに署名
- DNS を時刻の基準(local stratum 8)にし、残りはすべて DNS に合わせる

## 学んだこと

- WireGuard の `AllowedIPs` は方向ごとに意味が違う。FW 側は「このピアから受け取るアドレス」、REMOTE 側は「トンネルに送る宛先」。REMOTE に `10.10.0.0/16` だけを入れたので、内部網のトラフィックだけがトンネルを通る。
- 内部サーバーはデフォルトゲートウェイが FW なので、10.8.0.2 への応答は別途ルートを入れなくても FW → wg0 で戻っていく。
- SSH CA を使えば、サーバーごとに公開鍵を入れる必要はなく、CA の公開鍵1つを信頼させればいい。ただ、`/etc/ssl` を丸ごとコピーしたせいで CA の秘密鍵まで広がっていたことには、後で点検したときに気づいた。
- インターネットがないので、chrony のデフォルトの `pool` 行はコメントアウトしなければならない。そうしないとずっと外に出ようとする。
