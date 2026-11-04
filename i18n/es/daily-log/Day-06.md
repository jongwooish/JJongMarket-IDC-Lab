# Day 06 - 2026.10.21 (mié) WireGuard, SSH CA, chrony

[한국어](../../../daily-log/Day-06.md) | [English](../../en/daily-log/Day-06.md) | [日本語](../../ja/daily-log/Day-06.md) | [简体中文](../../zh-CN/daily-log/Day-06.md) | **Español** | [Français](../../fr/daily-log/Day-06.md)

## Lo que hice hoy

- wg0 (10.8.0.1) en el FW; REMOTE conecta el túnel con `nmcli connection import`
- Creé una SSH CA en MGMT y firmé la clave de root
- El DNS es la referencia de hora (local stratum 8); todos los demás se sincronizan con el DNS

## Lo que aprendí

- `AllowedIPs` de WireGuard significa algo distinto en cada sentido. En el FW es "direcciones que acepto de este par"; en REMOTE es "destinos que envío por el túnel". Como en REMOTE solo puse `10.10.0.0/16`, solo el tráfico interno va por el túnel.
- Los servidores internos tienen el FW como puerta de enlace por defecto, así que las respuestas hacia 10.8.0.2 vuelven por FW → wg0 sin añadir ninguna ruta.
- Con una SSH CA no hace falta instalar cada clave pública en cada servidor: basta con confiar en una sola clave pública de la CA. Eso sí, solo después, al revisar, descubrí que al copiar todo `/etc/ssl` también se había repartido la clave privada de la CA.
- Como no hay internet, hay que comentar la línea `pool` por defecto de chrony. Si no, intenta salir al exterior todo el rato.
