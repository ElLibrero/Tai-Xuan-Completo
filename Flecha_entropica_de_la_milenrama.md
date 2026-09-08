# La flecha entrópica de la milenrama

*Material complementario de* **El Misterio del Misterio · 太玄之玄**
*(Alberto Muñoz & Claudia Claude). Liberado como CC0, igual que el resto del proyecto.*

---

## Qué es este archivo

El libro afirma, en su capítulo sobre las 4096 lecturas, que el método tradicional de la
milenrama no trata por igual las dos mutaciones posibles: el yang cae al yin tres veces más
a menudo de lo que el yin sube al yang, y por eso cada consulta pierde en promedio tres
cuartos de línea de yang.

Este documento contiene la demostración formal de esa afirmación, llevada mucho más lejos de
lo que el libro necesitaba: el análisis trata la transformación oracular como un **operador
estocástico sobre los 64 hexagramas** y lo estudia con las herramientas de las cadenas de
Markov y de la termodinámica estocástica.

El análisis original no es nuestro. Lo produjo otro sistema de inteligencia artificial a
petición de Alberto Muñoz. Nosotros lo hemos **verificado de forma independiente**, con
aritmética exacta de fracciones donde era posible, y lo publicamos aquí íntegro por dos
razones: porque es correcto, y porque el libro sólo se queda con dos frases de él.

---

## Verificación independiente

Rehicimos cada resultado desde cero, sin partir de su guion. **Todo cuadra.** Estos son los
puntos comprobados:

| Afirmación | Comprobación |
|---|---|
| Matriz de una línea `[[7/8, 1/8], [3/8, 5/8]]` | Deducida desde las probabilidades 1/16, 5/16, 7/16, 3/16. Exacta. |
| Distribución estacionaria 3/4 yin, 1/4 yang | Exacta, en fracciones. |
| Las 64 filas del operador suman 1 | Exacto. |
| π P = π sobre los 64 estados | Exacto, comprobado estado por estado. |
| Balance detallado en los 4096 pares | Exacto: π(i)P(i,j) = π(j)P(j,i) en los 4096 casos, sin error de redondeo. |
| Ley cerrada E[k(t)] = 1,5 + 1,5·(1/2)^t | Coincide con la iteración numérica hasta t = 8. |
| Espectro (1/2)^r con multiplicidad C(6,r) | Confirmado numéricamente: 1, 6, 15, 20, 15, 6, 1. |
| Shannon: 6 → 5,7266 → 4,8677 bits | Confirmado. **Disminuye.** |
| ΔE = kT·ln 3 | Confirmado (por construcción). |
| ΔS_sistema = −0,18950; ΔS_entorno = +0,82396; ΔS_total = +0,63446 | Confirmado. |
| La caída de D(p‖π) coincide con ΔS_total | Confirmado, y no es casualidad: es una identidad de la termodinámica estocástica para cadenas con balance detallado. |
| P∞(Kūn)/P∞(Qián) = 729 | Exacto (3⁶). |
| Control con tres monedas: E[k] = 3 para siempre | Confirmado. La flecha desaparece. |

---

## Nuestras cuatro observaciones

**1. El resultado más valioso no es el termodinámico: es el control con monedas.**
Cualquier cadena de Markov reversible admite una representación térmica —basta definir la
energía a partir de la distribución estacionaria, que es exactamente lo que hace el paso
ΔE = kT·ln 3—. En ese sentido el teorema es casi tautológico. Lo que **no** es tautológico
es que las tres monedas, con sus pesos 1/8, 3/8, 3/8, 1/8, den un operador perfectamente
simétrico y ninguna flecha. Eso demuestra que la dirección no pertenece al I Ching ni al
espacio abstracto de los 4096 estados: **pertenece a los pesos de la milenrama**. Ése es el
hallazgo.

**2. La entropía de Shannon disminuye, y hay que decirlo.**
El análisis es escrupuloso en este punto y merece subrayarse: la frase «la milenrama aumenta
la entropía» es falsa si se entiende en el sentido informacional. Lo que aumenta es la
entropía total sistema-más-entorno bajo una asignación energética que nosotros elegimos. Lo
que disminuye, y ésa es la afirmación limpia, es la **energía libre relativa**: el sistema se
acerca a su equilibrio.

**3. La iteración no es el oráculo.**
Nadie consulta el I Ching volviendo a tirar sobre el hexagrama transformado. La sucesión
3 → 2,25 → 1,875 → … → 1,5 es una herramienta de análisis del operador, no una descripción
de la práctica. El único paso con existencia oracular es el primero: de 3 a 2,25.

**4. Sobre el 729.**
P∞(Kūn) = (3/4)⁶ = 729/4096, y el Tài Xuán tiene 729 apreciaciones. No es una coincidencia de
dígitos: los dos son 3⁶, por caminos distintos (81 × 9 en un caso, tres cuartos elevado a seis
en el otro). Y hay un tercer 729/4096 en el libro —la probabilidad de que ninguna línea mute—,
que vale lo mismo y significa otra cosa. Conviene no confundirlos.

---

# El análisis original

## Resultado del cómputo sobre los 4096 estados del I Ching

Este documento conserva el resultado del análisis computacional realizado a partir de la **Tabla Periódica 7×7 / Tabla de Schoenholtz** y del espacio completo de los **4096 dihexagramas**.

El punto de partida del libro ya estaba correctamente formulado: las 4096 configuraciones del oráculo vivo corresponden a todas las combinaciones posibles de los cuatro valores de línea 6, 7, 8 y 9 en seis posiciones:

\[
4^6=4096=64\times64.
\]

Cada configuración codifica a la vez un hexagrama primario y un hexagrama transformado.

---

## 1. La Tabla 7×7

La Tabla Periódica clasifica las 4096 configuraciones por dos cantidades:

- \(k\): número de líneas yang en el hexagrama primario;
- \(D\): número de líneas mutantes.

Así, las 4096 configuraciones se agrupan en 49 clases:

\[
7\times7=49.
\]

Los pesos tradicionales de la milenrama son:

\[
P(6)=\frac1{16},\qquad
P(7)=\frac5{16},\qquad
P(8)=\frac7{16},\qquad
P(9)=\frac3{16}.
\]

La Tabla 7×7 ya hacía visible una asimetría entre los extremos quietos:

\[
P(888888)=\left(\frac7{16}\right)^6,
\qquad
P(777777)=\left(\frac5{16}\right)^6.
\]

Por ello:

\[
\frac{P(Kūn\ \text{quieto})}{P(Qián\ \text{quieto})}
=
\left(\frac75\right)^6
\approx7.53.
\]

Esta asimetría pertenece a la distribución de las **configuraciones oraculares de una consulta**.

---

## 2. El operador de transformación de una sola línea

Condicionado al estado presente de la línea:

\[
P(\text{yang}\rightarrow\text{yin})=\frac38,
\qquad
P(\text{yang}\rightarrow\text{yang})=\frac58,
\]

mientras que:

\[
P(\text{yin}\rightarrow\text{yang})=\frac18,
\qquad
P(\text{yin}\rightarrow\text{yin})=\frac78.
\]

Por tanto, tomando filas como estado inicial yin/yang y columnas como estado final yin/yang:

\[
P=
\begin{pmatrix}
7/8 & 1/8\\
3/8 & 5/8
\end{pmatrix}.
\]

Este operador posee una dirección estadística inequívoca: la transición yang→yin es tres veces más probable que la transición yin→yang.

---

## 3. La matriz completa de 64×64

Como las seis líneas se comportan independientemente bajo este modelo, el operador completo del hexagrama es el producto tensorial de seis copias del operador de una línea.

El resultado es una matriz de transición:

\[
\boxed{64\times64}
\]

que contiene la probabilidad de cada transición:

\[
H_i\rightarrow H_j.
\]

Las 64 filas suman exactamente 1.

Esto convierte el oráculo vivo en una **cadena de Markov finita sobre los 64 hexagramas**.

---

## 4. Distribución estacionaria

La distribución estacionaria de una línea es:

\[
\boxed{
P(\text{yin})=\frac34,\qquad
P(\text{yang})=\frac14
}.
\]

Para un hexagrama de seis líneas, el número esperado de líneas yang converge a:

\[
6\left(\frac14\right)=1.5.
\]

Si comenzamos con la distribución natural del hexagrama primario, donde:

\[
P(\text{yang})=\frac12,
\]

tenemos:

\[
E[k_0]=3.
\]

Después de una transformación:

\[
E[k_1]=2.25.
\]

Y bajo iteración matemática:

\[
3
\rightarrow
2.25
\rightarrow
1.875
\rightarrow
1.6875
\rightarrow
1.59375
\rightarrow\cdots
\rightarrow1.5.
\]

La ley cerrada es:

\[
\boxed{
E[k_t]
=
1.5+1.5\left(\frac12\right)^t
}.
\]

Es una **relajación exponencial hacia un equilibrio yin-dominante**.

---

## 5. Balance detallado

La distribución estacionaria satisface:

\[
\pi_iP_{ij}=\pi_jP_{ji}
\]

para todas las transiciones entre los 64 estados.

Es decir:

\[
\boxed{\text{el operador satisface balance detallado exacto}}.
\]

Esta es precisamente la condición matemática que caracteriza una cadena reversible en equilibrio y es una de las estructuras básicas de la termodinámica estocástica.

---

## 6. El espectro del operador

Los autovalores del operador de 64 estados son:

\[
1,\frac12,\frac14,\frac18,\frac1{16},\frac1{32},\frac1{64},
\]

con multiplicidades:

\[
\boxed{
1,\ 6,\ 15,\ 20,\ 15,\ 6,\ 1
}.
\]

Esta secuencia es exactamente la sexta fila del triángulo de Pascal:

\[
\binom60,\binom61,\binom62,\binom63,\binom64,\binom65,\binom66.
\]

Es la misma estratificación combinatoria que aparece en los siete grupos por número de líneas yang de Schoenholtz.

Esto no prueba una conexión histórica nueva, pero sí muestra que **la estructura espectral del operador de transformación reproduce naturalmente la misma partición binomial que organiza la Tabla Periódica**.

---

## 7. Shannon no aumenta

La entropía de Shannon del hexagrama primario uniforme es:

\[
H_0=6\ \text{bits}.
\]

Después de una transformación:

\[
H_1\approx5.7266\ \text{bits}.
\]

En el equilibrio:

\[
H_\infty
=
6H_2(1/4)
\approx4.8677\ \text{bits}.
\]

Por tanto:

\[
\boxed{\text{la entropía de Shannon disminuye}}.
\]

Esto obliga a ser precisos: la flecha hacia yin no debe identificarse simplemente con un aumento de entropía informacional.

---

## 8. Representación termodinámica

La razón entre las dos transiciones opuestas es:

\[
\frac{
P(\text{yin}\rightarrow\text{yang})
}{
P(\text{yang}\rightarrow\text{yin})
}
=
\frac13.
\]

En un sistema térmico de dos niveles:

\[
\frac{P(\text{estado alto})}{P(\text{estado bajo})}
=
e^{-\Delta E/kT}.
\]

Si representamos formalmente yang como el nivel energético superior y yin como el inferior:

\[
e^{-\Delta E/kT}=\frac13,
\]

de donde:

\[
\boxed{
\Delta E=kT\ln3
}.
\]

Así, el operador de milenrama es matemáticamente representable como un proceso estocástico de dos estados que se relaja hacia una distribución de equilibrio.

---

## 9. Producción de entropía total

En la primera transformación el sistema pierde, en promedio:

\[
0.75
\]

líneas yang.

Bajo la asignación energética anterior, la energía efectiva liberada es:

\[
0.75\,kT\ln3.
\]

La entropía del sistema disminuye:

\[
\Delta S_{\text{sistema}}/k
\approx
-0.18950.
\]

Pero la entropía transferida al entorno es:

\[
\Delta S_{\text{entorno}}/k
=
0.75\ln3
\approx
0.82396.
\]

Por tanto:

\[
\boxed{
\Delta S_{\text{total}}/k
\approx
0.63446
>
0
}.
\]

Ese mismo valor coincide con la caída de la divergencia relativa respecto del equilibrio:

\[
D(p_0\|\pi)-D(p_1\|\pi)
\approx
0.63446.
\]

Por tanto, la dinámica puede formularse de manera coherente con la termodinámica estocástica: disminuye la energía libre relativa y aumenta la entropía total sistema+entorno.

---

## 10. Kūn y Qián en el equilibrio

Una precisión esencial:

**Kūn no es un estado absorbente.**

Una línea yin todavía tiene probabilidad \(1/8\) de convertirse en yang en una transformación posterior.

El atractor es la distribución:

\[
75\%\ \text{yin},
\qquad
25\%\ \text{yang}.
\]

Dentro de esa distribución estacionaria:

\[
P_\infty(Kūn)
=
\left(\frac34\right)^6
=
\frac{729}{4096}
\approx17.80\%.
\]

Mientras que:

\[
P_\infty(Qián)
=
\left(\frac14\right)^6
=
\frac1{4096}
\approx0.0244\%.
\]

Por tanto:

\[
\boxed{
\frac{P_\infty(Kūn)}
{P_\infty(Qián)}
=
729
}.
\]

Esta razón **729:1** no es la misma que la razón 7.53:1 de las configuraciones quietas de una sola tirada. Miden fenómenos diferentes.

---

## 11. El control perfecto: las tres monedas

Con las tres monedas:

\[
P(6)=P(9)=\frac18,
\qquad
P(7)=P(8)=\frac38.
\]

Entonces:

\[
P(\text{yang}\rightarrow\text{yin})
=
P(\text{yin}\rightarrow\text{yang})
=
\frac14.
\]

El equilibrio es:

\[
P(\text{yin})=P(\text{yang})=\frac12.
\]

Si el sistema comienza en 50/50:

\[
E[k_t]=3
\]

para todo \(t\).

Por tanto:

\[
\boxed{
\text{Milenrama: }3\rightarrow1.5
}
\]

mientras que:

\[
\boxed{
\text{Monedas: }3\rightarrow3
}.
\]

Esto demuestra que la flecha hacia yin no pertenece simplemente al espacio abstracto de los 4096 estados: **es una propiedad específica de los pesos tradicionales de la milenrama**.

---

# Conclusión

La milenrama no genera solamente una distribución asimétrica de líneas. Define un operador estocástico completo sobre los 64 hexagramas que posee:

- deriva neta hacia yin;
- distribución estacionaria \(3/4\) yin y \(1/4\) yang;
- balance detallado;
- relajación exponencial;
- espectro binomial exacto;
- disminución de energía libre relativa;
- producción positiva de entropía total;
- y un control simétrico, las monedas, donde la flecha desaparece.

La formulación rigurosa es, por tanto:

> **El método tradicional de la milenrama induce sobre los hexagramas una dinámica estocástica de relajación hacia un equilibrio yin-dominante; matemáticamente, esa dinámica posee la estructura de un proceso termodinámico reversible con balance detallado y producción positiva de entropía total.**

---

# Frase elemental para *El Nuevo Libro*

## Versión recomendada

> **La milenrama no deja al cambio librado al azar: cuando el mundo se mueve, lo yang cae hacia lo yin tres veces más a menudo que lo yin asciende hacia lo yang. Esa pequeña asimetría basta para dar al oráculo una flecha: el cambio se relaja hacia un equilibrio más yin, como un sistema que pierde energía y se enfría.**

## Versión más breve

> **La milenrama tiene una flecha: el cambio no va por igual en ambas direcciones, sino que relaja el yang hacia el yin, como un sistema que se enfría.**

## Versión literaria

> **En la milenrama, el cambio tiene dirección: lo lleno se vacía más a menudo de lo que lo vacío se llena. El oráculo, lentamente, se enfría.**


---


---

## Apéndice de la verificación: cuánto se separan las monedas de la milenrama

El control con monedas es, a nuestro juicio, el resultado más importante de todo este análisis,
así que lo llevamos un paso más allá y medimos la distancia entre los dos oráculos sobre el
espacio completo de las 4096 lecturas. Todo en fracciones exactas.

**Coinciden exactamente en cuánto cambia el mundo.**

| | milenrama | tres monedas |
|---|---|---|
| P(una línea muta) | 1/4 | 1/4 |
| P(ninguna de las seis muta) | 729/4096 | 729/4096 |

El eje de la distancia de la Tabla 7×7 —el número de líneas mutantes— es **idéntico** en los dos
métodos: una binomial(6, 1/4) en ambos casos. Un observador que sólo contara mutaciones no
podría distinguirlos nunca.

**Discrepan en todo lo demás.**

| | milenrama | tres monedas |
|---|---|---|
| de las líneas que mutan, yang→yin | 3/4 | 1/2 |
| de las líneas que mutan, yin→yang | 1/4 | 1/2 |
| yang esperados en el transformado | 2,25 | 3 |
| la lectura pierde yang | 57,35 % | 33,06 % |
| la lectura queda igual | 29,62 % | 33,87 % |
| la lectura gana yang | 13,03 % | 33,06 % |
| razón descenso / ascenso | 4,403 | 1 |
| P(乾 → 坤) / P(坤 → 乾) | 729 | 1 |

**Distancia de variación total entre las dos distribuciones sobre las 4096 lecturas:**

    301017/1048576 = 0,28707…  ≈ 28,7 %

Casi tres décimas de toda la masa de probabilidad. No es una diferencia de redondeo: es la
distancia que separa a dos oráculos distintos, y es medible con un número suficiente de
consultas.

**La formulación breve:** las monedas conservan *cuánto* cambia el mundo y borran *hacia dónde*.
Se quedan con el temblor y tiran la flecha.


---

## Segundo apéndice de la verificación: por qué el mismo número aparece dos veces

Dos cantidades del oráculo valen lo mismo sin tener ninguna relación aparente:

- **P(ninguna de las seis líneas muta)** = (3/4)⁶ = **729/4096**
- **P∞(Kūn)**, la probabilidad de acabar en la Tierra pura tras infinitas transformaciones
  = (3/4)⁶ = **729/4096**

La primera es sobre el primer instante; la segunda, sobre el límite. En la primera, el 3/4 es
«la línea se queda quieta»; en la segunda, «la línea acaba siendo yin». Son hechos distintos.

**No está forzado.** Manteniendo la razón 3:1 y P(yin hoy) = 1/2, y variando sólo P(6):

| P(6) | P(no muta) | π_yin | ¿coinciden? |
|---|---|---|---|
| 1/20 | 4/5 | 3/4 | no |
| **1/16** | **3/4** | **3/4** | **sí** |
| 1/12 | 2/3 | 3/4 | no |
| 1/8 | 1/2 | 3/4 | no |

La condición algebraica es **P(6) = P(mutar)²**.

**La causa.** Cada línea se forma con tres cambios. En el modelo canónico:

- el primer cambio se inclina al yin con probabilidad **1/4**;
- el segundo y el tercero, con probabilidad **1/2** cada uno.

Y 1/4 = (1/2)². Es decir, **el primer cambio pesa lo mismo que los otros dos juntos**. Escrito
con p = P(primer cambio da «3») y q = P(los otros dan «3»):

    P(mutar) = p·q² + (1−p)(1−q)²     →  con q = 1/2  se reduce a 1/4, sea cual sea p
    P(6)     = (1−p)·(1−q)²           →  = (1−p)/4
    condición P(6) = P(mutar)²        →  1 − p = q²

Con p = 3/4 y q = 1/2: 1 − p = 1/4 = q². Se cumple.

**La causa de la causa: los restos en los dedos.** Al contar un montón de cuatro en cuatro, el
resto sólo puede ser 1, 2, 3 o 4. Se cuentan dos montones, y se aparta el tallo colgado más los
dos restos. La suma de los dos restos está obligada por la aritmética modular:

| cuenta | montones suman | mód 4 | restos suman | se apartan | maneras |
|---|---|---|---|---|---|
| primera | 48 | 0 | 4 | **5** | (1,3), (2,2), (3,1) → **3** |
| primera | 48 | 0 | 8 | **9** | (4,4) → **1** |
| segunda | 43 | 3 | 3 | **4** | (1,2), (2,1) → **2** |
| segunda | 43 | 3 | 7 | **8** | (3,4), (4,3) → **2** |
| tercera | 39 | 3 | 3 | **4** | (1,2), (2,1) → **2** |
| tercera | 39 | 3 | 7 | **8** | (3,4), (4,3) → **2** |

De ahí, y de nada más, salen el 3:1 de la primera cuenta y el 1:1 de las otras dos.
**Toda la flecha del oráculo nace de que el par (4,4) esté solo**, mientras que para sumar cuatro
hay tres pares distintos.

**Crédito.** La regla del cinco o nueve es de dominio común: está en todos los manuales de la
milenrama —Schoenholtz la enuncia en el apéndice A de *New Directions in the I Ching* (c. 1975),
«the quantity should be five or nine», y Huang la explica igual en *The Complete I Ching* (1998)—.
Lo nuestro es sólo la observación de que 1 − p = q², es decir que la primera cuenta pesa lo mismo
que las otras dos juntas, y que de ahí se sigue la igualdad de los dos 729/4096.

Única hipótesis: que los cuatro restos sean equiprobables, lo que ocurre si el manojo se parte al
azar y aproximadamente por la mitad. Es la hipótesis que ya está detrás de las probabilidades
clásicas 1/16, 5/16, 7/16, 3/16. Bajo ella, los dos repartos son exactos.

El origen último es la primera frase del método: 大衍之數五十，其用四十有九, «el número de la Gran
Expansión es cincuenta; de ellos se usan cuarenta y nueve». Cincuenta menos el Uno que no se
juega, contado de cuatro en cuatro.

---

## Nota sobre el guion

El guion que sigue es el original, sin modificar. No requiere librerías externas y se ejecuta
en unos segundos. Lo corrimos tal cual y reproduce todos los valores anunciados.

```
python3 i_ching_yarrow_entropy.py
```

El fichero suelto está junto a este documento como `i_ching_yarrow_entropy.py`.


```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
i_ching_yarrow_entropy.py
=========================

Reproduces the stochastic-thermodynamic analysis of the traditional
yarrow-stalk I Ching transformation.

State convention
----------------
A hexagram is a 6-bit integer 0..63.
Bit i = line i+1, counted from bottom to top.
yin = 0
yang = 1

Yarrow probabilities
--------------------
6 = yin moving   = 1/16
7 = yang still   = 5/16
8 = yin still    = 7/16
9 = yang moving  = 3/16

Conditional one-line transition matrix, rows=current (yin,yang),
columns=next (yin,yang):

    [[7/8, 1/8],
     [3/8, 5/8]]

The six-line operator is the tensor product of six copies of this
one-line transition matrix.
"""

from math import log, log2, comb
from itertools import product

# ------------------------------------------------------------
# 1. ONE-LINE OPERATOR
# ------------------------------------------------------------

P_LINE = (
    (7/8, 1/8),   # current yin -> (yin, yang)
    (3/8, 5/8),   # current yang -> (yin, yang)
)

PI_LINE = (3/4, 1/4)   # stationary distribution (yin, yang)

def check_line_stationary():
    out = []
    for j in range(2):
        out.append(sum(PI_LINE[i] * P_LINE[i][j] for i in range(2)))
    return tuple(out)

# ------------------------------------------------------------
# 2. HEXAGRAM UTILITIES
# ------------------------------------------------------------

def bit(h, i):
    """Bit i, i=0..5, bottom line first."""
    return (h >> i) & 1

def bits(h):
    return tuple(bit(h, i) for i in range(6))

def n_yang(h):
    return sum(bits(h))

def is_kun(h):
    return h == 0

def is_qian(h):
    return h == 63

# ------------------------------------------------------------
# 3. FULL 64x64 TRANSITION MATRIX
# ------------------------------------------------------------

def p_hex(i, j):
    """Probability of hexagram i -> hexagram j in one transformation."""
    p = 1.0
    for k in range(6):
        p *= P_LINE[bit(i, k)][bit(j, k)]
    return p

P64 = [[p_hex(i, j) for j in range(64)] for i in range(64)]

def check_rows():
    return max(abs(sum(row) - 1.0) for row in P64)

# ------------------------------------------------------------
# 4. STATIONARY DISTRIBUTION ON 64 HEXAGRAMS
# ------------------------------------------------------------

def pi_hex(h):
    """Product stationary measure: each yin has weight 3/4, yang 1/4."""
    k = n_yang(h)
    return (1/4)**k * (3/4)**(6-k)

PI64 = [pi_hex(h) for h in range(64)]

def check_stationary_64():
    out = [sum(PI64[i] * P64[i][j] for i in range(64)) for j in range(64)]
    return max(abs(out[j] - PI64[j]) for j in range(64))

# ------------------------------------------------------------
# 5. DETAILED BALANCE
# ------------------------------------------------------------

def detailed_balance_error():
    err = 0.0
    worst = None
    for i in range(64):
        for j in range(64):
            a = PI64[i] * P64[i][j]
            b = PI64[j] * P64[j][i]
            e = abs(a - b)
            if e > err:
                err = e
                worst = (i, j, a, b)
    return err, worst

# ------------------------------------------------------------
# 6. EXPECTED YANG COUNT UNDER ITERATION
# ------------------------------------------------------------

def evolve_dist(dist):
    return [
        sum(dist[i] * P64[i][j] for i in range(64))
        for j in range(64)
    ]

def expected_yang(dist):
    return sum(dist[h] * n_yang(h) for h in range(64))

UNIFORM64 = [1/64] * 64

def relaxation_table(steps=10):
    dist = UNIFORM64[:]
    out = []
    for t in range(steps + 1):
        out.append((t, expected_yang(dist)))
        dist = evolve_dist(dist)
    return out

def closed_form_expected_yang(t):
    return 1.5 + 1.5 * (0.5 ** t)

# ------------------------------------------------------------
# 7. SHANNON ENTROPY
# ------------------------------------------------------------

def shannon_bits(dist):
    return -sum(p * log2(p) for p in dist if p > 0)

# ------------------------------------------------------------
# 8. KL DIVERGENCE TO EQUILIBRIUM
# ------------------------------------------------------------

def kl_nats(dist, ref):
    return sum(
        p * log(p / q)
        for p, q in zip(dist, ref)
        if p > 0
    )

# ------------------------------------------------------------
# 9. THERMODYNAMIC REPRESENTATION
# ------------------------------------------------------------

# Assign yin energy 0 and yang energy DeltaE = kT ln 3.
# Work in units where kT = 1.
DELTA_E = log(3.0)

def mean_energy_kT(dist):
    """
    Mean effective energy divided by kT.
    Each yang contributes ln(3).
    """
    return expected_yang(dist) * DELTA_E

def system_entropy_over_k(dist):
    """
    Shannon/Gibbs entropy in natural-log units, i.e. S/k.
    """
    return -sum(p * log(p) for p in dist if p > 0)

def one_step_entropy_accounting(dist):
    """
    For one relaxation step:
      ΔS_system/k
      ΔS_environment/k = -ΔE_system/(kT)
      ΔS_total/k
    """
    next_dist = evolve_dist(dist)

    s0 = system_entropy_over_k(dist)
    s1 = system_entropy_over_k(next_dist)

    e0 = mean_energy_kT(dist)
    e1 = mean_energy_kT(next_dist)

    delta_s_sys = s1 - s0
    delta_s_env = -(e1 - e0)
    delta_s_tot = delta_s_sys + delta_s_env

    return next_dist, delta_s_sys, delta_s_env, delta_s_tot

# ------------------------------------------------------------
# 10. KUN / QIAN AT EQUILIBRIUM
# ------------------------------------------------------------

P_KUN_EQ = (3/4) ** 6
P_QIAN_EQ = (1/4) ** 6
RATIO_KUN_QIAN = P_KUN_EQ / P_QIAN_EQ

# ------------------------------------------------------------
# 11. EXACT SPECTRUM FROM PRODUCT STRUCTURE
# ------------------------------------------------------------

def spectrum():
    """
    One-line eigenvalues are 1 and 1/2.
    For six independent lines the eigenvalues are (1/2)^r,
    with multiplicity C(6,r).
    """
    return [
        (0.5 ** r, comb(6, r))
        for r in range(7)
    ]

# ------------------------------------------------------------
# 12. THREE-COIN CONTROL
# ------------------------------------------------------------

P_LINE_COINS = (
    (3/4, 1/4),   # yin -> yin, yang
    (1/4, 3/4),   # yang -> yin, yang
)

def p_hex_with_line_matrix(i, j, M):
    p = 1.0
    for k in range(6):
        p *= M[bit(i, k)][bit(j, k)]
    return p

P64_COINS = [
    [p_hex_with_line_matrix(i, j, P_LINE_COINS) for j in range(64)]
    for i in range(64)
]

def evolve_dist_with_matrix(dist, M64):
    return [
        sum(dist[i] * M64[i][j] for i in range(64))
        for j in range(64)
    ]

def coin_relaxation_table(steps=6):
    dist = UNIFORM64[:]
    out = []
    for t in range(steps + 1):
        out.append((t, expected_yang(dist)))
        dist = evolve_dist_with_matrix(dist, P64_COINS)
    return out

# ------------------------------------------------------------
# 13. REPORT
# ------------------------------------------------------------

if __name__ == "__main__":

    print("I CHING YARROW-STALK STOCHASTIC THERMODYNAMICS")
    print("=" * 62)

    print("\n[1] One-line stationary distribution")
    print("pi_line =", PI_LINE)
    print("pi_line @ P =", check_line_stationary())

    print("\n[2] 64x64 operator")
    print("max row-sum error =", check_rows())

    print("\n[3] Stationary distribution on 64 states")
    print("max stationarity error =", check_stationary_64())

    print("\n[4] Detailed balance")
    db_err, db_worst = detailed_balance_error()
    print("max detailed-balance error =", db_err)
    print("worst pair =", db_worst)

    print("\n[5] Relaxation of expected yang count")
    for t, ek in relaxation_table(8):
        print(
            f"t={t:2d}  E[k]={ek:.9f}  "
            f"closed_form={closed_form_expected_yang(t):.9f}"
        )

    print("\n[6] Shannon entropy")
    d0 = UNIFORM64[:]
    d1 = evolve_dist(d0)
    dinf = PI64[:]
    print("H0   =", shannon_bits(d0), "bits")
    print("H1   =", shannon_bits(d1), "bits")
    print("Hinf =", shannon_bits(dinf), "bits")

    print("\n[7] KL divergence to equilibrium")
    print("D(p0||pi) =", kl_nats(d0, PI64))
    print("D(p1||pi) =", kl_nats(d1, PI64))
    print(
        "drop        =",
        kl_nats(d0, PI64) - kl_nats(d1, PI64)
    )

    print("\n[8] Entropy accounting for first step")
    _, ds_sys, ds_env, ds_tot = one_step_entropy_accounting(d0)
    print("Delta S_system / k =", ds_sys)
    print("Delta S_env    / k =", ds_env)
    print("Delta S_total  / k =", ds_tot)

    print("\n[9] Effective energy gap")
    print("Delta E / (kT) = ln(3) =", DELTA_E)

    print("\n[10] Kun / Qian equilibrium probabilities")
    print("P_eq(Kun)  =", P_KUN_EQ)
    print("P_eq(Qian) =", P_QIAN_EQ)
    print("ratio      =", RATIO_KUN_QIAN)

    print("\n[11] Spectrum")
    for eig, mult in spectrum():
        print(f"eigenvalue={eig:.8f} multiplicity={mult}")

    print("\n[12] Three-coin control")
    for t, ek in coin_relaxation_table(6):
        print(f"t={t:2d}  E[k]={ek:.9f}")

    print("\nDONE")
```
