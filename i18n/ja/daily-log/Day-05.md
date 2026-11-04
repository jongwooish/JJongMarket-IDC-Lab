# Day 05 - 2026.10.20 (火) CA、DNS、VPN の変更

[한국어](../../../daily-log/Day-05.md) | [English](../../en/daily-log/Day-05.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-05.md) | [Español](../../es/daily-log/Day-05.md) | [Français](../../fr/daily-log/Day-05.md)

## 今日やったこと

- MGMT にプライベート Root CA (`CA.pl -newca`)、全機器に RootCA.crt を信頼登録
- BIND9 view 2つ(internal / external)で split-horizon DNS
- VPN を OpenVPN から **WireGuard** に変更
- Notion の Session タブで章ごとのトグルとして Configuration の整理を開始

## なぜ WireGuard に変えたのか

OpenVPN はサーバー・クライアント証明書、tls-crypt キー、設定ファイルが多かった。WireGuard は互いの公開鍵さえ知っていればよく、設定も10行ほどだ。今回のプロジェクトの目的は「管理網に安全に入る経路」なので、よりシンプルなほうを選んだ。

## 学んだこと

- 外部からの問い合わせは FW が DNS へ DNAT するが、送信元アドレスは変わらない。だから DNS はその問い合わせを外部のものと判断し、external view で応答する。最初は DNAT すると全部内部から来たように見えると思っていた。
- `recursion no` なので、外部から `jjong.internal` を問い合わせると REFUSED になる。内部の名前が外に漏れない。
- zone ファイルのひな形は bind9-doc の ARM ドキュメントから gpm でコピーした。大会のときと同じやり方だ。

## 感じたこと

課題を解くのとは違い、実務のように構成しようとするとエラーがはるかに多い。それでも「なぜ」を問い続けることになるので、より勉強になる。
