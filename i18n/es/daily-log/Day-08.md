# Day 08 - 2026.10.23 (vie) La tienda, y un día de errores

[한국어](../../../daily-log/Day-08.md) | [English](../../en/daily-log/Day-08.md) | [日本語](../../ja/daily-log/Day-08.md) | [简体中文](../../zh-CN/daily-log/Day-08.md) | **Español** | [Français](../../fr/daily-log/Day-08.md)

## Lo que hice hoy

- Mañana: publiqué una tienda HTML sencilla (v1), pero en CLIENT todo el coreano salía como cuadrados
- El diseño se quedaba muy corto, así que rehíce la tienda (v2): tema amarillo, 160 productos, 800 reseñas, búsqueda, carrito, pedidos, envío simulado, publicación de productos, registro de tarjetas, membresía, ventanas emergentes de publicidad
- Noche: publiqué la v2 y resolví uno tras otro el error 500, la página de detalle rota y el error de registro

![v1](../../../docs/images/troubleshooting/01-hangul-broken.png)

## Dónde me atasqué

| Síntoma | Causa | Solución |
| --- | --- | --- |
| Coreano como cuadrados | Sin fuente coreana en CLIENT | `fonts-noto-cjk` |
| Error 500 | La carpeta `inc` enviada con scp desde Windows tenía 700 | `chmod -R a+rX` |
| Detalle roto, error de registro | Falta `php-mbstring` | Instalar + funciones alternativas |
| Campos del pedido vacíos | `header.php` y la página usaban la misma variable `$f` | Renombrada a `$flash_msg` |

El error 500 no aparecía al ejecutar `php index.php` como root, pero con `runuser -u www-data` daba Permission denied, así que supe que era un problema de permisos. Fue la primera vez que reproduje un fallo cambiando el usuario que ejecuta.

![permission](../../../docs/images/troubleshooting/03-permission-denied.png)

Detalles en [troubleshooting.md](../docs/troubleshooting.md)

## Impresiones

Hoy fue el día más largo. Aun así, tras encontrar y arreglar cada causa una a una, me dio mucha satisfacción ver la tienda funcionar como una tienda de verdad.
