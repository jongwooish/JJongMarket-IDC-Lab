# Day 05 - 2026.10.20 (mar.) CA, DNS, changement de VPN

[한국어](../../../daily-log/Day-05.md) | [English](../../en/daily-log/Day-05.md) | [日本語](../../ja/daily-log/Day-05.md) | [简体中文](../../zh-CN/daily-log/Day-05.md) | [Español](../../es/daily-log/Day-05.md) | **Français**

## Ce que j'ai fait aujourd'hui

- Root CA privée sur MGMT (`CA.pl -newca`), RootCA.crt enregistrée comme fiable sur tous les équipements
- DNS split-horizon avec deux view BIND9 (internal / external)
- Passage du VPN d'OpenVPN à **WireGuard**
- Début de l'organisation de la Configuration dans l'onglet Session de Notion, avec un bloc dépliable par chapitre

## Pourquoi je suis passé à WireGuard

OpenVPN demandait des certificats serveur et client, une clé tls-crypt et beaucoup de fichiers de configuration. Avec WireGuard, chaque côté n'a besoin que de la clé publique de l'autre, et la configuration tient en une dizaine de lignes. L'objectif de ce projet étant « un accès sécurisé au réseau de gestion », j'ai choisi la solution la plus simple.

## Ce que j'ai appris

- Le FW fait un DNAT des requêtes externes vers le DNS, mais l'adresse source ne change pas. Le DNS reconnaît donc la requête comme externe et répond avec l'external view. Au début, je pensais qu'avec le DNAT tout aurait l'air de venir de l'intérieur.
- Avec `recursion no`, interroger `jjong.internal` depuis l'extérieur renvoie REFUSED. Les noms internes ne fuient pas à l'extérieur.
- J'ai copié les modèles de zone depuis la documentation ARM de bind9-doc avec gpm, comme en concours.

## Impressions

Contrairement à la résolution d'exercices, construire comme en entreprise génère bien plus d'erreurs. Mais cela pousse à toujours demander « pourquoi », donc on apprend davantage.
