# Resources

[한국어](../../../Resources/README.md) | [English](../../en/Resources/README.md) | [日本語](../../ja/Resources/README.md) | [简体中文](../../zh-CN/Resources/README.md) | [Español](../../es/Resources/README.md) | **Français**

Les paquets de déploiement que j'envoyais avec `scp` depuis le PC hôte vers chaque VM pendant la construction. Ils ont la même structure que les fichiers que je gardais dans l'onglet Resources de Notion, et peuvent être régénérés depuis ce dossier avec `scripts/make-resources.sh`.

| Fichier | Contenu | Chapitre concerné |
| --- | --- | --- |
| `jjong-idc-lab_WEB.zip` | PHP de la boutique, configuration du site Apache, exemple de db.ini, shop.sql | 11 MariaDB, 14 WEB |
| `jjong-idc-lab_Streaming.zip` | Vidéo JJong LIVE (3,6 Mo), jjonglive.service | 15 Streaming |
| `jjong-idc-lab_Monitoring.zip` | blackbox, prometheus, règles d'alerte, provisioning Grafana, JSON du tableau de bord | 16 Monitoring |
| `jjong-idc-lab_Ansible.zip` | Playbooks à décompresser tels quels dans `/etc/ansible` de MGMT | 17 Ansible |

Les fichiers d'origine contenus dans les zip sont aussi fournis sous forme de dossiers.

| Dossier | Contenu |
| --- | --- |
| `configs/` | Fichiers de configuration par chapitre (network, dhcp, nftables, dns, wireguard, ssh, chrony, mariadb, backup, keepalived, web, streaming, monitoring) |
| `ansible/` | Playbooks (`common.yml`, `web.yml`, `site.yml`) |
| `web/` | PHP de la boutique |
| `live/` | Vidéo JJong LIVE et sources de production |
| `scripts/` | Scripts de génération des zip |

Les fichiers externes non inclus dans le dépôt sont à télécharger séparément.

| Fichier | Où le trouver |
| --- | --- |
| `grafana-enterprise_8.3.4_amd64.deb` | [Téléchargement sur grafana.com](https://grafana.com/grafana/download/8.3.4?edition=enterprise) |
| `hls.min.js` | [jsDelivr hls.js](https://cdn.jsdelivr.net/npm/hls.js@1/dist/hls.min.js) |

Notion limite chaque fichier à 5 Mo, j'ai donc envoyé le deb de Grafana en archive découpée en parties de 4 Mo.
