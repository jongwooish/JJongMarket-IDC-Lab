# Day 10 - 2026.10.27 (mar.) Bilan, GitHub

[한국어](../../../daily-log/Day-10.md) | [English](../../en/daily-log/Day-10.md) | [日本語](../../ja/daily-log/Day-10.md) | [简体中文](../../zh-CN/daily-log/Day-10.md) | [Español](../../es/daily-log/Day-10.md) | **Français**

## Ce que j'ai fait aujourd'hui

- Transfert du README, de Topology, Session, Verification et Resources organisés dans Notion vers un dépôt GitHub
- Séparation en Session (Configuration) et Resources (zip, fichiers de configuration) selon les onglets Notion, puis nouvelle vérification de la cohérence avec les valeurs réellement configurées
- Rassemblement des captures des blocages et rédaction du Troubleshooting
- Remplacement de tous les mots de passe par des marqueurs comme `<ROOT_PASSWORD>`

## Avec le recul

| Plan initial | Réalité |
| --- | --- |
| OpenVPN | WireGuard (configuration plus simple) |
| Grafana (https) derrière nginx | Grafana 3000 en http (laissé comme amélioration) |
| exporter apache·mysqld | Vérification directe de la réponse des services avec blackbox |
| Liste de 8 produits | Boutique de 160 produits + commerce en direct |

## Prochaines étapes

- N'autoriser l'accès SSH qu'avec des clés signées par la CA (supprimer les clés ajoutées avec ssh-copy-id)
- Passer aussi la configuration de FW, DNS et DB sous Ansible
- Grafana en https, réplication de la base, redondance du FW
