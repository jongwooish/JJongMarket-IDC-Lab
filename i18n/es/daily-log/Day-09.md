# Day 09 - 2026.10.26 (lun) JJong LIVE, monitorización, Ansible

[한국어](../../../daily-log/Day-09.md) | [English](../../en/daily-log/Day-09.md) | [日本語](../../ja/daily-log/Day-09.md) | [简体中文](../../zh-CN/daily-log/Day-09.md) | **Español** | [Français](../../fr/daily-log/Day-09.md)

## Lo que hice hoy

- **JJong LIVE**: hice un vídeo de 1 minuto en el que la presentadora "Piyo" muestra ofertas, y lo emito en bucle infinito por HLS con ffmpeg en WEB1·WEB2
- **Monitorización**: node-exporter en 6 equipos, blackbox comprobando la tienda, JJong LIVE y la BD como si fuera un cliente, 6 reglas de alerta de Prometheus, panel de Grafana
- **Ansible**: playbooks de configuración común y despliegue web; en la segunda ejecución `changed=0`
- **Verification**: organicé las comprobaciones por capítulo y las pruebas de fallos

![jjonglive](../../../docs/images/jjonglive.gif)

## Lo que aprendí

- En lugar de montar un servidor de emisión aparte, cada uno de los dos servidores web emite por su cuenta. Con `-c copy` no se recodifica, así que apenas usa CPU. Si cae WEB1, la emisión continúa desde WEB2.
- blackbox no mira "si el servidor está encendido", sino "si el servicio responde". Un caso en el que solo cae Apache no lo detecta node-exporter.
- Al principio Ansible daba UNREACHABLE en todo menos MGMT. Cuando hacía ssh a mano, escribía yo el yes y la contraseña, pero Ansible no puede. Lo resolví con keyscan y distribuyendo claves.
- Después se paró en WEB1 porque la ruta del certificado era distinta. Gracias a que lo había hecho comprobar los archivos antes de sobrescribir la configuración, la tienda no cayó.

![ansible](../../../docs/images/troubleshooting/07-ansible-cert-path.png)

## Impresiones

Ahora Ansible comprueba y corrige cada vez los errores que cometí a mano al principio (permisos 700, mbstring). El día en que entendí en carne propia por qué se automatiza.
