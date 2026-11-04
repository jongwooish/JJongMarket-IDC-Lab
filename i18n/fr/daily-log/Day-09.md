# Day 09 - 2026.10.26 (lun.) JJong LIVE, supervision, Ansible

[한국어](../../../daily-log/Day-09.md) | [English](../../en/daily-log/Day-09.md) | [日本語](../../ja/daily-log/Day-09.md) | [简体中文](../../zh-CN/daily-log/Day-09.md) | [Español](../../es/daily-log/Day-09.md) | **Français**

## Ce que j'ai fait aujourd'hui

- **JJong LIVE** : réalisation d'une vidéo d'1 minute où l'animatrice « Piyo » présente des promotions, diffusée en boucle infinie en HLS avec ffmpeg sur WEB1·WEB2
- **Supervision** : node-exporter sur 6 serveurs, blackbox qui vérifie la boutique, JJong LIVE et la base comme un client, 6 règles d'alerte Prometheus, tableau de bord Grafana
- **Ansible** : playbooks de configuration commune et de déploiement web ; deuxième exécution `changed=0`
- **Verification** : organisation des vérifications par chapitre et des tests de panne

![jjonglive](../../../docs/images/jjonglive.gif)

## Ce que j'ai appris

- Au lieu d'un serveur de diffusion séparé, chacun des deux serveurs web diffuse de son côté. Avec `-c copy`, pas de réencodage, donc presque aucune utilisation CPU. Si WEB1 tombe, la diffusion continue depuis WEB2.
- blackbox ne regarde pas « le serveur est-il allumé ? » mais « le service répond-il ? ». Une panne d'Apache seul ne peut pas être détectée par node-exporter.
- Au début, Ansible était UNREACHABLE partout sauf sur MGMT. En ssh manuel, je tapais moi-même yes et le mot de passe, mais Ansible ne peut pas le faire. Résolu avec keyscan et la distribution de clés.
- Ensuite, il s'est arrêté sur WEB1 car le chemin du certificat était différent. Comme je lui avais fait vérifier les fichiers avant d'écraser la configuration, la boutique n'est pas tombée.

![ansible](../../../docs/images/troubleshooting/07-ansible-cert-path.png)

## Impressions

Ansible vérifie et corrige désormais à chaque fois les erreurs que j'avais faites à la main au début (permissions 700, mbstring). Le jour où j'ai compris dans ma chair pourquoi on automatise.
