# Day 04 - 2026.10.19 (Mon) DHCP, nftables

[한국어](../../../daily-log/Day-04.md) | **English** | [日本語](../../ja/daily-log/Day-04.md) | [简体中文](../../zh-CN/daily-log/Day-04.md) | [Español](../../es/daily-log/Day-04.md) | [Français](../../fr/daily-log/Day-04.md)

## What I did today

- isc-dhcp-server on MGMT; confirmed CLIENT gets 10.10.30.100~150
- REMOTE is on a different network, so its broadcast can't reach MGMT → DHCP relay with dnsmasq on the FW
- Wrote nftables rules (default drop, allow only what's needed, external 80·443·53 DNAT)

## What I learned

- Through a relay, the DHCP server looks at the relay address (giaddr) in the packet to pick which subnet to lease from. That's why dhcpd.conf needs a 203.0.113.0 subnet.
- I intentionally left `option routers` out of the REMOTE range. With no gateway, there's no path into the internal network at all without the VPN.
- A few times during nftables work, I locked myself out with my own rules and scp stopped working. I got into the habit of running `systemctl stop nftables` only while working and turning it back on when done.
