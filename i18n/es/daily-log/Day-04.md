# Day 04 - 2026.10.19 (lun) DHCP, nftables

[한국어](../../../daily-log/Day-04.md) | [English](../../en/daily-log/Day-04.md) | [日本語](../../ja/daily-log/Day-04.md) | [简体中文](../../zh-CN/daily-log/Day-04.md) | **Español** | [Français](../../fr/daily-log/Day-04.md)

## Lo que hice hoy

- isc-dhcp-server en MGMT; comprobé que CLIENT recibe 10.10.30.100~150
- REMOTE está en otra red, así que su broadcast no llega a MGMT → relé DHCP con dnsmasq en el FW
- Escribí las reglas de nftables (drop por defecto, permitir solo lo necesario, DNAT externo de 80·443·53)

## Lo que aprendí

- A través de un relé, el servidor DHCP mira la dirección del relé (giaddr) dentro del paquete para elegir de qué subred asignar. Por eso dhcpd.conf necesita la subred 203.0.113.0.
- En el rango de REMOTE no puse `option routers` a propósito. Sin puerta de enlace, sin VPN no existe ningún camino hacia la red interna.
- Trabajando con nftables, varias veces me bloqueé con mis propias reglas y scp dejó de funcionar. Cogí la costumbre de hacer `systemctl stop nftables` solo mientras trabajo y volver a activarlo al terminar.
