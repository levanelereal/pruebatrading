# Informe técnico — Prueba 2 con solo el motor original

**Fecha:** 4 de agosto de 2026  
**Archivo principal:** `index.html`  
**Estado:** corregido y verificado

## 1. Objetivo de esta corrección

La versión anterior de Prueba 2 mezclaba el motor original de Prueba 1 con elementos visuales procedentes del
laboratorio experimental. Esa mezcla hacía aparecer líneas discontinuas moradas y azules que no pertenecían al
código original que se quería conservar.

La instrucción aplicada en esta revisión fue estricta: en el gráfico solamente deben existir las líneas producidas
por el motor original de Prueba 1.

El resultado actual solo permite crear:

1. una resistencia morada sólida;
2. un soporte azul sólido.

## 2. Elementos eliminados

Se retiraron tanto de la interfaz como del JavaScript las funciones que podían introducir otras líneas:

- lectura y dibujo de las cuatro salidas FV21 o Selector 21 contenidas en algunos CSV;
- carga y dibujo de un CSV de referencia de Finviz;
- capa de líneas discontinuas del laboratorio;
- generación experimental de múltiples candidatos;
- trazado manual de líneas mediante clics;
- ajuste de anclas arrastrando extremos;
- botón de deshacer líneas manuales;
- calculadora cuantílica interactiva;
- estado interno de líneas pendientes, líneas temporales y candidatos alternativos.

No se limitó la limpieza a ocultar controles. También se eliminaron los manejadores y las variables que ejecutaban
esas funciones. De este modo, las líneas auxiliares no pueden reaparecer por una interacción accidental ni por una
columna adicional incluida en un CSV.

## 3. Motor conservado

La función central `calcularPineScript` permanece idéntica a la que estaba incluida en el `index.html` de Prueba 1.
La comparación se hizo sobre el texto completo de la función y produjo una coincidencia exacta, con el mismo hash
SHA-256:

`dba084061df0b8070d4b59f5936f5f09e4ae2405fee91db74d4676d257099113`

Sus parámetros continúan fijos:

- ventana máxima de 200 velas;
- exclusión de las 20 velas más recientes para buscar las anclas;
- ancla de resistencia tomada del máximo de la ventana disponible;
- ancla de soporte tomada del mínimo de la ventana disponible;
- regresión cuantílica al 97 % para resistencia;
- regresión cuantílica al 8 % para soporte;
- zona cuerpo–mecha con peso 0,35;
- mínimo de 20 observaciones;
- búsqueda alrededor de la pendiente OLS;
- amplitud de pendiente de 0,12 %;
- 81 pasos de pendiente.

El motor sigue bloqueado fuera de la temporalidad diaria. La aplicación puede leer otros CSV, pero no ejecutará el
cálculo si la distancia temporal detectada no corresponde a `1D`.

## 4. Invariante aplicada

Antes de cada ejecución, la colección visual de líneas se reinicia por completo. Después se crean solamente los dos
resultados devueltos por el motor original. En términos funcionales:

1. se vacía el estado de líneas;
2. se localiza el máximo y el mínimo válidos;
3. se calcula la resistencia original;
4. se calcula el soporte original;
5. se incorporan únicamente esos dos objetos al gráfico.

No existe una segunda ruta de código que haga `push` de líneas. La única inserción presente corresponde al techo y
al piso generados por `crearLineaMotor`.

## 5. Funciones auxiliares que permanecen

Se conservaron las funciones que facilitan la auditoría sin cambiar la geometría:

- carga de uno o varios CSV de TradingView;
- selector por ticker cuando se cargan varios archivos;
- memoria local opcional de los CSV y del activo seleccionado;
- ejecución automática al cambiar de activo;
- zoom con la rueda;
- desplazamiento horizontal arrastrando el fondo;
- tabla de métricas de las dos líneas;
- exportación JSON del techo y el piso;
- exportación de un resumen legible.

Las columnas adicionales del CSV, incluidas las columnas FV21, no intervienen en el cálculo ni se dibujan.

## 6. Verificación técnica

Se realizaron las siguientes comprobaciones:

- sintaxis JavaScript válida;
- 29 referencias a controles HTML y 29 identificadores existentes;
- cero referencias a controles eliminados;
- cero llamadas a `setLineDash`, por lo que el motor no dibuja líneas discontinuas;
- una única ruta de creación de líneas, limitada al techo y al piso originales;
- coincidencia exacta de `calcularPineScript` con Prueba 1;
- prueba visual con el CSV `AMEX_SPY, 1D (12).csv`.

En la prueba de SPY se cargaron 323 velas diarias y se obtuvieron exactamente dos elementos en la lista:

- resistencia: ancla 2026-06-02, pendiente aproximada -0,0520 y valor actual 757,68;
- soporte: ancla 2026-03-30, pendiente aproximada +1,0969 y valor actual 743,45.

El gráfico mostró las dos líneas como trazos sólidos. No apareció ninguna línea FV21, de referencia, manual,
calculada o experimental.

## 7. Alcance de este resultado

Esta corrección no afirma que el motor original replique exactamente la selección interna de Finviz. Su propósito
es separar limpiamente el experimento: la página ahora muestra exclusivamente lo que calcula el código original de
Prueba 1. Con esta base se pueden comparar sus dos líneas contra Finviz sin que otras capas visuales contaminen la
lectura.
