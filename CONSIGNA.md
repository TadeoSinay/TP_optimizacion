# 📄 Consigna — Planificación Agregada de la Producción

**Trabajo Práctico de Optimización · Investigación Operativa (I4051)**
Ingeniería Industrial — UTN FRBA

> Documento de enunciado. Define el problema, el modelo y los entregables.
> **No contiene la resolución**: el óptimo se obtiene con la planilla `index.html`
> y/o con un modelo de programación matemática (ver *Guía de implementación*).

---

## 🟫 1. Contexto

Una empresa manufacturera debe planificar su producción para el **próximo año**
(horizonte de 12 meses, con períodos mensuales). El objetivo es **minimizar el
costo total anual** cubriendo un plan de ventas conocido, decidiendo mes a mes:

- cuántos **operarios** emplear (contratando o desvinculando respecto del mes anterior),
- cuántos **turnos** activar (1 o 2),
- cuántas **horas extra** utilizar,
- cuánto **tercerizar**,
- y cuánto **inventario** arrastrar entre meses.

Este es un problema clásico de **Planificación Agregada** (*aggregate production
planning*), resoluble con **Programación Lineal Entera Mixta (MILP)**.

---

## 🎯 2. Objetivo

Encontrar el plan de producción de **costo total mínimo** que satisfaga la demanda
mensual, respetando las restricciones de personal, capacidad y almacenamiento.

---

## 📊 3. Datos del problema

### 3.1. Parámetros de producción y personal

| Concepto | Símbolo | Valor |
|---|---|---|
| Productividad | $p$ | 20 piezas / operario / hora |
| Días laborables por mes | — | 20 días |
| Horas por turno-mes | $h$ | 120 h (20 días × 6 h) |
| Máx. horas extra por operario y turno | $\overline{o}$ | 32 h |
| Mínimo de operarios | $\underline{W}$ | 8 |
| Máximo de operarios | $\overline{W}$ | 18 |
| Operarios del año anterior | $W_0$ | 10 |
| Inventario inicial | $I_0$ | 8.000 u |
| Capacidad máxima de almacenamiento | $\overline{I}$ | 45.000 u |

### 3.2. Costos

| Concepto | Símbolo | Valor |
|---|---|---|
| Contratar un operario | $c^{H}$ | \$25.000 |
| Desvincular un operario | $c^{F}$ | \$50.000 |
| Hora normal de trabajo | $c^{N}$ | \$100 / h |
| Hora extra | $c^{E}$ | \$120 / h |
| Producir una unidad | $c^{P}$ | \$20 / u |
| Almacenar una unidad (fin de mes) | $c^{I}$ | \$7 / u |
| Tercerizar una unidad | $c^{S}$ | \$100 / u |

### 3.3. Plan de ventas (demanda $D_t$, en unidades)

| Ene | Feb | Mar | Abr | May | Jun | Jul | Ago | Sep | Oct | Nov | Dic |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 65.000 | 68.000 | 70.000 | 72.000 | 90.000 | 130.000 | 175.000 | 178.000 | 145.000 | 85.000 | 77.000 | 62.000 |

---

## 🧮 4. Modelo matemático (MILP)

> Esta es la **formulación del problema**, no su solución. Se pide que cada grupo
> la comprenda y la implemente.

### 4.1. Conjuntos e índices

$$t \in \mathcal{T} = \{1, 2, \dots, 12\} \quad \text{(meses)}$$

### 4.2. Variables de decisión

| Variable | Descripción | Dominio |
|---|---|---|
| $W_t$ | operarios activos en el mes $t$ | entero, $\underline{W} \le W_t \le \overline{W}$ |
| $H_t$ | operarios contratados en $t$ | entero $\ge 0$ |
| $F_t$ | operarios desvinculados en $t$ | entero $\ge 0$ |
| $s_t$ | turnos activos en $t$ | entero, $s_t \in \{1, 2\}$ |
| $O_t$ | horas extra totales en $t$ | continuo $\ge 0$ |
| $T_t$ | unidades tercerizadas en $t$ | continuo $\ge 0$ |
| $I_t$ | inventario al cierre del mes $t$ | continuo $\ge 0$ |
| $P_t$ | producción propia en $t$ (auxiliar) | continuo $\ge 0$ |

### 4.3. Función objetivo

Minimizar el costo total anual:

$$
\min Z = \sum_{t \in \mathcal{T}} \Big(
\underbrace{c^{P} P_t}_{\text{producción}} +
\underbrace{c^{N} \, h \, W_t s_t}_{\text{mano de obra normal}} +
\underbrace{c^{E} O_t}_{\text{horas extra}} +
\underbrace{c^{S} T_t}_{\text{tercerización}} +
\underbrace{c^{I} I_t}_{\text{almacenamiento}} +
\underbrace{c^{H} H_t}_{\text{contratación}} +
\underbrace{c^{F} F_t}_{\text{desvinculación}}
\Big)
$$

### 4.4. Restricciones

**(R1) Balance de personal** — con $W_0 = 10$:
$$W_t = W_{t-1} + H_t - F_t \qquad \forall t \in \mathcal{T}$$

**(R2) Límites de plantilla:**
$$\underline{W} \le W_t \le \overline{W} \qquad \forall t \in \mathcal{T}$$

**(R3) Turnos discretos:**
$$s_t \in \{1, 2\} \qquad \forall t \in \mathcal{T}$$

**(R4) Tope de horas extra** — proporcional a operarios y turnos:
$$O_t \le \overline{o} \, W_t s_t \qquad \forall t \in \mathcal{T}$$

**(R5) Definición de producción propia:**
$$P_t = p \, \big( h \, W_t s_t + O_t \big) \qquad \forall t \in \mathcal{T}$$

**(R6) Balance de inventario** — con $I_0 = 8.000$:
$$I_t = I_{t-1} + P_t + T_t - D_t \qquad \forall t \in \mathcal{T}$$

**(R7) Capacidad de almacenamiento:**
$$0 \le I_t \le \overline{I} \qquad \forall t \in \mathcal{T}$$

**(R8) Satisfacción de la demanda** (sin faltantes / *backorders*):
$$I_t \ge 0 \qquad \forall t \in \mathcal{T}$$

**(R9) Dominios:**
$$W_t, H_t, F_t \in \mathbb{Z}_{\ge 0}, \quad s_t \in \{1,2\}, \quad O_t, T_t, I_t, P_t \ge 0$$

> 💡 **Nota de modelado (alto valor).** Los términos $W_t s_t$ (en R4, R5 y en la FO)
> son **bilineales** porque multiplican dos variables de decisión. Para resolverlo
> como MILP puro hay dos caminos:
> 1. **Linealizar** definiendo variables auxiliares $u_t = W_t s_t$ con restricciones
>    de tipo *big-M* que activan el segundo turno mediante una binaria, o
> 2. **Redefinir** la unidad de decisión como "operario-turno" (variable entera
>    $\hat{W}_t = W_t s_t$) acotada por $\underline{W} \le \hat{W}_t \le 2\overline{W}$,
>    reconstruyendo operarios y turnos a posteriori.
>
> Discutir en el informe qué representación se eligió y por qué.

---

## 📌 5. Supuestos

1. La demanda debe cubrirse **íntegramente** en cada mes (no se admiten faltantes).
2. El inventario final de cada mes puede usarse para cubrir demanda futura.
3. Operarios y contrataciones/desvinculaciones son **cantidades enteras**.
4. Las horas extra se cargan sobre la plantilla y los turnos vigentes del mes.
5. La tercerización cubre unidades a costo unitario fijo, sin límite superior.
6. Todos los costos son lineales respecto de las cantidades.

---

## 🔵 6. Actividades y preguntas de análisis

### Parte A — Plan agregado óptimo (obligatoria)
1. Cargar los datos y **completar la planilla** (`index.html` o Excel) buscando el
   costo mínimo.
2. Presentar el **plan agregado** mes a mes (operarios, turnos, horas extra,
   producción, tercerización, stock y costos).
3. **Informe** justificando cada decisión y el desglose del costo total.

### Parte B — Análisis de sensibilidad (alto valor)
4. **Precio sombra** de la capacidad de almacenamiento $\overline{I}$: ¿cuánto
   cambiaría el costo si se ampliara el depósito? ¿Es una restricción activa?
5. **Cuello de botella de plantilla:** ¿el límite $\overline{W}=18$ operarios está
   activo en los meses pico (jul–ago)? ¿Convendría elevarlo?
6. **Umbral de tercerización:** ¿a partir de qué costo $c^{S}$ dejaría de convenir
   tercerizar en los meses de mayor demanda?

### Parte C — Comparación de estrategias y escenarios (alto valor)
7. Comparar el óptimo contra dos políticas heurísticas clásicas:
   - **Nivelada (*level*):** plantilla y producción constantes + stock/tercerización.
   - **Persecución (*chase*):** ajustar plantilla a la demanda de cada mes.
   Cuantificar la brecha de costo frente al óptimo.
8. **Escenario de estrés:** si la demanda pico (jul–ago) crece un **10 %**,
   ¿cómo se reconfigura el plan y cuánto sube el costo?
9. **Robustez:** identificar los 2–3 parámetros a los que el costo total es más
   sensible.

---

## 📦 7. Entregables

| # | Entregable | Formato |
|---|---|---|
| 1 | Plan agregado óptimo (planilla completa) | `index.html` / Excel / PDF |
| 2 | Informe de resultados con justificación de decisiones | PDF |
| 3 | Análisis de sensibilidad (Parte B) | PDF |
| 4 | Comparación de estrategias y escenarios (Parte C) | PDF |
| 5 | Modelo implementado (opcional, recomendado) | Notebook `.ipynb` (PuLP) |

Cada grupo debe **presentar su esquema al resto de los equipos**, explicando las
decisiones tomadas.

---

## 🧾 8. Rúbrica de evaluación (orientativa)

| Criterio | Peso |
|---|---:|
| Correctitud del plan y del costo total | 30 % |
| Formulación y comprensión del modelo (R1–R9) | 20 % |
| Análisis de sensibilidad (Parte B) | 20 % |
| Comparación de estrategias / escenarios (Parte C) | 15 % |
| Claridad del informe y de la presentación | 15 % |

---

## 🐍 9. Guía de implementación (PuLP)

Alineado con el enfoque de la cátedra ([invo.py](https://github.com/TadeoSinay/invo.py)),
se recomienda validar la planilla con un modelo en **PuLP**:

```python
import pulp

meses = range(12)
D = [65000, 68000, 70000, 72000, 90000, 130000,
     175000, 178000, 145000, 85000, 77000, 62000]

# Parámetros
p, h, o_max = 20, 120, 32
W_min, W_max, W0 = 8, 18, 10
I0, I_max = 8000, 45000
cH, cF, cN, cE, cP, cI, cS = 25000, 50000, 100, 120, 20, 7, 100

m = pulp.LpProblem("planificacion_agregada", pulp.LpMinimize)

# Variables de decisión (definir W, H, F, s, O, T, I, P según la sección 4.2)
# Restricciones (R1–R9) y función objetivo (4.3)
# ...  <-- a completar por cada grupo (esto es la CONSIGNA, no la solución)

# m.solve(pulp.PULP_CBC_CMD(msg=True))
# print("Costo total:", pulp.value(m.objective))
```

> El esqueleto anterior **no incluye la solución**: define la estructura para que
> cada grupo complete variables, restricciones y objetivo a partir del modelo de
> la sección 4.

---

## 📚 10. Bibliografía sugerida

- Hillier, F. & Lieberman, G. — *Introduction to Operations Research*.
- Taha, H. — *Investigación de Operaciones*.
- Winston, W. & Goldberg, J. — *Operations Research: Applications and Algorithms*.
- Cátedra I4051 — [invo.py](https://github.com/TadeoSinay/invo.py) · sección *Optimización*.

---

<sub>Convención de íconos (invo.py): 🟫 Teoría · 🔵 Práctica · 🔴 Casos.
Licencia del material de referencia: CC BY-SA 4.0.</sub>
