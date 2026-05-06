## Excercise

Demuestra y explica con tus palabras el teorema:

**1**. Escribir el algoritmo del método de bisección


El método de bisección encuentra una raíz de $f(x) = 0$ en el intervalo $[a, b]$, basándose en el **Teorema de Bolzano**: si $f$ es continua y $f(a) \cdot f(b) < 0$, entonces existe al menos una raíz $c \in (a, b)$.

---

#### Datos de entrada y salida

| Símbolo | Descripción |
|---------|-------------|
| $f(x)$ | Función continua cuya raíz se busca |
| $a,\, b$ | Extremos del intervalo inicial |
| $\text{tol}$ | Tolerancia o precisión deseada |
| $N_{\max}$ | Número máximo de iteraciones |
| $c$ | Aproximación de la raíz (salida) |

---

#### Algoritmo

**Paso 1 — Verificación del intervalo**

Comprobar que existe cambio de signo:

$$f(a) \cdot f(b) < 0$$

Si esta condición **no** se cumple, el método no puede aplicarse. Se emite un error y se termina.

---

**Paso 2 — Inicialización**

$$i \leftarrow 1$$

---

**Paso 3 — Iteración principal**

Mientras $i \leq N_{\max}$, repetir:

**(3a)** Calcular el punto medio del intervalo:

$$c \leftarrow \frac{a + b}{2}$$

**(3b)** Evaluar el criterio de convergencia:

$$f(c) = 0 \quad \text{ó} \quad \frac{b - a}{2} < \text{tol}$$

Si se cumple alguna de las dos condiciones, $c$ es la raíz aproximada. **Terminar con éxito.**

**(3c)** Incrementar el contador:

$$i \leftarrow i + 1$$

**(3d)** Seleccionar el subintervalo que contiene la raíz:

$$\text{Si } f(a) \cdot f(c) < 0 \implies b \leftarrow c \qquad (\text{la raíz está en } [a,\, c])$$

$$\text{Si } f(a) \cdot f(c) > 0 \implies a \leftarrow c \qquad (\text{la raíz está en } [c,\, b])$$

---

**Paso 4 — Sin convergencia**

Si se agotaron las $N_{\max}$ iteraciones sin satisfacer el criterio, se emite el mensaje:

> El método no convergió en $N_{\max}$ iteraciones.

---

#### Pseudocódigo

```
BISECCIÓN(f, a, b, tol, N_max)

  1. Si f(a)·f(b) ≥ 0 entonces
       Error: "No hay cambio de signo en [a, b]"
       Fin

  2. i ← 1

  3. Mientras i ≤ N_max hacer
       c ← (a + b) / 2
       Si f(c) = 0 ó (b − a)/2 < tol entonces
         Retornar c          ← raíz encontrada
       i ← i + 1
       Si f(a)·f(c) < 0 entonces
         b ← c
       Si no
         a ← c

  4. Error: "Sin convergencia tras N_max iteraciones"
```

---

#### Estimación del número de iteraciones

Para lograr una tolerancia $\text{tol}$ a partir de un intervalo inicial de longitud $L = b - a$, el número mínimo de iteraciones necesarias es:

$$n \geq \left\lceil \log_2\!\left(\frac{L}{\text{tol}}\right) \right\rceil$$

---

#### Ejemplo

Sea $f(x) = x^3 - x - 2$ en el intervalo $[1,\, 2]$, con $\text{tol} = 10^{-4}$.

- $f(1) = -2 < 0$ y $f(2) = 4 > 0$ $\implies$ hay cambio de signo.
- Longitud inicial: $L = 2 - 1 = 1$
- Iteraciones estimadas: $n \geq \lceil \log_2(1/10^{-4}) \rceil = \lceil 13.3 \rceil = 14$

| $i$ | $a$ | $b$ | $c$ | $f(c)$ |
|-----|-----|-----|-----|--------|
| 1 | 1.0000 | 2.0000 | 1.5000 | $-0.1250$ |
| 2 | 1.5000 | 2.0000 | 1.7500 | $+1.6094$ |
| 3 | 1.5000 | 1.7500 | 1.6250 | $+0.6660$ |
| $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ |
| 14 | 1.5213 | 1.5214 | 1.5214 | $\approx 0$ |

La raíz aproximada es $c \approx 1.5214$.

---

#### Propiedades del método

- **Convergencia garantizada** siempre que $f$ sea continua y $f(a) \cdot f(b) < 0$.
- **Velocidad lineal**: el error se reduce a la mitad en cada iteración.
- **Error en la iteración $n$**: $|e_n| \leq \dfrac{b - a}{2^n}$
- Es más lento que Newton-Raphson, pero más robusto: nunca diverge si las condiciones iniciales se cumplen.

**2**. Describe con tus propias palabras cada paso del algoritmo

#### ¿Qué problema resuelve?

Queremos encontrar el valor de $x$ donde una función $f(x)$ vale cero, es decir, su **raíz**. El método de bisección lo hace de forma muy intuitiva: si sabemos que la raíz está en algún lugar entre dos valores $a$ y $b$, vamos achicando ese intervalo a la mitad una y otra vez hasta que encontramos la raíz con la precisión que necesitamos.

---

#### Condición de partida

Para que el método funcione, la función debe cruzar el eje $x$ dentro del intervalo $[a, b]$. Eso ocurre cuando $f(a)$ y $f(b)$ tienen **signos opuestos**: uno es positivo y el otro negativo. Si esto no se cumple, el método no puede aplicarse.

$$f(a) \cdot f(b) < 0$$

---

#### ¿Cómo funciona paso a paso?

**1. Elegimos el punto medio del intervalo.**

Calculamos el valor exactamente a la mitad entre $a$ y $b$:

$$c = \frac{a + b}{2}$$

**2. Evaluamos la función en ese punto medio.**

Hay tres posibilidades:

- Si $f(c) = 0$, encontramos la raíz exacta. Terminamos.
- Si $f(c)$ tiene el **mismo signo que $f(a)$**, la raíz no está en $[a, c]$, sino en $[c, b]$. Entonces reemplazamos $a \leftarrow c$ y continuamos.
- Si $f(c)$ tiene el **mismo signo que $f(b)$**, la raíz no está en $[c, b]$, sino en $[a, c]$. Entonces reemplazamos $b \leftarrow c$ y continuamos.

**3. Repetimos.**

Volvemos al paso 1 con el nuevo intervalo, que ahora es la mitad del anterior. Cada repetición nos da una aproximación más precisa.

**4. Paramos cuando la precisión es suficiente.**

Detenemos el proceso cuando el intervalo sea tan pequeño que la mitad de su longitud ya sea menor que la tolerancia deseada:

$$\frac{b - a}{2} < \text{tol}$$

En ese momento, $c$ es nuestra mejor aproximación de la raíz.

---

#### ¿Por qué funciona?

En cada iteración el intervalo se divide a la mitad. Por eso el error máximo después de $n$ iteraciones es:

$$|e_n| \leq \frac{b - a}{2^n}$$

Si el intervalo inicial mide $L = b - a$, el número de iteraciones necesarias para alcanzar una tolerancia $\text{tol}$ es aproximadamente:

$$n \geq \left\lceil \log_2\!\left(\frac{L}{\text{tol}}\right) \right\rceil$$

---

#### Ventajas y limitaciones

**Ventajas:**
- Siempre converge si la condición de partida se cumple.
- Es muy sencillo de programar y entender.
- No necesita calcular derivadas.

**Limitaciones:**
- Es lento comparado con otros métodos como Newton-Raphson.
- Solo encuentra una raíz a la vez, aunque haya varias en el intervalo.
- Requiere conocer de antemano un intervalo que contenga la raíz.

**3**. Demuestra

Suponga que $f \in C[a,b]$ y $f(a)\cdot f(b) < 0$. El método de bisección genera una sucesión $\{p_n\}_{n=1}^{\infty}$ que se aproxima a un cero $p$ de $f$ con

$$
|p_n - p| \leq \frac{b - a}{2^n}, \quad \text{cuando } n \geq 1.
$$

---

#### Prueba

Definamos los extremos del intervalo en cada iteración como $a_n$ y $b_n$, con $n \geq 1$.

**Paso base.** Al inicio tenemos el intervalo original:

$$
a_1 = a, \qquad b_1 = b.
$$

El primer punto medio es:

$$
p_1 = \frac{a_1 + b_1}{2}.
$$

La longitud del intervalo inicial es $b - a$, por lo que:

$$
|p_1 - p| \leq \frac{b_1 - a_1}{2} = \frac{b - a}{2}.
$$

Esto verifica el caso $n = 1$.

---

**Paso inductivo.** Supongamos que en la iteración $n$ se cumple:

$$
|p_n - p| \leq \frac{b - a}{2^n}.
$$

En cada iteración el método selecciona el subintervalo $[a_{n+1}, b_{n+1}]$ de longitud:

$$
b_{n+1} - a_{n+1} = \frac{b_n - a_n}{2}.
$$

Aplicando esto recursivamente desde el inicio:

$$
b_n - a_n = \frac{b - a}{2^{n-1}}.
$$

El punto medio de la iteración $n+1$ es:

$$
p_{n+1} = \frac{a_{n+1} + b_{n+1}}{2},
$$

y la raíz $p$ pertenece al intervalo $[a_{n+1}, b_{n+1}]$, por lo que:

$$
|p_{n+1} - p| \leq \frac{b_{n+1} - a_{n+1}}{2} = \frac{b - a}{2^{n+1}}.
$$

Esto completa el paso inductivo.

---

**Conclusión.** Por inducción matemática, para todo $n \geq 1$:

$$
|p_n - p| \leq \frac{b - a}{2^n}.
$$

Como $\dfrac{b-a}{2^n} \to 0$ cuando $n \to \infty$, se concluye que:

$$
\lim_{n \to \infty} p_n = p. \qquad \blacksquare
$$

**4.** Escribe la demostración con tus propias palabras

La demostración dice esencialmente una cosa muy simple: **cada vez que biseccionamos, el error se reduce a la mitad**.

---

Sabemos que la raíz $p$ existe porque $f$ es continua y cambia de signo en $[a, b]$. Eso nos garantiza que hay algo que buscar.

En la **primera iteración** tomamos el punto medio del intervalo. La raíz $p$ puede estar en cualquier lugar dentro de $[a, b]$, así que lo peor que nos puede pasar es que el punto medio esté lo más lejos posible de $p$, que es exactamente la mitad del intervalo. Por eso el error en el primer paso no puede ser mayor que $\dfrac{b-a}{2}$.

En la **segunda iteración** descartamos la mitad del intervalo que no contiene la raíz, y nos quedamos con un intervalo que mide la mitad del anterior. Aplicamos el mismo argumento: el error ahora no puede superar la mitad de ese nuevo intervalo, que es $\dfrac{b-a}{4}$.

Y así sucesivamente. En cada paso el intervalo se parte a la mitad, y con él, la peor distancia posible entre nuestro punto medio y la raíz verdadera. Después de $n$ iteraciones, el error está acotado por:

$$|p_n - p| \leq \frac{b-a}{2^n}$$

La clave del argumento formal es la **inducción**: se verifica que esto vale para $n=1$, y luego se demuestra que si vale en el paso $n$, también vale en el paso $n+1$. Como el razonamiento se repite indefinidamente, vale para siempre.

Finalmente, como $2^n$ crece sin límite, la fracción $\dfrac{b-a}{2^n}$ se hace cada vez más pequeña hasta tender a cero. Eso significa que los puntos medios $p_n$ se acercan inevitablemente a la raíz $p$, sin importar cuánto tardemos: la convergencia está **garantizada**. $\blacksquare$

**5**. Escribe un ejemplo específico. Por ejemplo,

Determine el número de iteraciones necesarias para resolver

$$
f(x) = x^2 + 4x^2 - 10 = 0 
$$

con precisión de $10^{-3}$ mediante $a_1 = 1$ y $b_1 = 2$

Usando otra función, otra precisión y valores $a_1$, $\; b_1$

#### Ejemplo

Determine el número de iteraciones necesarias para resolver

$$
f(x) = x^3 - 2x - 5 = 0
$$

con precisión de $10^{-4}$ mediante $a_1 = 2$ y $b_1 = 3$.

---

#### Paso 1 — Verificar la condición de cambio de signo

Evaluamos $f$ en los extremos del intervalo inicial:

$$
f(2) = (2)^3 - 2(2) - 5 = 8 - 4 - 5 = -1 < 0
$$

$$
f(3) = (3)^3 - 2(3) - 5 = 27 - 6 - 5 = 16 > 0
$$

Como $f(2) \cdot f(3) < 0$, existe al menos una raíz en $(2,\, 3)$ y el método puede aplicarse.

---

#### Paso 2 — Estimar el número de iteraciones

Usando la cota de error del método de bisección:

$$
|p_n - p| \leq \frac{b_1 - a_1}{2^n} < 10^{-4}
$$

Sustituimos $b_1 - a_1 = 3 - 2 = 1$:

$$
\frac{1}{2^n} < 10^{-4}
$$

Aplicamos logaritmo en ambos lados:

$$
2^n > 10^{4}
$$

$$
n > \log_2(10^{4}) = 4\log_2(10) \approx 4 \times 3.3219 = 13.2877
$$

Por lo tanto se necesitan al menos $n = 14$ iteraciones.

---

#### Paso 3 — Primeras iteraciones

| $n$ | $a_n$ | $b_n$ | $p_n$ | $f(p_n)$ |
|-----|--------|--------|--------|-----------|
| 1 | 2.0000 | 3.0000 | 2.5000 | $+5.6250$ |
| 2 | 2.0000 | 2.5000 | 2.2500 | $+1.8906$ |
| 3 | 2.0000 | 2.2500 | 2.1250 | $+0.3457$ |
| 4 | 2.0000 | 2.1250 | 2.0625 | $-0.3513$ |
| 5 | 2.0625 | 2.1250 | 2.0938 | $-0.0089$ |
| 6 | 2.0938 | 2.1250 | 2.1094 | $+0.1668$ |
| 7 | 2.0938 | 2.1094 | 2.1016 | $+0.0787$ |
| $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ | $\vdots$ |
| 14 | 2.0945 | 2.0946 | 2.0946 | $\approx 0$ |

---

#### Conclusión

Después de $n = 14$ iteraciones, la aproximación de la raíz es:

$$
p_{14} \approx 2.0946
$$

con un error garantizado menor que:

$$
|p_{14} - p| \leq \frac{1}{2^{14}} = \frac{1}{16384} \approx 6.1 \times 10^{-5} < 10^{-4} \qquad \blacksquare
$$
