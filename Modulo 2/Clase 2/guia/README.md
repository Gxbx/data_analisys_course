# Dataset — Formula 1 2024 (primeros 4 Grandes Premios)

Este paquete se preparó para la **Clase 01 del Módulo 2 V2.0** con dos objetivos:

1. explicar de forma visual un **modelo estrella**;
2. construir un informe interactivo con **Bookmarks, Buttons, Drillthrough, Page Navigation y Tooltips personalizados**.

## Por qué este dataset

Formula 1 funciona muy bien para un ejercicio de BI porque tiene un hecho claro:

> **Una fila de la tabla de hechos representa el resultado de un piloto en una carrera.**

Y dimensiones intuitivas: piloto, escudería y carrera.

## Fuente de los datos

Repositorio público de referencia:

https://github.com/toUpperCase78/formula1-datasets

Archivos de origen utilizados:

- `Formula1_2024season_raceResults.csv`
- `Formula1_2024season_calendar.csv`
- `Formula1_2024season_drivers.csv`

Enlaces RAW:

https://raw.githubusercontent.com/toUpperCase78/formula1-datasets/master/Formula1_2024season_raceResults.csv

https://raw.githubusercontent.com/toUpperCase78/formula1-datasets/master/Formula1_2024season_calendar.csv

https://raw.githubusercontent.com/toUpperCase78/formula1-datasets/master/Formula1_2024season_drivers.csv

El repositorio indica como fuente principal el sitio oficial de Formula 1 y solicita atribución al autor del repositorio.

## Qué se modificó

Los valores deportivos no fueron inventados. El paquete:

- recorta la temporada 2024 a los primeros cuatro Grandes Premios;
- agrega `Track` al calendario como alias de unión didáctico;
- genera claves de carrera (`RaceKey`);
- crea `PositionNumeric` para poder calcular con posiciones clasificadas;
- crea `ResultStatus`;
- crea una versión corta del nombre de las escuderías (`TeamShort`);
- genera tablas de dimensiones listas para usar como respaldo.

## Carpetas

### `raw/`

Datos cercanos a la fuente para que el estudiante construya el modelo.

- `race_results_2024_first4gp.csv`
- `calendar_2024_first4gp.csv`
- `drivers_2024.csv`

### `model/`

Modelo estrella ya preparado para nivelación o recuperación rápida.

- `FactRaceResults.csv`
- `DimDriver.csv`
- `DimTeam.csv`
- `DimRace.csv`

## Modelo esperado

```text
DimDriver[DriverKey]  1 ───── * FactRaceResults[DriverKey]
DimTeam[TeamKey]      1 ───── * FactRaceResults[TeamKey]
DimRace[RaceKey]      1 ───── * FactRaceResults[RaceKey]
```

Dirección de filtro recomendada: **única, desde la dimensión hacia la tabla de hechos**.
