---
title: API Sismología
date: 2020-06-01
subtitle: ¿Está temblando?
image: /img/sismo.jpeg
---


Scraping de la página del [Centro Sismológico Nacional de la U. de Chile](http://sismologia.cl), que permite obtener en un formato amigable información de los sismos de cualquier fecha.


**URL**: `https://api.xor.cl/sismo/`

**Parámetros**:

* `fecha`: _(GET, opcional)_ Fecha en formato `YYYYMMDD` de la cual se quieren consultar los sismos. El horario es local (GMT-4 en invierno, GMT-3 en verano). Si no se define, se usa el día de la consulta.

**Ejemplo**: [https://api.xor.cl/sismo/?fecha=20100227](https://api.xor.cl/sismo/?fecha=20100227)

**Formato**:

* `raiz`: _(array)_ Arreglo de objetos de tipo `sismo`.
* `sismo`:
    * `id`: _(string)_ ID que entrega la página de sismología a cada sismo. A veces varía entre la versión final y la versión preliminar.
    * `enlace`: _(string)_ Link a la página oficial de sismología con información del sismo.
    * `imagen`: _(string)_ Link a la imagen de sismología con el mapa del sismo.
    * `preliminar`: _(string)_ Si es `true`, es información preliminar y todavía no confirmada. Puede actualizarse a futuro cambiando `preliminar`a `false` y manteniendo el ID, pero a veces Sismología no mantiene ese valor.
    * `fechaLocal`: _(string)_ Fecha del sismo en huso horario local en formato `YYYY/MM/DD HH:MM:SS`. El huso horario chileno es un enredo (cada año cambian las fechas de modificación de éste), así que no es muy conveniente usar este valor si se quiere medir tiempos entre sismos
    * `fechaUTC`: _(string)_ Fecha del sismo en UTC en formato `YYYY/MM/DD HH:MM:SS`.
    * `latitud`: _(float)_ Latitud del epicentro del sismo.
    * `longitud`: _(float)_ Longitud del epicentro del sismo.
    * `profundidad`: _(float)_ Profundidad en Km del sismo.
    * `magnitudes`: _(array)_ Arreglo de objetos de tipo `magnitud`.
    * `georreferencia`: _(string)_ Texto que refiere a alguna localidad conocida, cercana al epicentro del sismo.
* `magnitud`:
    * `magnitud`: _(float)_ Número que representa la magnitud del sismo.
    * `medida`: _(string)_ Sigla que representa la unidad de medida del sismo.
    * `fuente`: _(string)_ Representa la fuente de la medida. GUC es _Geología Universidad de Chile_.