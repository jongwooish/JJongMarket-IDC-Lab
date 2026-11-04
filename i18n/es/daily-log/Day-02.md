# Day 02 - 2026.10.15 (jue) Diseño

[한국어](../../../daily-log/Day-02.md) | [English](../../en/daily-log/Day-02.md) | [日本語](../../ja/daily-log/Day-02.md) | [简体中文](../../zh-CN/daily-log/Day-02.md) | **Español** | [Français](../../fr/daily-log/Day-02.md)

## Lo que hice hoy

- Dividí la red en 4 según su uso: OUTSIDE / SERVER / DB / MGMT
- Fijé las reglas de direcciones: `.254` de cada red es el FW, los servidores son `.10` `.21` `.22`, la VIP y los clientes `.100`
- Dibujé la topología y escribí el borrador del README; creé la página JJongMarket IDC en Notion

## Lo que me planteé al diseñar

- **Por qué separar la BD**: para que, si comprometen un servidor web, no pueda llegar directamente a la BD. En el FW basta con abrir red SERVER → DB 3306.
- **203.0.113.0/24**: es un rango reservado para documentación (RFC 5737), así que no coincide con direcciones reales de internet. Perfecto para el papel de exterior.
- **VPN**: al principio lo planeé con OpenVPN. (Lo cambié el Day 05)

![topology](../../../docs/images/topology.png)

## Para mañana

Crear las VM y configurar las IP
