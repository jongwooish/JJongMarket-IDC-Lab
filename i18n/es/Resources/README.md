# Resources

[한국어](../../../Resources/README.md) | [English](../../en/Resources/README.md) | [日本語](../../ja/Resources/README.md) | [简体中文](../../zh-CN/Resources/README.md) | **Español** | [Français](../../fr/Resources/README.md)

Paquetes de despliegue que enviaba con `scp` desde el PC anfitrión a cada VM durante la construcción. Tienen la misma estructura que los archivos que guardaba en la pestaña Resources de Notion y se pueden regenerar desde esta carpeta con `scripts/make-resources.sh`.

| Archivo | Contenido | Capítulo donde se usa |
| --- | --- | --- |
| `jjong-idc-lab_WEB.zip` | PHP de la tienda, configuración del sitio Apache, ejemplo de db.ini, shop.sql | 11 MariaDB, 14 WEB |
| `jjong-idc-lab_Streaming.zip` | Vídeo de JJong LIVE (3,6 MB), jjonglive.service | 15 Streaming |
| `jjong-idc-lab_Monitoring.zip` | blackbox, prometheus, reglas de alerta, provisioning de Grafana, JSON del panel | 16 Monitoring |
| `jjong-idc-lab_Ansible.zip` | Playbooks que se descomprimen tal cual en `/etc/ansible` de MGMT | 17 Ansible |

Los archivos originales de dentro de los zip también se incluyen como carpetas.

| Carpeta | Contenido |
| --- | --- |
| `configs/` | Archivos de configuración por capítulo (network, dhcp, nftables, dns, wireguard, ssh, chrony, mariadb, backup, keepalived, web, streaming, monitoring) |
| `ansible/` | Playbooks (`common.yml`, `web.yml`, `site.yml`) |
| `web/` | PHP de la tienda |
| `live/` | Vídeo de JJong LIVE y archivos fuente |
| `scripts/` | Scripts para generar los zip |

Los archivos externos que no están en el repositorio se descargan aparte.

| Archivo | Dónde obtenerlo |
| --- | --- |
| `grafana-enterprise_8.3.4_amd64.deb` | [Descarga en grafana.com](https://grafana.com/grafana/download/8.3.4?edition=enterprise) |
| `hls.min.js` | [jsDelivr hls.js](https://cdn.jsdelivr.net/npm/hls.js@1/dist/hls.min.js) |

Notion limita cada archivo a 5 MB, así que subí el deb de Grafana comprimido en partes de 4 MB.
