# Prueba 2 — solo motor original de Prueba 1

Esta versión utiliza exclusivamente el motor incluido en el `index.html` de Prueba 1.

En el gráfico solo pueden aparecer dos líneas producidas por ese código:

- resistencia morada;
- soporte azul.

Se eliminaron del código y de la interfaz:

- las líneas FV21 o Selector 21 incluidas en algunos CSV;
- la capa de referencia importada desde Finviz;
- las líneas discontinuas del laboratorio experimental;
- el trazado y ajuste manual de líneas;
- la calculadora cuantílica auxiliar;
- los generadores alternativos de candidatos.

## Parámetros originales conservados

- temporalidad: exclusivamente diaria (`1D`);
- ventana: 200 velas;
- últimas 20 velas excluidas al buscar el ancla;
- ancla superior: máximo de la ventana disponible;
- ancla inferior: mínimo de la ventana disponible;
- techo: regresión cuantílica al 97 %;
- piso: regresión cuantílica al 8 %;
- peso cuerpo–mecha: 0,35;
- mínimo: 20 velas;
- búsqueda de pendiente: amplitud 0,12 % y 81 pasos.

## Funciones auxiliares que permanecen

- carga simultánea de varios CSV;
- selector por ticker;
- memoria local de los CSV y del activo seleccionado;
- ejecución automática al cambiar de ticker;
- zoom, desplazamiento y exportación de los resultados del motor;
- bloqueo del cálculo fuera de la temporalidad diaria.

Los CSV solo necesitan las columnas `time, open, high, low, close`. Cualquier columna FV21 se ignora por completo.
