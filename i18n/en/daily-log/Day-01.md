# Day 01 - 2026.10.14 (Wed) Planning

[한국어](../../../daily-log/Day-01.md) | **English** | [日本語](../../ja/daily-log/Day-01.md) | [简体中文](../../zh-CN/daily-log/Day-01.md) | [Español](../../es/daily-log/Day-01.md) | [Français](../../fr/daily-log/Day-01.md)

## What I did today

- Went on my first field trip of the internship to install servers at the SK Broadband IDC in Seocho-dong
- Came back to the office in the afternoon and decided the direction of my personal project
- Subscribed to Claude Pro, created a Claude project "쫑" (JJong) for this, and connected my Notion notes (IT Network System)

## Why an IDC

It was my first time inside an IDC. Seeing servers mounted in racks, with web servers and a DB separated behind a firewall, it really hit me that the services I had been building one task at a time in my skills-competition training actually run tied together as one.
So instead of "solving tasks", I decided to build "one company's infrastructure" from start to finish.

## Project plan

### Situation

JJongMarket Inc. is an online shop with about 20 employees. As cloud costs kept rising, it decided to rent an IDC rack and run its own servers. I'm the infrastructure engineer.

### Requirements

| ID | Requirement | How |
| --- | --- | --- |
| R1 | Access the shop at `https://www.jjongmarket.kr` from the internet | FW DNAT → web VIP → Apache |
| R2 | Keep the shop running even if one web server dies | keepalived VRRP |
| R3 | Open only the required ports | nftables default deny |
| R4 | Server management only from the management network; remote work via VPN | VPN → only MGMT allowed |
| R5 | Different DNS answers inside and outside | BIND9 view |
| R6 | Store and back up product data | MariaDB + daily dump + stored on another server |
| R7 | Status on one screen | Prometheus + Grafana |
| R8 | Let the next engineer rebuild it | Docs + Ansible |
| R9 | No HTTPS warnings | Private Root CA |
| R10 | All servers in time sync | chrony |

### Constraints

- VMware Workstation, Debian 13.2, 8 VMs (6 CLI servers, 2 GUI clients)
- Host-only networks only. Assume no internet; attach a NAT adapter only briefly when installing packages
- Unlike competition practice, "it works" is not the end — I must be able to explain why I did it that way

## Thoughts

This is my first project using AI. Instead of copying answers, I decided to ask about what I don't know, understand it, and then type it in myself.
