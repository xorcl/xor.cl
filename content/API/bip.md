---
title: API Saldo Bip!
date: 2020-06-01
subtitle: $$$
image: https://upload.wikimedia.org/wikipedia/commons/9/9a/Dibujo_Tarjeta_Bip%21.png
---

Scrapping de la página [m.ibus.cl](http://m.ibus.cl), que entrega en JSON el tiempo de llegada de los buses de Transantiago a un paradero.

**URL**: `https://api.xor.cl/ts/`

**Parámetros**:

* `paradero`: _(GET)_ recibe el código de un paradero.

**Ejemplo**: [https://api.xor.cl/ts/?paradero=pa444](https://api.xor.cl/ts/?paradero=pa444)

**Formato**: 

* `raíz`
    * `horaConsulta`: _(string)_ Hora de la consulta en formato _HH:mm_.
    * `id`: _(string)_ ID de paradero entregado si es que este es válido. Si no, es el string _"NULL"_.
    * `descripcion`: _(string)_ si `id`es _"NULL"_, este valor es el error ocurrido. Si no, es el nombre de la parada.
    * `servicios`:  _(list)_ Lista de objetos de tipo `servicio`.
* `servicio`:
    * `valido`: _(number)_ 1 si el servicio es válido, 0 si no.
    * `servicio`: _(string)_ Código del servicio que pasa por la parada. Si `valido` es 0, este valor no existe.
    * `patente`: _(string)_ Patente del servicio que pasa por la parada. Si `valido` es 0, este valor no existe.
    * `tiempo`: _(string)_ Cadena de texto que representa un estimado del tiempo que demorará el bus en pasar por la parada. Si `valido` es 0, este valor no existe.
    * `distancia`_(string)_ Cadena de texto que rerpesenta la distancia (en metros) que demorará el bus en pasar por la parada. Lleva la cadena _"mts."_ al final del valor. Si `valido` es 0, este valor no existe.
    * `descripcionError`: _(string)_ texto explicando por qué este servicio no es válido. Si `valido` es 1, este valor no existe.