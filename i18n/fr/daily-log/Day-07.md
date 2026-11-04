# Day 07 - 2026.10.22 (jeu.) MariaDB, sauvegarde, keepalived

[한국어](../../../daily-log/Day-07.md) | [English](../../en/daily-log/Day-07.md) | [日本語](../../ja/daily-log/Day-07.md) | [简体中文](../../zh-CN/daily-log/Day-07.md) | [Español](../../es/daily-log/Day-07.md) | **Français**

## Ce que j'ai fait aujourd'hui

- MariaDB sur DB, `bind-address = 10.10.20.10`, utf8mb4
- Script de sauvegarde + cron, que MGMT récupère avec rsync
- VIP 10.10.10.100 avec keepalived sur WEB1·WEB2
- Émission du certificat web (www.crt) sur MGMT, installation d'Apache sur WEB

## Ce que j'ai appris

- Sans `set -o pipefail` dans le script de sauvegarde, même si `mariadb-dump` échoue, le gzip qui suit réussit et une sauvegarde vide reste marquée comme « réussie ».
- Pour que MGMT ne récupère pas un fichier en cours d'écriture, je l'écris en `.tmp` puis je fais un `mv` à la fin. rsync utilise aussi `--exclude '*.tmp'`.
- cron ignore les fichiers de `/etc/cron.d` dont le nom contient un point. Un nom comme `mariadb-backup.cron` ne fonctionne pas.
- keepalived vérifie Apache toutes les 2 secondes et baisse la priorité de 20 en cas d'échec. 110 - 20 = 90 passe sous les 100 de WEB2, et la VIP bascule. Concevoir avec des chiffres, c'était amusant.

## Pour demain

Mettre en ligne les pages de la boutique
