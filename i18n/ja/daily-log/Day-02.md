# Day 02 - 2026.10.15 (木) 設計

[한국어](../../../daily-log/Day-02.md) | [English](../../en/daily-log/Day-02.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-02.md) | [Español](../../es/daily-log/Day-02.md) | [Français](../../fr/daily-log/Day-02.md)

## 今日やったこと

- ネットワークを用途別に4つに分割: OUTSIDE / SERVER / DB / MGMT
- アドレス規則を決定: 各ネットワークの `.254` は FW、サーバーは `.10` `.21` `.22`、VIP とクライアントは `.100`
- トポロジー図と README の草案を作成し、Notion に JJongMarket IDC ページを作った

## 設計しながら悩んだこと

- **なぜ DB を分けるのか**: Web サーバーが侵害されても DB に直接届かないように。FW で SERVER 網 → DB 3306 だけを開ければいい。
- **203.0.113.0/24**: ドキュメント用に予約された帯域(RFC 5737)なので、実際のインターネットアドレスと重ならない。外部の役割にぴったりだった。
- **VPN**: 最初は OpenVPN で計画していた。(Day 05 で変更)

![topology](../../../docs/images/topology.png)

## 明日やること

VM を作って IP を設定する
