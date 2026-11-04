# Day 03 - 2026.10.16 (Fri) VM creation, IP, forwarding

[한국어](../../../daily-log/Day-03.md) | **English** | [日本語](../../ja/daily-log/Day-03.md) | [简体中文](../../zh-CN/daily-log/Day-03.md) | [Español](../../es/daily-log/Day-03.md) | [Français](../../fr/daily-log/Day-03.md)

## What I did today

- Created Host-only VMnet0~3 in VMware and disabled DHCP (VMnet0 is Bridged by default, so I had to change it)
- Created 8 VMs, Basic Configuration (packages, allow root SSH, hostnames)
- Static IPs via `/etc/network/interfaces`
- IP forwarding on the FW

## What I learned

- Even with `net.ipv4.ip_forward=1` set on the FW, forwarding was sometimes off after a reboot. I made `rc.local` run `sysctl -p` again at every boot.
- In the afternoon, I received training at work on installing server OSes by vendor and mounting. RAID and iLO, learned on real servers, helped me picture "where and how the servers are mounted" in this project.
