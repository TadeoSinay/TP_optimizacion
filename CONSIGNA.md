# 📖 TP · Tu plan de producción

**Planificación Agregada · Investigación Operativa — Ingeniería Industrial (UTN FRBA)**

> Este es el enunciado. La herramienta para resolverlo es la página **`index.html`**
> (planilla interactiva). El entregable se descarga desde ahí, en Excel.

---

## La situación

Trabajás en una fábrica que hace un solo producto. El año que viene tenés que
**planificar mes a mes** cómo vas a producir para **cubrir todas las ventas**.
Podés jugar con varias palancas: cuánta gente tenés, si abrís uno o dos turnos,
si hacés horas extra, si comprás afuera (tercerizás) o si producís de más y
guardás para después.

Todas esas decisiones **cuestan plata**. Tu trabajo es encontrar la combinación
que cubra la demanda y que te haga gastar **lo menos posible en todo el año**.
No hay una única respuesta "correcta": hay decisiones mejores y peores, y vas a
tener que **justificar las tuyas** frente a los otros equipos.

---

## Con qué arrancás (datos que ya te damos)

- Cada operario produce **20 piezas por hora**.
- Se trabaja 20 días al mes, en turnos de 6 horas → **120 horas por turno al mes**.
- Podés tener entre **8 y 18 operarios** (el año pasado había 10).
- Arrancás el año con **8.000 unidades** en el depósito.
- No podés guardar más de **45.000 unidades**.

**Costos:**

| Concepto | Cuánto |
|---|---|
| Producir una unidad | $20 |
| Hora normal de trabajo | $100 |
| Hora extra (máx. 32 h por operario y turno) | $120 |
| Tercerizar una unidad (comprar afuera) | $100 |
| Guardar una unidad a fin de mes | $7 |
| Contratar un operario | $25.000 |
| Desvincular un operario | $50.000 |

---

## Las dos cuentas que ya vienen resueltas

No tenés que deducir nada raro. Vos elegís las cantidades y la planilla hace las
cuentas sola.

**1) ¿Cuánto produzco en un mes?**

```
Producción = ( operarios × 120 horas × turnos + horas extra ) × 20 piezas
```

Cuánta gente pusiste a trabajar, por cuántas horas (según si abriste 1 o 2
turnos), más las horas extra que le sumes, todo por las 20 piezas que sale cada
hora de trabajo.

**2) ¿Qué pasa con el stock cada mes?**

```
Stock final = stock del mes anterior + lo que producís + lo que tercerizás − lo que vendés
```

Arrancás cada mes con lo que te sobró del anterior, sumás lo que fabricaste y lo
que compraste afuera, y restás la venta del mes. Lo que queda pasa al mes
siguiente. **Ojo:** ese stock final **no puede ser negativo** (siempre tenés que
cubrir la venta) ni pasarse de 45.000.

---

## La demanda que tenés que cubrir

| Ene | Feb | Mar | Abr | May | Jun | Jul | Ago | Sep | Oct | Nov | Dic |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 65.000 | 68.000 | 70.000 | 72.000 | 90.000 | 130.000 | 175.000 | 178.000 | 145.000 | 85.000 | 77.000 | 62.000 |

La demanda **no es pareja**: hay un pico fuerte en invierno (jul–ago) y meses
bajos en verano. Ahí está el desafío. ¿Producís de más antes y guardás?
¿Contratás gente para el pico y después la desvinculás? ¿Hacés horas extra?
¿Tercerizás? Cada camino cuesta distinto.

---

## Experimentá antes de decidir

La planilla te muestra al instante el costo y si tu plan cierra. Usala para
probar y comparar. Algunas ideas para arrancar:

- **¿Gente fija o gente al pico?** Probá subir operarios en jun–ago y
  desvincular después, contra mantener la dotación pareja todo el año. Mirá si
  te conviene pagar contratar/desvincular o cargar el pico con horas extra.
- **¿Producir antes y guardar?** Producí de más en los meses flojos y guardá
  stock para el pico. Compará el costo de guardar ($7/u) contra el de horas
  extra o tercerizar.
- **¿Cuánto tercerizar?** Cubrí parte de jul–ago comprando afuera. Es lo más
  caro por unidad ($100), pero a veces evita picos de costo. ¿Dónde está el
  equilibrio?

No hay una receta: se trata de que juegues, mires cómo evoluciona tu plan y
elijas con criterio.

---

## Qué tenés que entregar

**Un solo entregable:** tu planilla completa en **Excel** (botón verde
*Descargar mi Excel* en la página), **con tu justificación adentro**.

La justificación es un texto corto donde contás **por qué elegiste lo que
elegiste**. Con eso vamos a discutir y comparar las decisiones de cada equipo en
clase. Preguntas para guiarte (no hace falta responder todas):

- ¿Cómo manejaste el pico de julio–agosto?
- ¿Dotación estable o ajustada mes a mes? ¿Por qué?
- ¿En qué meses te convino guardar stock?
- ¿Cuál fue tu gasto más grande del año? ¿Se podía evitar?
- Si tuvieras que recortar un 10% el costo, ¿por dónde empezarías?
