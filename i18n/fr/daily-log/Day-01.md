# Day 01 - 2026.10.14 (mer.) Planification

[한국어](../../../daily-log/Day-01.md) | [English](../../en/daily-log/Day-01.md) | [日本語](../../ja/daily-log/Day-01.md) | [简体中文](../../zh-CN/daily-log/Day-01.md) | [Español](../../es/daily-log/Day-01.md) | **Français**

## Ce que j'ai fait aujourd'hui

- Première sortie de mon stage : installation de serveurs à l'IDC de SK Broadband à Seocho-dong
- Retour au bureau l'après-midi, et choix de l'orientation de mon projet personnel
- Abonnement à Claude Pro, création du projet Claude « 쫑 » (JJong) et connexion de mes notes Notion (IT Network System)

## Pourquoi un IDC

C'était la première fois que j'entrais dans un IDC. En voyant les serveurs dans les baies, avec les serveurs web et la base de données séparés derrière un pare-feu, j'ai vraiment compris que les services que je construisais jusque-là un exercice à la fois pendant l'entraînement aux concours de compétences fonctionnent en réalité ensemble.
J'ai donc décidé de ne pas « résoudre des exercices », mais de construire de bout en bout « l'infrastructure d'une entreprise ».

## Plan du projet

### Contexte

JJongMarket SA est une boutique en ligne d'une vingtaine d'employés. Comme les coûts du cloud ne cessaient d'augmenter, elle a décidé de louer une baie dans un IDC et d'exploiter ses propres serveurs. Je suis le responsable de l'infrastructure.

### Exigences

| ID | Exigence | Comment |
| --- | --- | --- |
| R1 | Accéder à la boutique via `https://www.jjongmarket.kr` depuis Internet | FW DNAT → VIP web → Apache |
| R2 | Garder la boutique en ligne même si un serveur web tombe | keepalived VRRP |
| R3 | N'ouvrir que les ports nécessaires | nftables avec blocage par défaut |
| R4 | Administration uniquement depuis le réseau de gestion ; télétravail via VPN | VPN → MGMT uniquement |
| R5 | Réponses DNS différentes à l'intérieur et à l'extérieur | BIND9 view |
| R6 | Stockage et sauvegarde des données produits | MariaDB + dump quotidien + copie sur un autre serveur |
| R7 | L'état sur un seul écran | Prometheus + Grafana |
| R8 | Permettre au prochain responsable de tout reconstruire | Documentation + Ansible |
| R9 | Pas d'avertissement HTTPS | Root CA privée |
| R10 | Heure synchronisée sur tous les serveurs | chrony |

### Contraintes

- VMware Workstation, Debian 13.2, 8 VM (6 serveurs CLI, 2 clients GUI)
- Réseaux Host-only uniquement. On suppose qu'il n'y a pas d'Internet ; l'adaptateur NAT n'est branché que brièvement pour installer des paquets
- Contrairement à l'entraînement aux concours, « ça marche » n'est pas une fin en soi : je dois pouvoir expliquer pourquoi j'ai fait ainsi

## Impressions

C'est mon premier projet avec une IA. J'ai décidé de ne pas recopier les réponses, mais de poser des questions sur ce que je ne sais pas, de comprendre, puis de taper moi-même.
