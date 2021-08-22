---
title: API Sismología
date: 2021-08-22
subtitle: ¿Está temblando?
image: /img/sismo.jpeg
---


Scraping de la página del [Centro Sismológico Nacional de la U. de Chile](http://sismologia.cl), que permite obtener en un formato amigable información de los sismos de cualquier fecha.

[Código Fuente](https://github.com/xorcl/api-sismo)

## URLs 

* **Sismos Recientes**: `https://api.xor.cl/sismo/recent`
* **Sismos Históricos**: `https://api.xor.cl/sismo/<fecha-sismo>`


## Parámetros

* `fecha-sismo`: _(GET, obligatorio para Sismos Históricos)_ Valor en formato YYYYMMDD que representa la fecha local de la cual se quieren ver los sismos.
* `magnitude`: _(GET, opcional para ambas URLs)_ Valor numérico que representa la magnitud mínima de los sismos a mostrar.

## Ejemplos 

* [https://api.xor.cl/sismo/recent?magnitude=5](https://api.xor.cl/sismo/recent?magnitude=5)
* [https://api.xor.cl/sismo/20100227?magnitude=8](https://api.xor.cl/sismo/20100227?magnitude=8)

## Formato

* `status_code` _(integer)_ Código de status. Si es 0, la solicitud se realizó de forma satisfactoria.
* `status_description` _(string)_ Explicación del error.
* `events`: _(array)_ Arreglo de objetos de tipo `event`.
* `event`:
    * `id`: _(string)_ ID que entrega la página de sismología a cada sismo. A veces varía entre la versión final y la versión preliminar.
    * `url`: _(string)_ Link a la página oficial de sismología con información del sismo.
    * `map_url`: _(string)_ Link a la imagen de sismología con el mapa del sismo.
    * `local_date`: _(string)_ Fecha del sismo en huso horario local en formato `YYYY/MM/DD HH:MM:SS`. El huso horario chileno es un enredo (cada año cambian las fechas de modificación de éste), así que no es muy conveniente usar este valor si se quiere medir tiempos entre sismos
    * `utc_date`: _(string)_ Fecha del sismo en UTC en formato `YYYY/MM/DD HH:MM:SS`.
    * `latitude`: _(float)_ Latitud del epicentro del sismo.
    * `longitude`: _(float)_ Longitud del epicentro del sismo.
    * `depth`: _(float)_ Profundidad en Km del sismo.
    * `magnitude`: _(magnitud)_ Objeto de tipo `magnitud`.
    * `geo_reference`: _(string)_ Texto que refiere a alguna localidad conocida, cercana al epicentro del sismo.
* `magnitud`:
    * `value`: _(float)_ Número que representa la magnitud del sismo.
    * `measure_unit`: _(string)_ Sigla que representa la unidad de medida del sismo.

## Tipos de errores

* **0**:  Información obtenida satisfactoriamente,
* **10**: Error indeterminado al interpretar parámetro,
* **11**: Parámetro Obligatorio event-date mal formado,
* **12**: Parámetro Opcional magnitude mal formado,
* **20**: Error indeterminado al interpretar información desde Sismologí,,
* **21**: Sismología no contesta,
* **22**: Sismología contesta, pero no entrega información interpretable,