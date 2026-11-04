# Day 02 - 2026.10.15 (jeu.) Conception

[한국어](../../../daily-log/Day-02.md) | [English](../../en/daily-log/Day-02.md) | [日本語](../../ja/daily-log/Day-02.md) | [简体中文](../../zh-CN/daily-log/Day-02.md) | [Español](../../es/daily-log/Day-02.md) | **Français**

## Ce que j'ai fait aujourd'hui

- Découpage du réseau en 4 selon l'usage : OUTSIDE / SERVER / DB / MGMT
- Règles d'adressage : `.254` de chaque réseau pour le FW, `.10` `.21` `.22` pour les serveurs, `.100` pour la VIP et les clients
- Schéma de topologie et brouillon du README ; création de la page JJongMarket IDC dans Notion

## Ce à quoi j'ai réfléchi en concevant

- **Pourquoi isoler la base de données** : pour qu'un serveur web compromis ne puisse pas l'atteindre directement. Il suffit d'ouvrir sur le FW réseau SERVER → DB 3306.
- **203.0.113.0/24** : une plage réservée à la documentation (RFC 5737), qui ne chevauche jamais de vraies adresses Internet. Parfait pour jouer le rôle de l'extérieur.
- **VPN** : prévu au départ avec OpenVPN. (Changé au Day 05)

![topology](../../../docs/images/topology.png)

## Pour demain

Créer les VM et configurer les IP
