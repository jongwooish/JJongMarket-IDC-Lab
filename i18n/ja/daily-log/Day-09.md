# Day 09 - 2026.10.26 (月) チョンライブ、モニタリング、Ansible

[한국어](../../../daily-log/Day-09.md) | [English](../../en/daily-log/Day-09.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-09.md) | [Español](../../es/daily-log/Day-09.md) | [Français](../../fr/daily-log/Day-09.md)

## 今日やったこと

- **チョンライブ**: ショーホスト「ピヨ」が特価商品を紹介する1分の動画を作り、WEB1・WEB2 で ffmpeg により HLS で無限ループ配信
- **モニタリング**: node-exporter 6台、blackbox でショップ・チョンライブ・DB を顧客の立場で確認、Prometheus アラートルール6個、Grafana ダッシュボード
- **Ansible**: 共通設定と Web デプロイのプレイブック、2回目の実行で `changed=0`
- **Verification**: 章ごとの点検と障害テストを整理

![jjonglive](../../../docs/images/jjonglive.gif)

## 学んだこと

- 配信サーバーを別に作らず、Web サーバー2台がそれぞれ配信するようにした。`-c copy` なので再エンコードせず、CPU をほとんど使わない。WEB1 が落ちても WEB2 の配信で続く。
- blackbox は「サーバーが起動しているか」ではなく「サービスが応答するか」を見る。Apache だけが落ちた状況は node-exporter では検知できない。
- Ansible は最初、MGMT 以外すべて UNREACHABLE。手で ssh するときは自分で yes とパスワードを入力していたが、Ansible にはそれができない。keyscan と鍵の配布で解決。
- 次は WEB1 で証明書のパスが違って止まった。設定を上書きする前にファイルを確認するようにしておいたおかげで、ショップは落ちなかった。

![ansible](../../../docs/images/troubleshooting/07-ansible-cert-path.png)

## 感じたこと

最初に手作業でミスしたこと(700 権限、mbstring)を、Ansible が毎回チェックして直してくれる。なぜ自動化するのかを体で理解した日。
