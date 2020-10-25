---
title: API Tía Yoli As a Service
date: 2020-06-01
subtitle: Que nos vaiga bien
image: https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/Stars_%28Unsplash%29.jpg/800px-Stars_%28Unsplash%29.jpg
---

Scrapping del horóscopo de la Tía Yoli en la página [Login](https://login.cl/horoscopo).

**URL**: `https://api.xor.cl/tyaas/`

**Parámetros**: _Ninguno_

**Formato**:

* `raíz`
    - `titulo`: _(string)_ Suele ser el día y el mes del horóscopo
    - `horoscopo`: _(dictionary)_ diccionario con cero o más objetos de tipo `signo`. La llave es el nombre del signo en minúsculas y sin tildes.
    - `fuente`: _(string)_ URL a la página de fuente (Login).
    - `autor`: _(string)_ Siempre es Yolanda Sultana.
* `signo`:
    - `nombre`: _(string)_ Nombre legible del signo.
    - `fechaSigno`: _(string)_ Rango de fechas del signo en texto plano
    - `amor`: _(string)_ Destino en términos de amor. Termina en punto.
    - `salud`: _(string)_ Destino en términos de salud. Termina en punto.
    - `dinero`: _(string)_ Destino en términos de dinero. Termina en punto.
    - `color`: _(string)_ Color asignado al signo para este día. Termina en punto.
    - `numero`: _(string)_ Número asignado al signo para este día. Termina en punto y es una cadena de texto.

**Notas**: No hay API para obtener horóscopos anteriores,porque es de mala suerte.