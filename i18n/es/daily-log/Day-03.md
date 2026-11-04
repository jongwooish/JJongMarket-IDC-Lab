# Day 03 - 2026.10.16 (vie) Creación de VM, IP, reenvío

[한국어](../../../daily-log/Day-03.md) | [English](../../en/daily-log/Day-03.md) | [日本語](../../ja/daily-log/Day-03.md) | [简体中文](../../zh-CN/daily-log/Day-03.md) | **Español** | [Français](../../fr/daily-log/Day-03.md)

## Lo que hice hoy

- Creé VMnet0~3 Host-only en VMware y desactivé el DHCP (VMnet0 es Bridged por defecto, así que tuve que cambiarlo)
- Creé 8 VM, Basic Configuration (paquetes, permitir SSH de root, nombres de host)
- IP fija con `/etc/network/interfaces`
- Reenvío IP en el FW

## Lo que aprendí

- Aunque pusiera `net.ipv4.ip_forward=1` en el FW, a veces el reenvío estaba desactivado tras reiniciar. Hice que `rc.local` volviera a ejecutar `sysctl -p` en cada arranque.
- Por la tarde recibí en la empresa formación sobre instalación del sistema operativo de servidores según el fabricante y montaje. Lo aprendido sobre RAID e iLO en servidores reales me ayudó a imaginar "dónde y cómo están montados los servidores" en este proyecto.
