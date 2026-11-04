# Day 03 - 2026.10.16 (ven.) Création des VM, IP, routage

[한국어](../../../daily-log/Day-03.md) | [English](../../en/daily-log/Day-03.md) | [日本語](../../ja/daily-log/Day-03.md) | [简体中文](../../zh-CN/daily-log/Day-03.md) | [Español](../../es/daily-log/Day-03.md) | **Français**

## Ce que j'ai fait aujourd'hui

- Création des VMnet0~3 Host-only dans VMware et désactivation du DHCP (VMnet0 est en Bridged par défaut, il a fallu le changer)
- Création des 8 VM, Basic Configuration (paquets, connexion SSH root autorisée, noms d'hôte)
- IP fixes via `/etc/network/interfaces`
- Routage IP sur le FW

## Ce que j'ai appris

- Même avec `net.ipv4.ip_forward=1` sur le FW, le routage était parfois désactivé après un redémarrage. J'ai fait relancer `sysctl -p` par `rc.local` à chaque démarrage.
- L'après-midi, j'ai suivi au travail une formation sur l'installation des systèmes d'exploitation serveur selon les constructeurs et sur le montage. Le RAID et l'iLO vus sur de vrais serveurs m'ont aidé à imaginer « où et comment les serveurs sont installés » dans ce projet.
