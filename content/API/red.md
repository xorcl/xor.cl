---
title: API Red Movilidad
date: 2020-06-01
subtitle: ¿Cuánto falta a la micro?
image: https://upload.wikimedia.org/wikipedia/commons/3/32/Red_Metropolitana_de_Movilidad.png
---

Scraping de la página [Bip! en línea](http://pocae.tstgo.cl/PortalCAE-WAR-MODULE/) que entrega en JSON el saldo y contratos activos de una tarjeta. El saldo en esta plataforma puede tener unas horas de desfase, debido a la forma en que funciona la actualización de datos en la plataforma de tarjetas Bip!.


**URL**: `https://api.xor.cl/bip/`

**Parámetros**:

* `n`: _(GET)_ Recibe el número de 8 dígitos de una tarjeta Bip!

**Ejemplo**: [https://api.xor.cl/bip/?n=00000001](https://api.xor.cl/bip/?n=00000001)

**Formato**:

* `raíz`
* `numeroBip`: _(number)_ Número de la tarjeta Bip! consultada. Si el parámetro _n_ no es un número, se cambia por un 0.
* `valida`: _(boolean)_ `true` si la tarjeta es válida. `false` si no. Que sea válida no significa que tenga contratos asociados.
* `estadoContrato`: _(string)_ Indica si el contrato de la tarjeta se encuentra activo (o sea, si la tarjeta está habilitada para pagar).
* `saldoBip`: _(string)_ Indica el saldo de la tarjeta. El número se encuentra formateado con `$` y `.`.
* `fechaSaldo`: _(string)_ La fecha de actualización del saldo, en formato `DD/MM/AA HH:MM`.
* `tiposContrato`: _(array)_ Lista de _(string)_ con los tipos de contrato de una tarjeta. Con esto, se puede determinar si la tarjeta es de estudiante o no.


**Notas**: Hay más campos posibles, como `CuotasTransporte` y `FechaVencimiento`, pero no entiendo muy bien qué significan.
