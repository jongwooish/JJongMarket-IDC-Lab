# Day 04 - 2026.10.19 (月) DHCP、nftables

[한국어](../../../daily-log/Day-04.md) | [English](../../en/daily-log/Day-04.md) | **日本語** | [简体中文](../../zh-CN/daily-log/Day-04.md) | [Español](../../es/daily-log/Day-04.md) | [Français](../../fr/daily-log/Day-04.md)

## 今日やったこと

- MGMT に isc-dhcp-server、CLIENT が 10.10.30.100~150 を受け取ることを確認
- REMOTE は別ネットワークなのでブロードキャストが MGMT まで届かない → FW で dnsmasq による DHCP リレー
- nftables ルールを作成 (デフォルト drop、必要なものだけ許可、外部 80・443・53 を DNAT)

## 学んだこと

- リレーを経由すると、DHCP サーバーはパケット内のリレーアドレス(giaddr)を見て、どのサブネットから払い出すかを選ぶ。そのため dhcpd.conf に 203.0.113.0 のサブネットが必要だ。
- REMOTE の範囲には `option routers` をわざと入れなかった。ゲートウェイがないので、VPN なしでは内部へ行く経路そのものがない。
- nftables の作業中、自分で作ったルールに自分がブロックされて scp ができないことが何度かあった。作業するときだけ `systemctl stop nftables` して、終わったら再びオンにする習慣がついた。
