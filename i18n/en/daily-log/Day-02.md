# Day 02 - 2026.10.15 (Thu) Design

[한국어](../../../daily-log/Day-02.md) | **English** | [日本語](../../ja/daily-log/Day-02.md) | [简体中文](../../zh-CN/daily-log/Day-02.md) | [Español](../../es/daily-log/Day-02.md) | [Français](../../fr/daily-log/Day-02.md)

## What I did today

- Split the network into 4 by purpose: OUTSIDE / SERVER / DB / MGMT
- Set address rules: `.254` on each network is the FW, servers are `.10` `.21` `.22`, VIP and clients are `.100`
- Drew the topology and wrote a README draft; created a JJongMarket IDC page in Notion

## What I considered while designing

- **Why separate the DB**: so a compromised web server can't reach the DB directly. The FW only needs to open SERVER network → DB 3306.
- **203.0.113.0/24**: a range reserved for documentation (RFC 5737), so it never overlaps real internet addresses. Perfect for the outside role.
- **VPN**: initially planned with OpenVPN. (Changed on Day 05)

![topology](../../../docs/images/topology.png)

## Tomorrow

Create the VMs and set up IPs
