# Day 08 - 2026.10.23 (金) ショップ、そしてエラーの日

[한국어](../../../daily-log/Day-08.md) | [English](../../en/daily-log/Day-08.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-08.md) | [Español](../../es/daily-log/Day-08.md) | [Français](../../fr/daily-log/Day-08.md)

## 今日やったこと

- 午前: シンプルな HTML ショップ(v1)を公開したが、CLIENT でハングルがすべて四角に
- デザインがあまりにも物足りなかったのでショップを作り直した(v2): 黄色のテーマ、商品160件、レビュー800件、検索、カート、注文、仮想配送、出品、カード登録、メンバーシップ、広告ポップアップ
- 夜: v2 を公開し、500 エラー、詳細ページの崩れ、会員登録エラーを順に解決

![v1](../../../docs/images/troubleshooting/01-hangul-broken.png)

## 詰まったところ

| 症状 | 原因 | 解決 |
| --- | --- | --- |
| ハングルが四角 | CLIENT にハングルフォントがない | `fonts-noto-cjk` |
| 500 エラー | Windows の scp で送った `inc` フォルダーが 700 | `chmod -R a+rX` |
| 詳細ページの崩れ、会員登録エラー | `php-mbstring` がない | インストール + 代替関数 |
| 注文フォームの入力欄が空 | `header.php` とページが `$f` 変数を共用 | `$flash_msg` に名前を変更 |

500 エラーは、root で `php index.php` すると問題ないのに `runuser -u www-data` で実行すると Permission denied が出たので、権限の問題だとわかった。実行ユーザーを変えて再現してみる方法を初めて使った。

![permission](../../../docs/images/troubleshooting/03-permission-denied.png)

詳しくは [troubleshooting.md](../docs/troubleshooting.md)

## 感じたこと

今日がいちばん時間がかかった。それでも一つずつ原因を見つけて直したら、ショップが本物のショップのように動くのがうれしかった。
