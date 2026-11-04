# Day 04 - 2026.10.19 (lun.) DHCP, nftables

[한국어](../../../daily-log/Day-04.md) | [English](../../en/daily-log/Day-04.md) | [日本語](../../ja/daily-log/Day-04.md) | [简体中文](../../zh-CN/daily-log/Day-04.md) | [Español](../../es/daily-log/Day-04.md) | **Français**

## Ce que j'ai fait aujourd'hui

- isc-dhcp-server sur MGMT ; vérifié que CLIENT reçoit 10.10.30.100~150
- REMOTE est sur un autre réseau, son broadcast n'atteint donc pas MGMT → relais DHCP avec dnsmasq sur le FW
- Écriture des règles nftables (drop par défaut, n'autoriser que le nécessaire, DNAT externe pour 80·443·53)

## Ce que j'ai appris

- Via un relais, le serveur DHCP regarde l'adresse du relais (giaddr) dans le paquet pour choisir dans quel sous-réseau attribuer. C'est pourquoi dhcpd.conf doit contenir le sous-réseau 203.0.113.0.
- Je n'ai volontairement pas mis `option routers` dans la plage de REMOTE. Sans passerelle, il n'existe aucun chemin vers le réseau interne sans VPN.
- Pendant le travail sur nftables, je me suis bloqué plusieurs fois avec mes propres règles et scp ne fonctionnait plus. J'ai pris l'habitude de faire `systemctl stop nftables` uniquement pendant le travail et de le réactiver ensuite.
