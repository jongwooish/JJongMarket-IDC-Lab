# Day 08 - 2026.10.23 (ven.) La boutique, et une journée d'erreurs

[한국어](../../../daily-log/Day-08.md) | [English](../../en/daily-log/Day-08.md) | [日本語](../../ja/daily-log/Day-08.md) | [简体中文](../../zh-CN/daily-log/Day-08.md) | [Español](../../es/daily-log/Day-08.md) | **Français**

## Ce que j'ai fait aujourd'hui

- Matin : mise en ligne d'une boutique HTML simple (v1), mais sur CLIENT tout le coréen s'affichait en carrés
- Le design était vraiment trop pauvre, alors j'ai refait la boutique (v2) : thème jaune, 160 produits, 800 avis, recherche, panier, commandes, livraison simulée, mise en vente, enregistrement de carte, abonnement, pop-ups publicitaires
- Soir : mise en ligne de la v2, puis résolution successive de l'erreur 500, de la page de détail cassée et de l'erreur d'inscription

![v1](../../../docs/images/troubleshooting/01-hangul-broken.png)

## Là où j'ai bloqué

| Symptôme | Cause | Solution |
| --- | --- | --- |
| Coréen en carrés | Pas de police coréenne sur CLIENT | `fonts-noto-cjk` |
| Erreur 500 | Le dossier `inc` envoyé par scp depuis Windows était en 700 | `chmod -R a+rX` |
| Détail cassé, erreur d'inscription | `php-mbstring` absent | Installation + fonctions de repli |
| Champs de commande vides | `header.php` et la page utilisaient la même variable `$f` | Renommée en `$flash_msg` |

L'erreur 500 n'apparaissait pas avec `php index.php` en root, mais `runuser -u www-data` donnait Permission denied : j'ai compris que c'était un problème de permissions. C'était la première fois que je reproduisais un problème en changeant l'utilisateur d'exécution.

![permission](../../../docs/images/troubleshooting/03-permission-denied.png)

Détails dans [troubleshooting.md](../docs/troubleshooting.md)

## Impressions

Aujourd'hui a été la journée la plus longue. Mais après avoir trouvé et corrigé chaque cause une par une, j'étais fier de voir la boutique fonctionner comme une vraie boutique.
