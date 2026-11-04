# Day 05 - 2026.10.20 (Tue) CA, DNS, VPN switch

[한국어](../../../daily-log/Day-05.md) | **English** | [日本語](../../ja/daily-log/Day-05.md) | [简体中文](../../zh-CN/daily-log/Day-05.md) | [Español](../../es/daily-log/Day-05.md) | [Français](../../fr/daily-log/Day-05.md)

## What I did today

- Private Root CA on MGMT (`CA.pl -newca`), RootCA.crt registered as trusted on all devices
- Split-horizon DNS with two BIND9 views (internal / external)
- Switched the VPN from OpenVPN to **WireGuard**
- Started organizing Configuration in the Notion Session tab as per-chapter toggles

## Why I switched to WireGuard

OpenVPN needed server and client certificates, a tls-crypt key, and many config files. With WireGuard, each side only needs the other's public key, and the config is about 10 lines. The goal of this project is "a safe path into the management network", so I chose the simpler option.

## What I learned

- The FW DNATs external queries to the DNS, but the source address doesn't change. So the DNS recognizes the query as external and answers from the external view. At first I thought DNAT would make everything look like it came from inside.
- With `recursion no`, asking for `jjong.internal` from outside returns REFUSED. Internal names don't leak out.
- I copied the zone file templates from the ARM docs in bind9-doc using gpm. Same way I did in competitions.

## Thoughts

Unlike solving tasks, building it like a real workplace brings far more errors. But it keeps me asking "why", so I learn more.
