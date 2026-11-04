# Day 06 - 2026.10.21 (mer.) WireGuard, SSH CA, chrony

[한국어](../../../daily-log/Day-06.md) | [English](../../en/daily-log/Day-06.md) | [日本語](../../ja/daily-log/Day-06.md) | [简体中文](../../zh-CN/daily-log/Day-06.md) | [Español](../../es/daily-log/Day-06.md) | **Français**

## Ce que j'ai fait aujourd'hui

- wg0 (10.8.0.1) sur le FW ; REMOTE établit le tunnel avec `nmcli connection import`
- Création d'une SSH CA sur MGMT et signature de la clé root
- Le DNS sert de référence de temps (local stratum 8) ; tous les autres se synchronisent sur le DNS

## Ce que j'ai appris

- `AllowedIPs` de WireGuard n'a pas le même sens dans chaque direction. Côté FW, c'est « les adresses acceptées de ce pair » ; côté REMOTE, « les destinations envoyées dans le tunnel ». En ne mettant que `10.10.0.0/16` sur REMOTE, seul le trafic interne passe par le tunnel.
- La passerelle par défaut des serveurs internes est le FW, donc les réponses vers 10.8.0.2 repartent par FW → wg0 sans route supplémentaire.
- Avec une SSH CA, il n'est pas nécessaire d'installer chaque clé publique sur chaque serveur : il suffit de faire confiance à la seule clé publique de la CA. En revanche, ce n'est qu'en faisant la revue plus tard que j'ai découvert qu'en copiant tout `/etc/ssl`, la clé privée de la CA s'était aussi propagée.
- Sans Internet, il faut commenter la ligne `pool` par défaut de chrony. Sinon, il essaie sans arrêt de sortir.
