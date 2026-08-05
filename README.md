# TP_optimizacion

Este repositorio trata del TP de optimización de Investigación Operativa de
Ingeniería Industrial (UTN FRBA). El trabajo consiste en resolver un problema de
**Planificación Agregada de la Producción** (aggregate production planning),
minimizando el costo total anual de un plan de 12 meses.

El enfoque sigue el estilo de la cátedra I4051 —
[invo.py](https://github.com/TadeoSinay/invo.py) — modelando el problema como una
**Programación Lineal Entera Mixta (MILP)** y validándolo con **PuLP**.

## Contenido

| Archivo | Descripción |
|---|---|
| [`CONSIGNA.md`](CONSIGNA.md) | Consigna completa: contexto, datos, **formulación matemática** (conjuntos, parámetros, variables, función objetivo y restricciones), supuestos, actividades de análisis (sensibilidad, estrategias, escenarios), entregables, rúbrica y guía de implementación en PuLP. |
| [`index.html`](index.html) | Planilla interactiva de planificación agregada: carga de parámetros y datos mensuales, validación de restricciones en vivo, desglose de costos y gráficos. Incluye el panel de consigna con el modelo formal. |

## Cómo usar la planilla

Abrí `index.html` en el navegador. Navegá entre las secciones **Consigna**,
**Datos de entrada** y **Desglose y gráfico**. Completá los parámetros y la
planilla mensual: el costo total, las alertas de restricciones y los gráficos se
actualizan automáticamente.

## El problema en una línea

Decidir, mes a mes, **operarios, turnos, horas extra, tercerización e inventario**
para cubrir la demanda de ventas al **mínimo costo total**, respetando los límites
de plantilla, horas extra y capacidad de almacenamiento.
