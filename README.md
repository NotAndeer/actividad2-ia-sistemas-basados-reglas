# Sistema Inteligente de Rutas para Transporte Masivo

Un sistema basado en conocimiento que utiliza técnicas de Inteligencia Artificial para encontrar la mejor ruta entre estaciones de un sistema de transporte masivo, considerando tiempos de viaje y costos por transbordo.

---

# Descripción

Este proyecto implementa un sistema inteligente de transporte basado en:

- Base de conocimiento representada mediante hechos.
- Motor de inferencia para búsqueda de rutas óptimas.
- Algoritmo A* (configurado con heurística cero, equivalente a Dijkstra).
- Encadenamiento hacia adelante (*Forward Chaining*).
- Gestión de transbordos entre líneas.

El sistema permite calcular la ruta de menor costo temporal entre dos estaciones de una red de transporte.

---

# Características

- Representación del conocimiento mediante hechos.
- Grafo bidireccional de estaciones.
- Búsqueda de rutas óptimas usando A*.
- Penalización por cambio de línea.
- Generación de detalles por tramo.
- Encadenamiento hacia adelante para inferencia de nuevas conexiones.
- Fácil ampliación de estaciones y líneas.

---

# Estructura del Proyecto

```text
.
├── sistema_transporte.py
└── README.md
```

---

# Red de Transporte

## Línea B1

```text
Portal Norte
    |
Calle 127
    |
Calle 100
    |
Calle 72
    |
Calle 45
    |
Av. Jimenez
    |
Portal Sur
```

## Línea B2

Ruta similar a la línea B1 con tiempos de recorrido distintos.

## Línea C1

```text
Portal Sur
    |
Av. Jimenez
    |
Calle 45
    |
Calle 72
    |
Calle 100
    |
Calle 127
    |
Portal Norte
```

## Líneas Complementarias

### D1

```text
Calle 100 <--> Usaquén <--> Calle 127
```

### E1

```text
Calle 45 <--> Centro <--> Av. Jimenez
```

---

# Funcionamiento

## Base de Conocimiento

Las conexiones se representan mediante tuplas con la siguiente estructura:

```python
(origen, destino, linea, tiempo_minutos)
```

Ejemplo:

```python
("Portal Norte", "Calle 127", "B1", 8)
```

Interpretación:

```text
La estación Portal Norte está conectada con Calle 127
mediante la línea B1 en un tiempo de 8 minutos.
```

---

## Construcción del Grafo

A partir de los hechos definidos, el sistema crea un grafo donde cada conexión se considera bidireccional.

Ejemplo:

```text
Portal Norte ⇄ Calle 127
```

Esto permite calcular rutas en ambos sentidos utilizando la misma información.

---

## Búsqueda de la Mejor Ruta

La operación principal del sistema es:

```python
buscar_mejor_ruta(origen, destino)
```

Retorna:

```python
camino
costo_total
detalles
```

### Ejemplo de uso

```python
camino, tiempo, detalles = sistema.buscar_mejor_ruta(
    "Portal Norte",
    "Portal Sur"
)
```

---

# Manejo de Transbordos

Cuando una ruta implica cambiar de línea, se aplica una penalización de tiempo definida por la constante:

```python
TIEMPO_TRANSBORDO = 5
```

Por ejemplo:

```text
B1 → E1
```

Genera:

```text
+5 minutos al tiempo acumulado
```

Esta característica permite modelar de forma más realista los desplazamientos dentro del sistema de transporte.

---

# Algoritmo de Búsqueda

El método implementado utiliza el algoritmo A*.

La heurística empleada es:

```python
h(n) = 0
```

Por lo tanto:

```text
A* = Dijkstra
```

Garantizando la obtención de la ruta de menor costo desde el origen hasta el destino.

---

# Encadenamiento hacia Adelante

La función:

```python
forward_chaining()
```

permite inferir nuevas conexiones transitivas dentro de una misma línea.

## Ejemplo

Si existen los hechos:

```python
("Portal Norte", "Calle 127", "B1", 8)

("Calle 127", "Calle 100", "B1", 7)
```

El sistema puede inferir:

```python
("Portal Norte", "Calle 100", "B1", 15)
```

De esta forma amplía automáticamente el conocimiento disponible.

---

# Ejecución

## Requisitos

- Python 3.8 o superior.

Verificar la instalación:

```bash
python --version
```

## Ejecutar el programa

```bash
python sistema_transporte.py
```

o

```bash
python3 sistema_transporte.py
```

---

# Casos de Prueba Incluidos

## Caso 1: Portal Norte a Portal Sur

```python
origen = "Portal Norte"
destino = "Portal Sur"
```

El sistema calcula la ruta más eficiente y muestra:

- Ruta completa.
- Tiempo total estimado.
- Detalle por tramos.

## Caso 2: Calle 100 a Centro

```python
origen = "Calle 100"
destino = "Centro"
```

Permite verificar el uso de rutas con posibles transbordos.

## Caso 3: Inferencia de Nuevos Hechos

```python
hechos_iniciales = [
    ("Portal Norte", "Calle 127", "B1", 8)
]
```

Se utiliza el mecanismo de encadenamiento hacia adelante para generar conexiones adicionales.

---

# Complejidad Computacional

## Construcción del Grafo

```text
O(E)
```

Donde:

```text
E = número de conexiones
```

## Búsqueda de Rutas

```text
O((V + E) log V)
```

Donde:

```text
V = número de estaciones
E = número de conexiones
```

## Encadenamiento hacia Adelante

En el peor caso:

```text
O(N²)
```

Donde:

```text
N = número de hechos conocidos
```

---

# Posibles Mejoras

- Implementar heurísticas reales para A*.
- Incorporar tráfico o congestión en tiempo real.
- Soportar horarios de operación.
- Integrar una base de datos para almacenamiento persistente.
- Agregar interfaz gráfica.
- Exponer funcionalidades mediante una API REST.
- Integrar mapas geográficos y sistemas GIS.
- Incorporar múltiples criterios de optimización (tiempo, costo, número de transbordos).

---

# Autores

Equipo de Trabajo

Proyecto académico desarrollado para la implementación de Sistemas Basados en Conocimiento e Inteligencia Artificial aplicada al transporte masivo.

---

# Licencia

Este proyecto se distribuye con fines educativos y académicos.
