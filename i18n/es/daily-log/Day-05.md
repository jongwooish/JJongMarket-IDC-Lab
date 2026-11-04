# Day 05 - 2026.10.20 (mar) CA, DNS, cambio de VPN

[한국어](../../../daily-log/Day-05.md) | [English](../../en/daily-log/Day-05.md) | [日本語](../../ja/daily-log/Day-05.md) | [简体中文](../../zh-CN/daily-log/Day-05.md) | **Español** | [Français](../../fr/daily-log/Day-05.md)

## Lo que hice hoy

- Root CA privada en MGMT (`CA.pl -newca`); RootCA.crt registrada como de confianza en todos los equipos
- DNS split-horizon con dos view de BIND9 (internal / external)
- Cambié la VPN de OpenVPN a **WireGuard**
- Empecé a organizar la Configuration en la pestaña Session de Notion con un desplegable por capítulo

## Por qué cambié a WireGuard

OpenVPN necesitaba certificados de servidor y cliente, una clave tls-crypt y muchos archivos de configuración. Con WireGuard cada lado solo necesita conocer la clave pública del otro, y la configuración ocupa unas 10 líneas. El objetivo de este proyecto es "un camino seguro hacia la red de gestión", así que elegí la opción más sencilla.

## Lo que aprendí

- El FW hace DNAT de las consultas externas hacia el DNS, pero la dirección de origen no cambia. Por eso el DNS reconoce la consulta como externa y responde con la external view. Al principio pensaba que, con DNAT, todo parecería venir de dentro.
- Con `recursion no`, si se pregunta por `jjong.internal` desde fuera, la respuesta es REFUSED. Los nombres internos no se filtran al exterior.
- Copié las plantillas de zona de la documentación ARM de bind9-doc con gpm, igual que hacía en las competiciones.

## Impresiones

A diferencia de resolver ejercicios, montarlo como en un trabajo real da muchos más errores. Pero me obliga a seguir preguntando "por qué", así que aprendo más.
