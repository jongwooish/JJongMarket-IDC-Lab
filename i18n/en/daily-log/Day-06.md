# Day 06 - 2026.10.21 (Wed) WireGuard, SSH CA, chrony

[한국어](../../../daily-log/Day-06.md) | **English** | [日本語](../../ja/daily-log/Day-06.md) | [简体中文](../../zh-CN/daily-log/Day-06.md) | [Español](../../es/daily-log/Day-06.md) | [Français](../../fr/daily-log/Day-06.md)

## What I did today

- wg0 (10.8.0.1) on the FW; REMOTE connects the tunnel with `nmcli connection import`
- Created an SSH CA on MGMT and signed the root key
- Made DNS the time source (local stratum 8); everything else syncs to DNS

## What I learned

- WireGuard's `AllowedIPs` means something different in each direction. On the FW it's "addresses to accept from this peer"; on REMOTE it's "destinations to send through the tunnel". By putting only `10.10.0.0/16` on REMOTE, only internal traffic goes through the tunnel.
- Internal servers use the FW as their default gateway, so replies to 10.8.0.2 return via FW → wg0 without adding any extra route.
- With an SSH CA, servers only need to trust the one CA public key instead of having each public key installed. However, I only found out later during the review that copying all of `/etc/ssl` had spread the CA private key too.
- With no internet, the default chrony `pool` line must be commented out. Otherwise it keeps trying to reach outside.
