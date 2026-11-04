# Day 10 - 2026.10.27 (mar) Cierre, GitHub

[한국어](../../../daily-log/Day-10.md) | [English](../../en/daily-log/Day-10.md) | [日本語](../../ja/daily-log/Day-10.md) | [简体中文](../../zh-CN/daily-log/Day-10.md) | **Español** | [Français](../../fr/daily-log/Day-10.md)

## Lo que hice hoy

- Pasé a un repositorio de GitHub el README, Topology, Session, Verification y Resources que había organizado en Notion
- Lo dividí en Session (Configuration) y Resources (zip, archivos de configuración) siguiendo las pestañas de Notion, y volví a comprobar que coincide con los valores realmente configurados
- Reuní las capturas de los problemas y redacté el Troubleshooting
- Sustituí todas las contraseñas por marcadores como `<ROOT_PASSWORD>`

## Mirando atrás

| Plan inicial | Realidad |
| --- | --- |
| OpenVPN | WireGuard (configuración más sencilla) |
| Grafana (https) detrás de nginx | Grafana 3000 http (queda como mejora pendiente) |
| exporter de apache·mysqld | Comprobar directamente la respuesta del servicio con blackbox |
| Lista de 8 productos | Tienda con 160 productos + comercio en directo |

## Próximos pasos

- Hacer que el acceso SSH use solo claves firmadas por la CA (eliminar las claves añadidas con ssh-copy-id)
- Pasar también a Ansible la configuración de FW, DNS y DB
- Grafana con https, replicación de la BD, FW redundante
