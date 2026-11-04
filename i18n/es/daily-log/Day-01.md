# Day 01 - 2026.10.14 (mié) Planificación

[한국어](../../../daily-log/Day-01.md) | [English](../../en/daily-log/Day-01.md) | [日本語](../../ja/daily-log/Day-01.md) | [简体中文](../../zh-CN/daily-log/Day-01.md) | **Español** | [Français](../../fr/daily-log/Day-01.md)

## Lo que hice hoy

- En mi primera salida de las prácticas, fui a instalar servidores al IDC de SK Broadband en Seocho-dong
- Por la tarde volví a la oficina y decidí la dirección de mi proyecto personal
- Me suscribí a Claude Pro, creé el proyecto de Claude "쫑" (JJong) y conecté mis notas de Notion (IT Network System)

## Por qué un IDC

Era la primera vez que entraba en un IDC. Al ver los servidores montados en los racks y los servidores web y la base de datos separados detrás de un firewall, me di cuenta de que los servicios que hasta entonces montaba por separado, ejercicio a ejercicio, en el entrenamiento para la competición de habilidades, en realidad funcionan unidos.
Por eso decidí no "resolver ejercicios", sino construir de principio a fin "la infraestructura de una empresa".

## Plan del proyecto

### Situación

JJongMarket S.A. es una tienda online con unos 20 empleados. Como el coste de la nube no paraba de subir, decidió alquilar un rack en un IDC y gestionar sus propios servidores. Yo soy el responsable de infraestructura.

### Requisitos

| ID | Requisito | Cómo |
| --- | --- | --- |
| R1 | Acceder a la tienda en `https://www.jjongmarket.kr` desde internet | FW DNAT → VIP web → Apache |
| R2 | Mantener la tienda aunque caiga un servidor web | keepalived VRRP |
| R3 | Abrir solo los puertos necesarios | nftables con bloqueo por defecto |
| R4 | Gestión de servidores solo desde la red de gestión; teletrabajo por VPN | VPN → solo MGMT permitido |
| R5 | Respuestas DNS distintas dentro y fuera | BIND9 view |
| R6 | Guardar y respaldar los datos de productos | MariaDB + volcado diario + copia en otro servidor |
| R7 | Estado en una sola pantalla | Prometheus + Grafana |
| R8 | Que el siguiente responsable pueda reconstruirlo | Documentación + Ansible |
| R9 | Sin avisos de HTTPS | Root CA privada |
| R10 | Hora sincronizada en todos los servidores | chrony |

### Restricciones

- VMware Workstation, Debian 13.2, 8 VM (6 servidores CLI, 2 clientes GUI)
- Solo redes Host-only. Se supone que no hay internet; el adaptador NAT se conecta solo un momento para instalar paquetes
- A diferencia de la práctica para competiciones, "que funcione" no es el final: tengo que poder explicar por qué lo hice así

## Impresiones

Es mi primer proyecto usando IA. Decidí no copiar respuestas, sino preguntar lo que no sé, entenderlo y después escribirlo yo mismo.
