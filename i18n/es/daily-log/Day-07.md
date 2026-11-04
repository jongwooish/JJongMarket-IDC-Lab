# Day 07 - 2026.10.22 (jue) MariaDB, copias de seguridad, keepalived

[한국어](../../../daily-log/Day-07.md) | [English](../../en/daily-log/Day-07.md) | [日本語](../../ja/daily-log/Day-07.md) | [简体中文](../../zh-CN/daily-log/Day-07.md) | **Español** | [Français](../../fr/daily-log/Day-07.md)

## Lo que hice hoy

- MariaDB en DB, `bind-address = 10.10.20.10`, utf8mb4
- Script de copia de seguridad + cron, y MGMT la recoge con rsync
- VIP 10.10.10.100 con keepalived en WEB1·WEB2
- Emití el certificado web (www.crt) en MGMT e instalé Apache en WEB

## Lo que aprendí

- Sin `set -o pipefail` en el script, aunque `mariadb-dump` falle, el gzip posterior tiene éxito y queda una copia vacía marcada como "correcta".
- Para que MGMT no se lleve un archivo a medio escribir, lo escribí como `.tmp` y al terminar hice `mv`. rsync también usa `--exclude '*.tmp'`.
- cron ignora los archivos de `/etc/cron.d` con un punto en el nombre. Un nombre como `mariadb-backup.cron` no sirve.
- keepalived comprueba Apache cada 2 segundos y, si falla, baja la prioridad en 20. 110 - 20 = 90 queda por debajo del 100 de WEB2 y la VIP pasa a WEB2. Diseñar con números fue divertido.

## Para mañana

Publicar las páginas de la tienda
