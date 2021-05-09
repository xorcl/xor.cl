---
title: API Red
date: 2020-06-01
subtitle: Saldo, Metro y Paraderos.
image: https://upload.wikimedia.org/wikipedia/commons/3/32/Red_Metropolitana_de_Movilidad.png
---

API con varios servicios relacionados con el sistema de transporte público de Santiago.

Su código fuente lo pueden encontrar [en este repositorio](https://github.com/xorcl/api-red), y se encuentra publicado bajo una licencia GPLv3.

## Saldo Bip!

Scrapping de la página [Carga Tu Bip!](https://cargatubip.metro.cl/CargaTuBipV2/) de Metro de Santiago con el saldo actualizado.

**URL**: GET `https://api.xor.cl/red/balance/<id-tarjeta>`

**Parámetros**: No tiene.

**Ejemplo**: [https://api.xor.cl/red/balance/87658765](https://api.xor.cl/red/balance/87658765)

**Formato**
* `raiz`
    * `id`: _(string)_ ID de la tarjeta. A pesar que actualmente los ID son solo numéricos, se prefirió dejar este campo como _string_ por si en el futuro cambian este estándar.
    * `status_code`: _(number)_ ID de status de la respuesta. Revisr más abajo para ver todos los estados posibles.
    * `status_description`: _(string)_ Descripción del `status_code` en español.
    * `balance`: _(number)_ Saldo de la tarjeta.

### Códigos de Estado

```go
var Errors = map[ErrorCode]string{
	0:  "Saldo obtenido satisfactoriamente",
	10: "Error indeterminado al interpretar parámetro Bip ID",
	11: "Parámetro Bip ID faltante",
	12: "Parámetro Bip ID mal formado",
	20: "Error indeterminado al parsear información desde RedBip",
	21: "RedBip no contesta",
	22: "RedBip contesta, pero no entrega información interpretable",
	23: "Imposible interpretar valor de saldo",
}
```

## Estado Red Metro

Scrapping de la página [Estado de la Red]() de Metro de Santiago con el estado en tiempo real de cada estación.

**URL**: GET `https://api.xor.cl/red/metro-network`

**Parámetros**: No tiene.

**Ejemplo**: [https://api.xor.cl/red/metro-network](https://api.xor.cl/red/metro-network)

**Formato**

* `raiz`:
    * `api_status`: _(string)_ Es OK si la API está funcionando.
    * `issues`: _(boolean)_ Es `true` si el campo `issues` de algúna línea es `true`. En caso contrario, es `false`.
    * `lines`: _(list)_ Recopila la lista de líneas publicadas. Una línea posee los siguientes campos:
        * `name`: _(string)_ Nombre en español de la línea.
        * `id`: _(string)_ Código de la línea. Compuesto por una L y el número de la línea.
        * `issues`: _(boolean)_ Es `true` si el campo `issues` de algúna estación es `true`. En caso contrario, es `false`.
        * `stations`: _(list)_ Recopila la lista de estaciones de la línea. Una estación posee los siguientes campos:
            * `name`: _(string)_ Nombre en español de la estación.
            * `id`: _(string)_ Código de la estación. Versión en minúscula y sin espacios ni tildes del nombre de la estación.
            * `status`: _(string)_ Define el estado de la estación. Los códigos se definen más abajo.
            * `lines`: _(list)_ Lista de IDs de líneas a las cuales está conectada esta estación.
            * `description` _(string)_ Descripción del estado de la línea.
            * `reason` _(string)_ Cuando `status` es distinto de cero, da más detalles sobre el estado de la línea.

### Códigos de estado

* `0`: Estación operativa
* `1`: Estación temporalmente cerrada
* `2`: Estación no habilitada
* `3`: Estación con accesos cerrados


## Paraderos

Scrapping de la página [m.ibus.cl](http://m.ibus.cl), que entrega en JSON el tiempo de llegada de los buses de Transantiago a un paradero.

**URL**: GET `https://api.xor.cl/red/bus-stop/<paradero>`

**Parámetros**:

* `paradero`: Recibe el código de un paradero.

**Ejemplo**: [https://api.xor.cl/red/bus-stop/PA433](https://api.xor.cl/red/bus-stop/PA433)

**Formato**: 

* `raíz`
    * `id`: _(string)_ ID de paradero entregado si es que este es válido. Si no, es un string vacío.
    * `descripcion`: _(string)_ Nombre de paradero. Si el ID es inválido, es un string vacío.
    * `status_code`: _(number)_ Código de status. Más abajo se detallan los valores posibles.
    * `status_description`: _(string)_ Explicación del código de status.
    * `services`: _(list)_ Lista con los recorridos que pasan por el paradero. Cada servicio tiene la siguiente estructura:
        * `id`: _(string)_ Identificador del recorrido.
        * `valid`: _(boolean)_ Es `true` si hay al menos un bus publicado en el itinerario para ese recorrido (es decir, si al menos un bus viene cerca).
        * `status_description`: _(string)_ Descripción del status del paradero en español.
        * `buses`: Lista con los buses próximos a pasar del recorrido. Los campos de cada bus son:
            * `id`: Patente del bus.
            * `meters_distance`: Distancia del bus en metros. 
            * `min_arrival_time`: Cuántos minutos mínimo faltan para que el bus pase. Si el texto entregado por el iBus es "menos de x minutos", este valor se configura en `0`.
            * `max_arrival_time`: Cuántos minutos máximo faltan para que el bus pase. Si el texto entregado por iBus es "más de x minutos", este valor se configura en `60`.


### Códigos de Estado

```go
var Errors = map[ErrorCode]string{
	0:  "Itinerario obtenido satisfactoriamente",
	10: "Error indeterminado al interpretar parámetro ID de Parada",
	11: "Parámetro ID de Parada faltante",
	12: "Parámetro ID de Parada mal formado",
	20: "Error indeterminado al parsear información desde SMSBus",
	21: "SMSBus no contesta",
	22: "SMSBus contesta, pero no entrega información interpretable",
	23: "Imposible interpretar valores de itinerario",
	30: "Error indeterminado con paradero",
}
```