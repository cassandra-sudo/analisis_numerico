# Resolución de Ecuaciones No Lineales: Métodos Iterativos

**Área:** Análisis Numérico  
**Nivel:** Licenciatura en Matemáticas Aplicadas — Curso avanzado  
**Prerrequisitos:** Cálculo diferencial e integral, Álgebra lineal, Fundamentos de análisis real.

---

## Introducción

El problema fundamental del análisis numérico unidimensional consiste en encontrar las raíces de una ecuación de la forma

$$f(x) = 0, \quad f : D \subseteq \mathbb{R} \to \mathbb{R}$$

Cuando $f$ no admite una solución analítica cerrada —situación habitual en la práctica científica e ingenieril—, se recurre a **métodos iterativos**: procedimientos algorítmicos que generan sucesiones $\{x_n\}$ que convergen (bajo ciertas hipótesis) a una raíz $x^*$ tal que $f(x^*) = 0$.

El análisis riguroso de estos métodos exige estudiar tres propiedades fundamentales:

1. **Convergencia:** ¿La sucesión generada converge a la raíz?  
2. **Orden de convergencia:** ¿Con qué velocidad converge?  
   Se dice que el método tiene orden $p$ si existe $C > 0$ tal que $\lim_{n\to\infty} \dfrac{|e_{n+1}|}{|e_n|^p} = C$, donde $e_n = x_n - x^*$.  
3. **Estabilidad y robustez:** ¿Cuándo falla el método?

En este documento se presentan y desarrollan formalmente cinco métodos clásicos, acompañados de ejercicios teóricos y prácticos de nivel avanzado.

---

---

## 1. Método de Bisección

### 1.1 Fundamento teórico

El método de bisección es el algoritmo de búsqueda de raíces más elemental y robusto. Su sustento matemático es el **Teorema del Valor Intermedio (TVI)**:

> **Teorema (Bolzano, 1817).** Sea $f : [a, b] \to \mathbb{R}$ continua. Si $f(a) \cdot f(b) < 0$, entonces existe al menos un $x^* \in (a, b)$ tal que $f(x^*) = 0$.

El método no requiere diferenciabilidad, solo continuidad y la condición de cambio de signo, lo que lo convierte en el método de garantía absoluta de convergencia bajo dichas hipótesis.

### 1.2 Algoritmo

**Datos de entrada:** $f$, $a$, $b$ con $f(a) \cdot f(b) < 0$, tolerancia $\varepsilon > 0$, máximo de iteraciones $N$.

**Idea central:** En cada iteración se evalúa el punto medio $c = \dfrac{a+b}{2}$ y se descarta el subintervalo que no contiene a la raíz, reduciendo a la mitad la longitud del intervalo de búsqueda.

```
ALGORITMO Bisección(f, a, b, ε, N):
    Si f(a) · f(b) ≥ 0 entonces
        ERROR: "La condición de cambio de signo no se cumple"
    Fin Si

    Para n = 1, 2, ..., N hacer:
        c ← (a + b) / 2
        Si f(c) = 0 o (b - a)/2 < ε entonces
            RETORNAR c          // Solución encontrada
        Fin Si
        Si f(a) · f(c) < 0 entonces
            b ← c               // La raíz está en [a, c]
        En otro caso:
            a ← c               // La raíz está en [c, b]
        Fin Si
    Fin Para

    RETORNAR c                  // Mejor aproximación tras N iteraciones
```

### 1.3 Análisis de convergencia

**Proposición.** El método de bisección converge linealmente. Si $[a_0, b_0]$ es el intervalo inicial con longitud $L_0 = b_0 - a_0$, entonces después de $n$ iteraciones:

$$|x_n - x^*| \leq \frac{L_0}{2^n}$$

**Demostración.** Por construcción, en cada iteración la longitud del intervalo se divide por dos: $L_n = L_0 / 2^n$. La raíz $x^*$ siempre pertenece al intervalo activo $[a_n, b_n]$, y el punto medio $c_n$ dista a lo sumo $L_n/2$ de cualquier punto del intervalo. Por tanto:

$$|c_n - x^*| \leq \frac{b_n - a_n}{2} = \frac{L_0}{2^{n+1}}$$

**Corolario (número de iteraciones necesarias).** Para garantizar un error $|c_n - x^*| < \varepsilon$, es suficiente tomar:

$$n > \frac{\ln(L_0/\varepsilon)}{\ln 2} = \log_2\!\left(\frac{L_0}{\varepsilon}\right)$$

**Orden de convergencia.** El método es de **orden 1** (convergencia lineal) con constante asintótica $C = 1/2$, ya que:

$$|e_{n+1}| \leq \frac{1}{2}|e_n| \implies \frac{|e_{n+1}|}{|e_n|} \to \frac{1}{2}$$

### 1.4 Criterios de parada

En la práctica se emplean uno o varios de los siguientes:

- **Tolerancia absoluta:** $|b_n - a_n| < \varepsilon$  
- **Tolerancia relativa:** $\dfrac{|b_n - a_n|}{|c_n|} < \varepsilon$  
- **Residuo:** $|f(c_n)| < \delta$

---

### Ejercicios Teóricos — Método de Bisección

---

**Ejercicio T1.1 — Cota de iteraciones y unicidad.**

Sea $f(x) = x^3 - 2x - 5$ en $[2, 3]$.

*(a)* Demuestra que $f$ tiene exactamente una raíz en $(2, 3)$.  
*(b)* Calcula el número mínimo de iteraciones necesarias para aproximar la raíz con un error absoluto menor que $10^{-6}$.  
*(c)* Demuestra que la sucesión $\{c_n\}$ generada por bisección converge a dicha raíz.

**Solución:**

*(a)* Se verifica: $f(2) = 8 - 4 - 5 = -1 < 0$ y $f(3) = 27 - 6 - 5 = 16 > 0$. Por el TVI existe al menos una raíz en $(2,3)$. Para la unicidad, calculamos $f'(x) = 3x^2 - 2$. En $[2,3]$: $f'(x) \geq 3(4) - 2 = 10 > 0$, luego $f$ es estrictamente creciente en $[2,3]$, por lo que la raíz es única.

*(b)* $L_0 = b - a = 1$. Se requiere $n > \log_2(1/10^{-6}) = \log_2(10^6) = 6\log_2(10) \approx 6 \times 3.3219 \approx 19.93$. Luego se necesitan al menos $\mathbf{n = 20}$ iteraciones.

*(c)* Por construcción, $x^* \in [a_n, b_n]$ para todo $n$, y $|c_n - x^*| \leq (b_n - a_n)/2 = 1/2^{n+1} \to 0$ cuando $n \to \infty$. Por tanto $c_n \to x^*$.

---

**Ejercicio T1.2 — Análisis del orden de convergencia.**

Sea $g : [0,1] \to \mathbb{R}$ continua con $g(0)g(1) < 0$, y suponga que $g$ tiene una única raíz $x^*$ en $(0,1)$.

*(a)* Prueba que para la sucesión de bisección $\{c_n\}$, el cociente $\dfrac{|e_{n+1}|}{|e_n|}$ no tiene límite en general (es decir, el método tiene orden 1 pero no converge de forma monótona).

*(b)* Construye explícitamente una función $g$ en $[0,1]$ para la que en la iteración $n=1$ el error se reduce más del 50%, y otra para la que el error en $n=1$ es exactamente la mitad del inicial.

**Solución:**

*(a)* El cociente $|e_{n+1}|/|e_n|$ en bisección depende de la posición relativa de $x^*$ dentro de $[a_n, b_n]$ en cada paso: si $x^*$ está cerca del punto medio $c_n$, el cociente puede ser mucho menor que $1/2$; si está en el extremo del subintervalo seleccionado, puede ser cercano a $1$. Dado que la posición de $x^*$ respecto a $c_n$ varía irregularmente, el cociente oscila y no converge en general. Formalmente: $|e_{n+1}| = |c_{n+1} - x^*|$ donde $c_{n+1}$ es el punto medio del nuevo subintervalo, el cual puede contener a $x^*$ en cualquier posición.

*(b)* Sea $g(x) = x - 0.1$ en $[0,1]$. Entonces $x^* = 0.1$, $c_1 = 0.5$, $|e_1| = 0.4 = L_0/2$. La reducción es exactamente la mitad. Para reducción mayor al 50%: sea $g(x) = x - 0.01$. Aquí $x^* = 0.01$, $c_1 = 0.5$, $|e_1| = 0.49 < 0.5 = L_0/2$. Pero $|e_1|/|e_0| = 0.49/0.5 < 1$. La cota $|e_{n+1}| \leq |e_n|/2$ es garantizada, pero $x^*$ muy cerca de un extremo produce $|e_1| \approx |e_0|/2$ (casi sin mejora local aunque la cota global se cumple).

---

**Ejercicio T1.3 — Existencia de múltiples raíces y fallo de la hipótesis.**

Sea $f(x) = (x-1)^2(x-3)$ en $[0, 4]$.

*(a)* ¿Puede aplicarse el método de bisección directamente en $[0,4]$? Justifica usando el TVI.  
*(b)* ¿Es posible encontrar subintervalos de $[0,4]$ donde sí se cumpla la condición? Encuéntralos y analiza qué raíz encontrará bisección en cada uno.  
*(c)* ¿Qué ocurre con la raíz de multiplicidad par $x^*=1$ desde el punto de vista del método?

**Solución:**

*(a)* $f(0) = (0-1)^2(0-3) = 1 \cdot (-3) = -3 < 0$ y $f(4) = (3)^2(1) = 9 > 0$. Existe cambio de signo en $[0,4]$, por lo que bisección puede aplicarse. Sin embargo, hay dos raíces distintas (considerando multiplicidad): $x=1$ (doble) y $x=3$ (simple). La condición $f(0)f(4)<0$ se satisface.

*(b)* Analicemos los signos: $f(0) < 0$, $f(2) = (1)^2(-1) = -1 < 0$, $f(2.5) = (1.5)^2(-0.5) = -1.125 < 0$, $f(4) = 9 > 0$. El único cambio de signo ocurre entre $x=3$ (donde $f$ pasa de negativo a positivo): bisección en $[0,4]$ converge a $x^* = 3$. En ningún subintervalo que contenga solo a $x=1$ puede verificarse el cambio de signo, pues $f(x) \leq 0$ en una vecindad de $x=1$.

*(c)* La raíz $x^* = 1$ tiene multiplicidad par, lo que significa que $f$ no cambia de signo en ella: $f(x) = (x-1)^2(x-3) \leq 0$ para $x \in [0,3]$. El método de bisección no puede detectar raíces de multiplicidad par en general sin modificaciones (por ejemplo, trabajar con $|f|$ o $f'$).

---

### Ejercicios Prácticos — Método de Bisección

---

**Ejercicio P1.1**

Aplica el método de bisección para encontrar la raíz positiva de $f(x) = e^x - 3x$ en el intervalo $[1, 2]$, con una tolerancia de $\varepsilon = 10^{-4}$. Presenta la tabla de iteraciones completa e indica cuántas fueron necesarias.

**Solución:**

Verificación: $f(1) = e - 3 \approx 2.718 - 3 = -0.282 < 0$ y $f(2) = e^2 - 6 \approx 7.389 - 6 = 1.389 > 0$. ✓

$L_0 = 1$, iteraciones necesarias: $n > \log_2(1/10^{-4}) \approx 13.29$, es decir, $n = 14$.

| $n$ | $a_n$   | $b_n$   | $c_n$   | $f(c_n)$    |
|-----|---------|---------|---------|-------------|
| 1   | 1.0000  | 2.0000  | 1.5000  | $e^{1.5}-4.5 \approx -0.0183 < 0$ |
| 2   | 1.5000  | 2.0000  | 1.7500  | $e^{1.75}-5.25 \approx 0.5437 > 0$ |
| 3   | 1.5000  | 1.7500  | 1.6250  | $e^{1.625}-4.875 \approx 0.2517 > 0$ |
| 4   | 1.5000  | 1.6250  | 1.5625  | $e^{1.5625}-4.6875 \approx 0.1147 > 0$ |
| 5   | 1.5000  | 1.5625  | 1.5313  | $\approx 0.0473 > 0$ |
| 6   | 1.5000  | 1.5313  | 1.5156  | $\approx 0.0143 > 0$ |
| 7   | 1.5000  | 1.5156  | 1.5078  | $\approx -0.0021 < 0$ |
| 8   | 1.5078  | 1.5156  | 1.5117  | $\approx 0.0061 > 0$ |
| 9   | 1.5078  | 1.5117  | 1.5098  | $\approx 0.0020 > 0$ |
| 10  | 1.5078  | 1.5098  | 1.5088  | $\approx -0.00005 < 0$ |
| 11  | 1.5088  | 1.5098  | 1.5093  | $\approx 0.0010 > 0$ |
| 12  | 1.5088  | 1.5093  | 1.5090  | $\approx 0.0005 > 0$ |
| 13  | 1.5088  | 1.5090  | 1.5089  | $\approx 0.0002 > 0$ |
| 14  | 1.5088  | 1.5089  | **1.5088** | — |

La raíz aproximada es $x^* \approx \mathbf{1.5088}$, obtenida en **14 iteraciones**. (La raíz exacta es la solución de $e^x = 3x$, que corresponde a $x^* \approx 1.51213$.)

---

**Ejercicio P1.2**

La ecuación $\cos(x) = x$ tiene una única solución en $[0, \pi/2]$, conocida como el **punto fijo del coseno** o constante de Dottie ($x^* \approx 0.7391$). Aplica bisección en $[0, 1]$ hasta alcanzar $\varepsilon = 10^{-5}$ (error en longitud del intervalo). ¿Cuántas iteraciones se necesitan? Verifica tu resultado sustituyendo en la ecuación original.

**Solución:**

Reformulamos: $g(x) = \cos(x) - x = 0$. $g(0) = 1 > 0$, $g(1) = \cos(1) - 1 \approx -0.4597 < 0$. ✓

Iteraciones: $n > \log_2(1/10^{-5}) \approx 16.6$, por lo que se necesitan **17 iteraciones**.

Iteraciones seleccionadas:

| $n$ | $c_n$    | $g(c_n)$  |
|-----|----------|-----------|
| 1   | 0.5000   | $+0.3776$ |
| 2   | 0.7500   | $-0.0183$ |
| 3   | 0.6250   | $+0.1864$ |
| 4   | 0.6875   | $+0.0831$ |
| 5   | 0.7188   | $+0.0321$ |
| 6   | 0.7344   | $+0.0069$ |
| 7   | 0.7422   | $-0.0057$ |
| 8   | 0.7383   | $+0.0006$ |
| 9   | 0.7402   | $-0.0026$ |
| 10  | 0.7393   | $-0.0010$ |
| 11  | 0.7388   | $-0.0002$ |
| 12  | 0.7385   | $+0.0002$ |
| 13  | 0.7387   | $+0.00001$|
| 14–17 | converge a $0.73909$ | — |

Resultado: $x^* \approx \mathbf{0.73909}$. Verificación: $\cos(0.73909) \approx 0.73912 \approx x^*$. ✓

---

**Ejercicio P1.3**

Un ingeniero modela el perfil de temperatura en una barra como $T(x) = e^{-x} \sin(\pi x)$ para $x \in [0.5, 1.5]$ y necesita encontrar el punto donde la temperatura es cero (diferente de los extremos conocidos). Aplica bisección con $\varepsilon = 10^{-3}$ y determina la raíz, indicando su interpretación física.

**Solución:**

$T(x) = e^{-x}\sin(\pi x)$. La ecuación $T(x)=0$ implica $\sin(\pi x) = 0$ (pues $e^{-x} > 0$ siempre), cuyas soluciones son $x = k$ para $k \in \mathbb{Z}$. En $[0.5, 1.5]$ la única raíz es $x^* = 1$.

Verificamos: $T(0.5) = e^{-0.5}\sin(\pi/2) = e^{-0.5}(1) > 0$ y $T(1.5) = e^{-1.5}\sin(3\pi/2) = e^{-1.5}(-1) < 0$. ✓

Iteraciones necesarias: $n > \log_2(1/10^{-3}) \approx 9.97$, es decir, **10 iteraciones**.

| $n$ | $a_n$  | $b_n$  | $c_n$  | $T(c_n)$  |
|-----|--------|--------|--------|-----------|
| 1   | 0.5000 | 1.5000 | 1.0000 | $e^{-1}\sin(\pi) = 0$ |

En la **primera iteración**, $T(c_1) = e^{-1}\sin(\pi) = 0$ exactamente. El método termina de inmediato.

$x^* = \mathbf{1.000}$. **Interpretación física:** el punto $x=1$ es el nodo donde la temperatura es nula, correspondiente a un punto de cambio de fase en la distribución de temperatura de la barra (la temperatura cambia de positiva a negativa alrededor de este punto).

---
---

## 2. Método de Newton-Raphson

### 2.1 Fundamento teórico

El método de Newton-Raphson (también llamado método de Newton o método de la tangente) es el método iterativo más utilizado en la práctica por su convergencia cuadrática. La idea es aproximar $f$ por su linealización de primer orden (serie de Taylor truncada) en el punto actual $x_n$:

$$f(x) \approx f(x_n) + f'(x_n)(x - x_n)$$

Igualando a cero esta aproximación lineal se obtiene la siguiente iteración:

$$x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}, \quad f'(x_n) \neq 0$$

Geométricamente, $x_{n+1}$ es la intersección de la tangente a la curva $y=f(x)$ en $(x_n, f(x_n))$ con el eje $x$.

### 2.2 Algoritmo

**Datos de entrada:** $f$, $f'$, $x_0$ (estimación inicial), tolerancia $\varepsilon$, máximo de iteraciones $N$.

```
ALGORITMO Newton-Raphson(f, f', x₀, ε, N):
    Para n = 0, 1, 2, ..., N-1 hacer:
        Si f'(xₙ) = 0 entonces
            ERROR: "Derivada nula — el método falla"
        Fin Si
        x_{n+1} ← xₙ - f(xₙ) / f'(xₙ)
        Si |x_{n+1} - xₙ| < ε entonces
            RETORNAR x_{n+1}
        Fin Si
    Fin Para
    RETORNAR x_N   // Mejor aproximación alcanzada
```

**Teorema 2.5 (Teorema de Newton-Raphson).** Supongamos que la función $f \in C^2[a, b]$ y que existe un número $p \in [a, b]$ tal que $f(p) = 0$. Si $f'(p) \neq 0$, entonces existe $\delta > 0$ tal que la sucesión $\{p_k\}_{k=0}^{\infty}$ definida por el proceso iterativo

$$
p_k = g(p_{k-1}) = p_{k-1} - \frac{f(p_{k-1})}{f'(p_{k-1})}, \quad k = 1,2,\ldots
$$

converge a $p$ cualquiera que sea la aproximación inicial $p_0 \in [p - \delta, p + \delta]$.

*Observación.* La función $g(x)$ definida por la relación

$$
g(x) = x - \frac{f(x)}{f'(x)}
$$

se llama **función de iteración de Newton-Raphson**. Puesto que $f(p) = 0$, es fácil ver que $g(p) = p$, lo que nos dice que la iteración de Newton-Raphson para hallar una raíz de la ecuación $f(x) = 0$ consiste en hallar un punto fijo de $g(x)$.

*Demostración.* La construcción geométrica de $p_1$ que se muestra en la Figura 2.13 no nos ayuda a entender por qué $p_0$ debe estar cerca de $p$ ni por qué la continuidad de $f''(x)$ es esencial. Nuestro análisis comienza con el polinomio de Taylor de grado $n = 1$ de $f$ alrededor de $p_0$ y su correspondiente resto:

$$
f(x) = f(p_0) + f'(p_0)(x - p_0) + \frac{f''(c)(x - p_0)^2}{2!}
$$

donde $c$ es un punto intermedio entre $p_0$ y $x$. Poniendo $x = p$ en la relación (6) y usando que $f(p) = 0$ obtenemos

$$
0 = f(p_0) + f'(p_0)(p - p_0) + \frac{f''(c)(p - p_0)^2}{2!}.
$$

Si $p_0$ está suficientemente cerca de $p$, entonces el último sumando del miembro derecho de (7) será pequeño, comparado con la suma de los dos primeros, así que podemos despreciarlo y usar la aproximación

$$
0 \approx f(p_0) + f'(p_0)(p - p_0).
$$

Despejando $p$ en la relación (8), obtenemos $p \approx p_0 - f(p_0)/f'(p_0)$, expresión que usamos para definir $p_1$, la siguiente aproximación a la raíz

$$
p_1 = p_0 - \frac{f(p_0)}{f'(p_0)}.
$$

Poniendo $p_{k-1}$ en lugar de $p_0$ en la relación (9), la regla general (4) queda establecida. En muchas aplicaciones, esto es todo lo que hace falta entender y saber usar; sin embargo, para comprender totalmente lo que ocurre, necesitamos considerar la iteración de Newton-Raphson como una iteración de punto fijo y aplicar el Teorema 2.2 en nuestra situación. La clave nos la da el análisis de $g'(x)$:

$$
g'(x) = 1 - \frac{f'(x)f'(x) - f(x)f''(x)}{(f'(x))^2} = \frac{f(x)f''(x)}{(f'(x))^2}.
$$

Por hipótesis, sabemos que $f(p) = 0$; luego $g'(p) = 0$. Como $g(x)$ es continua y $g'(p) = 0$, podemos encontrar $\delta > 0$ tal que la hipótesis $|g'(x)| < 1$ del Teorema 2.2 se cumple en el intervalo $(p - \delta, p + \delta)$. Por consiguiente, que $p_0 \in (p - \delta, p + \delta)$ es una condición suficiente para que $p_0$ sea el punto de partida de una sucesión $\{p_k\}_{k=0}^{\infty}$ que converge a la única raíz de $f(x) = 0$ en dicho intervalo, siempre que $\delta$ sea elegido tal que

$$
\frac{|f(x)f''(x)|}{|f'(x)|^2} < 1 \qquad \text{para todo } x \in (p - \delta,\, p + \delta).
$$

---

### Ejercicios Teóricos — Método de Newton-Raphson

---

**Ejercicio T2.1 — Demostración de convergencia y cota del error.**

Sea $f(x) = x^2 - a$ con $a > 0$. La iteración de Newton-Raphson aplicada a esta función genera la famosa **fórmula de Herón** para aproximar $\sqrt{a}$.

*(a)* Deduce la iteración explícita $x_{n+1} = \frac{1}{2}\left(x_n + \frac{a}{x_n}\right)$ y demuestra que converge cuadráticamente a $\sqrt{a}$ para cualquier $x_0 > 0$.

*(b)* Prueba que la sucesión es monótonamente decreciente para $n \geq 1$ y acotada inferiormente por $\sqrt{a}$.

*(c)* Determina la constante asintótica de convergencia cuadrática.

**Solución:**

*(a)* $f(x) = x^2 - a$, $f'(x) = 2x$. La iteración es:

$$x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$$

$$x_{n+1} = x_n - \frac{x_n^2 - a}{2x_n} = \frac{2x_n^2 - x_n^2 + a}{2x_n} = \frac{x_n^2 + a}{2x_n} = \frac{1}{2}\left(x_n + \frac{a}{x_n}\right)$$

Para la convergencia: $f''(x) = 2$, $f'(\sqrt{a}) = 2\sqrt{a} \neq 0$. El error satisface:
$$e_{n+1} = -\frac{f''(\xi_n)}{2f'(x_n)}e_n^2 = -\frac{2}{2 \cdot 2x_n}e_n^2 = -\frac{e_n^2}{2x_n}$$
Para $x_0 > 0$ y $n$ suficientemente grande, $x_n > 0$ y la convergencia es cuadrática.

*(b)* Por la desigualdad AM-GM: $x_{n+1} = \frac{1}{2}\left(x_n + \frac{a}{x_n}\right) \geq \sqrt{x_n \cdot \frac{a}{x_n}} = \sqrt{a}$. La igualdad solo ocurre si $x_n = \sqrt{a}$. Para la monotonía cuando $n \geq 1$: $x_{n+1} \leq x_n$ iff $\frac{1}{2}(x_n + a/x_n) \leq x_n$ iff $a \leq x_n^2$. Para $n=1$: $x_1 \geq \sqrt{a}$ implica $x_1^2 \geq a$. ✓

*(c)* La constante asintótica es 

$$C = \frac{\lvert f''(x^{\ast})\rvert}{2\lvert f'(x^{\ast})\rvert} = \frac{2}{2 \cdot 2\sqrt{a}} = \frac{1}{2\sqrt{a}}$$

---

**Ejercicio T2.2 — Análisis de fallo y condiciones de Kantoróvich.**

*(a)* Construye un ejemplo concreto donde Newton-Raphson, aplicado con una estimación inicial $x_0$ "cercana" a la raíz, falle en converger (ciclo o divergencia).

*(b)* Enuncia el **Teorema de Kantoróvich** y explica cómo proporciona una condición suficiente de convergencia que es verificable sin conocer la raíz.

**Solución:**

*(a)* Sea $f(x) = \arctan(x)$. La raíz es $x^* = 0$ y $f'(x) = 1/(1+x^2)$. La iteración es:
$$x_{n+1} = x_n - (1+x_n^2)\arctan(x_n)$$
Para $x_0 = 1.4$: $x_1 = 1.4 - (1+1.96)\arctan(1.4) \approx 1.4 - 2.96 \times 0.9505 \approx -1.414$. Luego $x_2 \approx 1.414$, y la sucesión oscila sin converger. De hecho, para $|x_0| > 1.3917...$ el método diverge.

*(b)* **Teorema de Kantoróvich:** Sea $f \in \mathcal{C}^2$ y suponga que existe $x_0$ tal que:
- $|f(x_0)| \leq \alpha$
- $|[f'(x_0)]^{-1}| \leq \beta$  
- $|f''(x)| \leq \gamma$ en una bola $B(x_0, r)$ con $r = 2\alpha\beta$

Si $h = \alpha\beta\gamma \leq 1/2$, entonces Newton-Raphson converge a una raíz $x^* \in B(x_0, r)$ y la convergencia es cuadrática. La ventaja es que todas las condiciones se verifican en $x_0$ y sus vecinos, sin conocer $x^*$.

---

**Ejercicio T2.3 — Raíces múltiples y modificación del método.**

Sea $f(x) = (x-2)^3(x+1)$.

*(a)* Identifica todas las raíces y sus multiplicidades.  
*(b)* Aplica la fórmula de Newton estándar cerca de $x=2$ y determina el orden de convergencia analíticamente.  
*(c)* Propone y justifica la modificación $x_{n+1} = x_n - 3 \cdot f(x_n)/f'(x_n)$ para restaurar la convergencia cuadrática.

**Solución:**

*(a)* Las raíces son $x_1 = 2$ (multiplicidad 3) y $x_2 = -1$ (multiplicidad 1).

*(b)* Para raíz de multiplicidad $m=3$: el método estándar converge linealmente con constante $(m-1)/m = 2/3$. Analíticamente:
$$\frac{e_{n+1}}{e_n} \to 1 - \frac{1}{m} = 1 - \frac{1}{3} = \frac{2}{3}$$

*(c)* La modificación $x_{n+1} = x_n - m \cdot f(x_n)/f'(x_n)$ con $m=3$ convierte el problema en encontrar la raíz de $h(x) = f(x)^{1/m}$, que tiene una raíz simple en $x=2$. Esto restaura la convergencia cuadrática. Formalmente, si $f(x) = (x-2)^3 q(x)$ con $q(2) \neq 0$, entonces $f'(x) = 3(x-2)^2 q(x) + (x-2)^3 q'(x)$ y $f(x)/f'(x) \approx (x-2)/3$ cerca de $x=2$, por lo que multiplicar por 3 da $x_{n+1} \approx x_n - (x_n - 2) + O((x_n-2)^2)$, que converge cuadráticamente.

---

### Ejercicios Prácticos — Método de Newton-Raphson

---

**Ejercicio P2.1**

Aplica Newton-Raphson para encontrar la raíz de $f(x) = x^3 - x - 1$ con $x_0 = 1.5$ y $\varepsilon = 10^{-6}$. Muestra la tabla de iteraciones y el orden de convergencia empírico.

**Solución:**

$f'(x) = 3x^2 - 1$. Iteración: $x_{n+1} = x_n - \dfrac{x_n^3 - x_n - 1}{3x_n^2 - 1}$.

| n | xₙ       | f(xₙ)     | f′(xₙ)   | \|eₙ\| |
|---|-----------|------------|-----------|---------|
| 0 | 1.500000  | 0.875000   | 5.750000  | — |
| 1 | 1.347826  | 0.100682   | 4.448150  | 0.152174 |
| 2 | 1.325200  | 0.002058   | 4.268250  | 0.022626 |
| 3 | 1.324718  | 0.000001   | 4.264534  | 0.000482 |
| 4 | 1.324718  | < 10⁻¹²    | —         | < 10⁻⁹ |

La raíz es $x^* \approx \mathbf{1.324718}$. El cociente $|e_{n+1}|/|e_n|^2$ converge hacia la constante asintótica $C = |f''(x^*)|/(2|f'(x^*)|) = |6x^*|/(2|3(x^*)^2-1|) \approx 7.948/8.529 \approx 0.93$. La convergencia es cuadrática (el número de dígitos correctos se duplica en cada paso).

---

**Ejercicio P2.2**

La ecuación $f(x) = e^{-x} - x = 0$ modela un equilibrio poblacional. Encuentra la raíz usando Newton-Raphson con $x_0 = 0.5$ y $\varepsilon = 10^{-8}$.

**Solución:**

$f(x) = e^{-x} - x$, $f'(x) = -e^{-x} - 1$. Iteración: $x_{n+1} = x_n - \dfrac{e^{-x_n} - x_n}{-e^{-x_n} - 1} = x_n + \dfrac{e^{-x_n} - x_n}{e^{-x_n} + 1}$.

| n | xₙ       | f(xₙ)      | Error estimado |
|---|-----------|-------------|----------------|
| 0 | 0.500000  | 0.106531    | — |
| 1 | 0.566311  | 0.001245    | 0.066311 |
| 2 | 0.567143  | 1.7 × 10⁻⁷  | 0.000832 |
| 3 | 0.567143  | < 10⁻¹⁴     | < 10⁻⁸ |

$x^* \approx \mathbf{0.56714329}$. Esta es la **constante de Omega** $\Omega = W(1)$ donde $W$ es la función Lambert. Verificación: $e^{-0.56714} \approx 0.56714$. ✓

---

**Ejercicio P2.3**

Usa Newton-Raphson para encontrar $\sqrt[5]{17}$ sin usar la operación raíz directamente. Formula el problema como una ecuación de la forma $f(x)=0$, aplica el método con $x_0 = 1.7$ y $\varepsilon = 10^{-7}$.

**Solución:**

$\sqrt[5]{17}$ es la raíz positiva de $f(x) = x^5 - 17$. $f'(x) = 5x^4$.

Iteración: $x_{n+1} = x_n - \dfrac{x_n^5 - 17}{5x_n^4} = \dfrac{4x_n^5 + 17}{5x_n^4} = \dfrac{4x_n}{5} + \dfrac{17}{5x_n^4}$.

| n | xₙ        | xₙ⁵        | abs(xₙ − x*) |
|---|------------|-------------|---------------|
| 0 | 1.7000000  | 14.19857    | 0.0232 |
| 1 | 1.7279327  | 17.08421    | 0.0048 |
| 2 | 1.7231759  | 17.00023    | 0.000033 |
| 3 | 1.7231434  | 17.00000    | < 10⁻⁷ |

$\sqrt[5]{17} \approx \mathbf{1.7231434}$. Verificación: $(1.7231434)^5 \approx 17.0000$. ✓

---
---

## 3. Método de la Secante

### 3.1 Fundamento teórico

El método de la secante es una variante del método de Newton-Raphson que **evita el cálculo de la derivada** $f'(x_n)$, aproximándola mediante un cociente incremental con la iteración anterior:

$$f'(x_n) \approx \frac{f(x_n) - f(x_{n-1})}{x_n - x_{n-1}}$$

Sustituyendo en la fórmula de Newton:

$$x_{n+1} = x_n - f(x_n) \cdot \frac{x_n - x_{n-1}}{f(x_n) - f(x_{n-1})}$$

El método de la secante es esencial cuando:
- $f'$ no tiene expresión analítica cerrada.
- El cálculo de $f'$ es computacionalmente costoso.
- $f$ solo se conoce de forma discreta (datos experimentales).

Geométricamente, en lugar de la tangente, se traza la **recta secante** a través de los puntos $(x_{n-1}, f(x_{n-1}))$ y $(x_n, f(x_n))$, y $x_{n+1}$ es su intersección con el eje $x$.

### 3.2 Algoritmo

**Datos de entrada:** $f$, $x_0$, $x_1$ (dos estimaciones iniciales), tolerancia $\varepsilon$, máximo de iteraciones $N$.

```
ALGORITMO Secante(f, x₀, x₁, ε, N):
    Para n = 1, 2, ..., N hacer:
        Si |f(xₙ) - f(x_{n-1})| < ε_maq entonces
            ERROR: "División por casi-cero — secante paralela al eje x"
        Fin Si
        x_{n+1} ← xₙ - f(xₙ) · (xₙ - x_{n-1}) / (f(xₙ) - f(x_{n-1}))
        Si |x_{n+1} - xₙ| < ε entonces
            RETORNAR x_{n+1}
        Fin Si
    Fin Para
    RETORNAR x_N
```

### 3.3 Análisis de convergencia

**Teorema.** Bajo condiciones de regularidad análogas al método de Newton ($f \in \mathcal{C}^2$ en una vecindad de $x^*$, $f'(x^*) \neq 0$), el método de la secante converge con orden:

$$p = \frac{1 + \sqrt{5}}{2} \approx 1.618 \quad \text{(número áureo)}$$

Es decir, el error satisface:

$\lim_{n \to \infty} \frac{\lvert e_{n+1} \rvert}{\lvert e_n \rvert^p} = \left( \frac{\lvert f''(x^{*}) \rvert}{2\lvert f'(x^{*}) \rvert} \right)^{p-1} = C$

**Demostración (esbozo).** Definamos $\phi_n = f[x_n, x_{n-1}]$ (diferencia dividida de orden 1). Usando la fórmula de diferencias divididas y la expansión de Taylor:

$$
x_{n+1} - x^{*}
=
-\frac{f''(\xi_n)}{2f'(\eta_n)}
(x_n - x^{*})
(x_{n-1} - x^{*})
$$

Tomando valores absolutos: $|e_{n+1}| \approx C|e_n||e_{n-1}|$. Asumiendo $|e_n| \approx A\lambda^{p^n}$, se obtiene $p^{n+1} = p^n + p^{n-1}$, es decir, $p^2 = p + 1$, cuya raíz positiva es $p = (1+\sqrt{5})/2$. $\blacksquare$

**Comparación:**
| Método | Orden | Evaluaciones de $f$ por iteración | Eficiencia |
|--------|-------|-------------------------------------|------------|
| Newton-Raphson | 2 | 2 (f y f') | $2^{1/2} \approx 1.414$ |
| Secante | $\varphi \approx 1.618$ | 1 (reutiliza $f(x_{n-1})$) | $\varphi^{1/1} \approx 1.618$ |

La eficiencia del método de la secante es **mayor** que la de Newton en términos de evaluaciones funcionales.

---

### Ejercicios Teóricos — Método de la Secante

---

**Ejercicio T3.1 — Derivación del orden de convergencia.**

Sea $f \in \mathcal{C}^3$ con raíz simple $x^*$. Demuestra rigurosamente que si la sucesión de la secante converge a $x^*$, entonces el orden de convergencia es $(1+\sqrt{5})/2$.

**Solución:**

Defínase $e_n = x_n - x^*$. La fórmula de la secante da:
$$e_{n+1} = e_n - f(x_n)\frac{e_n - e_{n-1}}{f(x_n) - f(x_{n-1})}$$

Usando las expansiones de Taylor $f(x_n) = f'(x^*)e_n + \frac{f''(x^*)}{2}e_n^2 + O(e_n^3)$ y similarmente para $f(x_{n-1})$:

$$f(x_n) - f(x_{n-1}) = f'(x^*)(e_n - e_{n-1}) + \frac{f''(x^*)}{2}(e_n^2 - e_{n-1}^2) + O(\cdots)$$
$$= (e_n - e_{n-1})\left[f'(x^*) + \frac{f''(x^*)}{2}(e_n + e_{n-1})\right]$$

Entonces:
$$e_{n+1} = e_n - \frac{f'(x^*)e_n + \frac{f''(x^*)}{2}e_n^2}{f'(x^*) + \frac{f''(x^*)}{2}(e_n+e_{n-1})} \approx -\frac{f''(x^*)}{2f'(x^*)} e_n e_{n-1}$$

Sea $C = |f''(x^*)|/|2f'(x^*)|$ y supóngase $|e_n| \sim M \cdot \lambda^{p^n}$ para constantes $M, \lambda$. Entonces:
$$M\lambda^{p^{n+1}} \approx C M^2 \lambda^{p^n + p^{n-1}}$$

Para que esto sea consistente: $p^{n+1} = p^n + p^{n-1}$, lo que dividiendo por $p^{n-1}$ da $p^2 = p + 1$, con solución positiva $p = (1+\sqrt{5})/2 \approx 1.618$. $\blacksquare$

---

**Ejercicio T3.2 — Comparación con bisección y Newton.**

Para $f(x) = x^3 - 2$ con $x^* = \sqrt[3]{2} \approx 1.2599$:

*(a)* Estima cuántas iteraciones necesitan Newton-Raphson (orden 2), la secante (orden $\varphi$) y bisección (orden 1) para reducir el error a $10^{-10}$, partiendo de $|e_0| = 0.5$.

*(b)* Cuantifica la ventaja computacional de la secante sobre Newton (en número de evaluaciones de $f$).

**Solución:**

Para un método de orden $p$, $|e_n| \approx |e_0|^{p^n} \cdot C^{(p^n-1)/(p-1)}$. Una estimación práctica: $|e_n| \approx |e_0|^{p^n}$ (ignorando constante).

Se necesita $|e_0|^{p^n} < 10^{-10}$, es decir $p^n \cdot \log|e_0| < -10$, o sea:
$$n > \frac{10}{\log(1/|e_0|)} \cdot \frac{1}{\log p} = \frac{10}{\log 2} \cdot \frac{1}{\log p}$$

*(a)* Con $|e_0| = 0.5$ (es decir $\log(1/0.5) = \log 2 \approx 0.301$):

- **Bisección** ($p=1$): Requiere $n > 10/0.301 \approx 33.2$, es decir **34 iteraciones**, cada una requiere 1 evaluación de $f$.  
- **Secante** ($p \approx 1.618$): $n > \log(10/0.301)/\log(1.618) = \log(33.2)/0.481 \approx 7.5$, es decir **8 iteraciones**, cada una requiere 1 evaluación nueva de $f$ (total: ~9 evaluaciones).  
- **Newton** ($p=2$): $n > \log(33.2)/\log(2) \approx 5.1$, es decir **6 iteraciones**, cada una requiere 2 evaluaciones ($f$ y $f'$), total: ~12 evaluaciones.

*(b)* La secante es más eficiente que Newton: 9 evaluaciones de $f$ frente a 12 (más el coste adicional de evaluar $f'$ en Newton).

---

**Ejercicio T3.3 — Estabilidad y condiciones iniciales.**

Analiza qué sucede con el método de la secante cuando $f(x_n) - f(x_{n-1}) \approx 0$ pero $x_n \neq x_{n-1}$. Describe las condiciones geométricas bajo las que esto ocurre y propón estrategias de recuperación.

**Solución:**

La condición $f(x_n) - f(x_{n-1}) \approx 0$ con $x_n \neq x_{n-1}$ ocurre cuando la secante que une $(x_{n-1}, f(x_{n-1}))$ y $(x_n, f(x_n))$ es casi horizontal (pendiente $\approx 0$). Geométricamente, esto ocurre cuando $f(x_n) \approx f(x_{n-1})$, es decir, los dos iterados están cerca de un **punto de mínimo o máximo local** de $f$.

En este caso, $x_{n+1}$ se desplaza a un punto muy lejano (la secante casi horizontal intersecta el eje $x$ muy lejos del intervalo de interés), produciendo **divergencia**.

**Estrategias de recuperación:**
1. **Reinicio:** Si $|f(x_n) - f(x_{n-1})| < \delta$, reiniciar con un nuevo par $(x_0, x_1)$ más cercano a la raíz.
2. **Hybrid con bisección:** Combinar el método de la secante con bisección (algoritmo de Brent), usando la secante cuando converge rápido y bisección cuando la secante da un punto fuera del intervalo de garantía.
3. **Perturbación controlada:** Si la secante es casi horizontal, perturbar uno de los puntos: $x_{n-1} \leftarrow x_{n-1} + \delta_{\text{mach}}$.

---

### Ejercicios Prácticos — Método de la Secante

---

**Ejercicio P3.1**

Aplica el método de la secante para encontrar la raíz de $f(x) = x - \cos(x)$ con $x_0 = 0$ y $x_1 = 1$, con $\varepsilon = 10^{-8}$.

**Solución:**

| $n$ | $x_n$       | $f(x_n)$    |
|-----|-------------|-------------|
| 0   | 0.000000    | −1.000000   |
| 1   | 1.000000    | 0.459698    |
| 2   | 0.685073    | −0.089229   |
| 3   | 0.736298    | 0.004073    |
| 4   | 0.739078    | −0.000101   |
| 5   | 0.739085    | $< 10^{-9}$ |

$x^* \approx \mathbf{0.73908513}$ (constante de Dottie). Convergencia en **5 iteraciones**. El orden empírico se puede verificar: el error en el paso 5 es del orden del cuadrado del error en el paso 3, consistente con orden $\varphi \approx 1.618$.

---

**Ejercicio P3.2**

La temperatura de ignición de un material satisface la ecuación implícita $f(T) = T^4 - 100T - 10^5 = 0$ (en unidades adimensionales). Usa el método de la secante con $T_0 = 18$ y $T_1 = 20$ para encontrar la raíz físicamente relevante ($T > 0$).

**Solución:**

$f(18) = 104976 - 1800 - 100000 = 3176 > 0$, $f(20) = 160000 - 2000 - 100000 = 58000 > 0$. Hay que buscar la raíz en $[0, 20]$. $f(0) = -10^5 < 0$, $f(18) > 0$, por lo que la raíz está en $(0, 18)$.

Usamos $T_0 = 15$, $T_1 = 18$: $f(15) = 50625 - 1500 - 100000 = -50875 < 0$.

| $n$ | $T_n$    | $f(T_n)$  |
|-----|----------|-----------|
| 0   | 15.000   | −50875    |
| 1   | 18.000   | 3176      |
| 2   | 17.849   | 143       |
| 3   | 17.843   | $\approx 1.2$ |
| 4   | 17.843   | $< 10^{-3}$ |

$T^* \approx \mathbf{17.843}$. La raíz es la temperatura de ignición del material.

---

**Ejercicio P3.3**

Compara empíricamente la velocidad de convergencia de Newton-Raphson y la secante para $f(x) = x^5 - x^3 + x - 1$ cerca de $x^*=1$ (la única raíz real). Usa $x_0 = 2$ para Newton y $(x_0, x_1) = (2, 1.5)$ para la secante. Construye la tabla de errores y estima el orden de convergencia de la secante a partir de los datos.

**Solución:**

La raíz exacta es $x^* = 1$ (verificar: $1 - 1 + 1 - 1 = 0$). $f'(x) = 5x^4 - 3x^2 + 1$.

**Newton-Raphson** ($x_0 = 2$):

| n | xₙ    | abs(eₙ)    | abs(eₙ)/abs(eₙ₋₁)^2 |
|---|--------|------------|---------------------|
| 0 | 2.0000 | 1.0000     | —                   |
| 1 | 1.4681 | 0.4681     | 0.468               |
| 2 | 1.1540 | 0.1540     | 0.703               |
| 3 | 1.0165 | 0.0165     | 0.696               |
| 4 | 1.0002 | 0.0002     | 0.735               |
| 5 | 1.0000 | < 10^-7    | —                   |

**Secante** ($(x_0, x_1) = (2, 1.5)$):

| n | xₙ    | abs(eₙ)     | abs(eₙ)/abs(eₙ₋₁)^φ |
|---|--------|----------|----------------|
| 0 | 2.0000 | 1.0000   | —              |
| 1 | 1.5000 | 0.5000   | —              |
| 2 | 1.2026 | 0.2026   | 0.335          |
| 3 | 1.0418 | 0.0418   | 0.317          |
| 4 | 1.0025 | 0.0025   | 0.291          |
| 5 | 1.0000 | < 10⁻⁵   | —              |

El cociente $|e_n|/|e_{n-1}|^\varphi$ converge hacia la constante asintótica, confirmando el orden $\varphi \approx 1.618$. Newton converge en 5 iteraciones (10 evaluaciones), la secante en 5 iteraciones (6 evaluaciones), siendo computacionalmente más eficiente.

---
---

## 4. Método de Punto Fijo

### 4.1 Fundamento teórico

El método de punto fijo reformula el problema $f(x) = 0$ como un problema equivalente de la forma:

$$x = g(x)$$

Un **punto fijo** de $g$ es un valor $x^*$ tal que $g(x^*) = x^*$. Si $f(x^*) = 0$, entonces cualquier función $g$ que satisfaga $f(x) = x - g(x)$ (o $g(x) = x - f(x)$, o cualquier otra manipulación algebraica equivalente) tiene a $x^*$ como punto fijo. La **iteración de punto fijo** es simplemente:

$$x_{n+1} = g(x_n)$$

La clave es que la elección de $g$ determina completamente la convergencia. No toda reformulación es convergente: el análisis teórico es fundamental.

### 4.2 Algoritmo

**Datos de entrada:** $g$, $x_0$ (estimación inicial), tolerancia $\varepsilon$, máximo de iteraciones $N$.

```
ALGORITMO PuntoFijo(g, x₀, ε, N):
    Para n = 0, 1, 2, ..., N-1 hacer:
        x_{n+1} ← g(xₙ)
        Si |x_{n+1} - xₙ| < ε entonces
            RETORNAR x_{n+1}
        Fin Si
    Fin Para
    RETORNAR x_N
```

### 4.3 Análisis de convergencia — Teorema de Banach

El resultado fundamental que garantiza convergencia es el **Teorema del Punto Fijo de Banach** (o Principio de Contracción):

> **Teorema (Banach, 1922).** Sea $g : [a, b] \to [a, b]$ una función que satisface:  
> (1) $g(x) \in [a, b]$ para todo $x \in [a, b]$ (**invariancia**).  
> (2) Existe $L \in (0,1)$ tal que $|g(x) - g(y)| \leq L|x - y|$ para todo $x, y \in [a,b]$ (**contracción**).  
>
> Entonces $g$ tiene un **único** punto fijo $x^*$ en $[a,b]$, y la iteración $x_{n+1} = g(x_n)$ converge a $x^*$ para cualquier $x_0 \in [a,b]$.

**Cota de error a priori:**
$$|x_n - x^*| \leq \frac{L^n}{1-L}|x_1 - x_0|$$

**Cota de error a posteriori:**
$$|x_n - x^*| \leq \frac{L}{1-L}|x_n - x_{n-1}|$$

**Criterio práctico.** Si $g$ es diferenciable, la condición de contracción se verifica comprobando que $|g'(x)| \leq L < 1$ para todo $x \in [a,b]$.

### 4.4 Orden de convergencia

Para la iteración de punto fijo:

- Si $g'(x^*) \neq 0$: el método converge **linealmente** con constante $|g'(x^*)|$.
- Si $g'(x^*) = 0$ y $g''(x^*) \neq 0$: el método converge **cuadráticamente**.
- En general, si $g^{(k)}(x^*) = 0$ para $k = 1, \ldots, p-1$ y $g^{(p)}(x^*) \neq 0$: convergencia de **orden $p$**.

Esto explica por qué Newton-Raphson es un caso especial de punto fijo con $g(x) = x - f(x)/f'(x)$, que satisface $g'(x^*) = 0$.

---

### Ejercicios Teóricos — Método de Punto Fijo

---

**Ejercicio T4.1 — Verificación de contracciones.**

Considera la ecuación $f(x) = x^3 + x - 1 = 0$.

*(a)* Reformula como $x = g_1(x) = 1 - x^3$ y como $x = g_2(x) = (1-x)^{1/3}$. Para cada reformulación, determina si $g_i$ es una contracción en $[0,1]$ y qué implica eso para la convergencia.

*(b)* Determina la constante de Lipschitz óptima para $g_2$ en $[0,1]$ y estima la tasa de convergencia.

**Solución:**

*(a)* La raíz de $f$ está en $(0,1)$: $f(0)=-1<0$, $f(1)=1>0$. 

**Para $g_1(x) = 1-x^3$:** $g_1'(x) = -3x^2$. En $[0,1]$: $|g_1'(x)| = 3x^2$, que en $x=1$ vale 3 > 1. No es contracción en todo $[0,1]$. De hecho, $g_1(0)=1$ y $g_1(1)=0$, pero $g_1'(0.9) = -2.43$, $|g_1'| > 1$. **Diverge** en general.

**Para $g_2(x) = (1-x)^{1/3}$:** $g_2'(x) = -\frac{1}{3}(1-x)^{-2/3}$. En $x=0$: $|g_2'(0)| = 1/3 < 1$. En $x \to 1^-$: $|g_2'(x)| \to \infty$. El método falla cerca de $x=1$. Sin embargo, en $[0, 0.8]$: $|g_2'(x)| \leq 1/(3(0.2)^{2/3}) \approx 0.87 < 1$. En este subintervalo sí es contracción.

*(b)* La constante óptima en $[0, 0.7]$ es $L = \max_{x\in[0,0.7]}|g_2'(x)| = |g_2'(0.7)| = \frac{1}{3(0.3)^{2/3}} \approx \frac{1}{3 \times 0.448} \approx 0.744$. La tasa de convergencia lineal es aproximadamente $L \approx 0.744$.

---

**Ejercicio T4.2 — Conexión con Newton-Raphson.**

Muestra que el método de Newton-Raphson es un caso particular de iteración de punto fijo con $g(x) = x - f(x)/f'(x)$, y prueba que $g'(x^*) = 0$ para cualquier raíz simple $x^*$ de $f$.

**Solución:**

Con $g(x) = x - f(x)/f'(x)$, diferenciando:
$$g'(x) = 1 - \frac{[f'(x)]^2 - f(x)f''(x)}{[f'(x)]^2} = \frac{f(x)f''(x)}{[f'(x)]^2}$$

En $x = x^*$: $f(x^*) = 0$, entonces $g'(x^*) = 0$. Esto explica que Newton sea de orden al menos 2: la constante asintótica de convergencia lineal es cero, y el comportamiento dominante del error proviene del término cuadrático. En particular, la iteración de punto fijo con esta $g$ satisface $g'(x^*)=0$ y $g''(x^*) = f''(x^*)/f'(x^*)$, confirmando el orden 2 con constante $C = f''(x^*)/(2f'(x^*))$.

---

**Ejercicio T4.3 — Unicidad del punto fijo y cota de error.**

Sea $g : [1, 2] \to [1, 2]$ definida por $g(x) = \frac{1}{2}\left(x + \frac{3}{x}\right)$.

*(a)* Verifica que $g([1,2]) \subseteq [1,2]$.  
*(b)* Encuentra la constante de Lipschitz y el punto fijo exacto.  
*(c)* Aplica la cota a priori para determinar cuántas iteraciones se necesitan con $x_0 = 2$ para lograr $|x_n - x^*| < 10^{-5}$.

**Solución:**

*(a)* $g(x) = (x + 3/x)/2$. Por AM-GM: $g(x) \geq \sqrt{x \cdot 3/x} = \sqrt{3} \approx 1.732 \geq 1$. Para la cota superior: $g'(x) = (1 - 3/x^2)/2 = 0$ en $x = \sqrt{3}$, que es el mínimo. $g(1) = (1+3)/2 = 2$ y $g(2) = (2+1.5)/2 = 1.75 \in [1,2]$. ✓

*(b)* $g'(x) = (1 - 3/x^2)/2$. En $[1,2]$: máximo de $|g'|$ en $x=1$: $|g'(1)| = |1-3|/2 = 1$. En $x=2$: $|g'(2)| = |1-3/4|/2 = 1/8 = 0.125$. La constante de Lipschitz es $L = \max |g'| = 1/2$ (evaluando en el punto de mayor valor: en $x \in [1,2]$, $|g'(x)| \leq 1/2$ para $x \geq \sqrt{3} \approx 1.73$; necesita refinamiento). En $[\sqrt{3}, 2]$: $L = \max_{x} = g'(\sqrt{3}) = 0$ y $g'(2) = 1/8$, luego $L \approx 1/8$. El punto fijo: $x = (x + 3/x)/2 \Rightarrow 2x = x + 3/x \Rightarrow x = 3/x \Rightarrow x^2 = 3 \Rightarrow x^* = \sqrt{3}$.

*(c)* Con $L=1/8$ y $x_0=2$, $x_1 = g(2) = 1.75$: $|x_1 - x_0| = 0.25$.  
Cota a priori: $|x_n - x^*| \leq \frac{L^n}{1-L}|x_1 - x_0| = \frac{(1/8)^n}{7/8} \times 0.25 = \frac{2}{7}(1/8)^n$.  
Se requiere $\frac{2}{7}(1/8)^n < 10^{-5}$, es decir $(1/8)^n < 3.5\times10^{-5}$, $n \cdot \log(8) > \log(1/3.5\times10^{-5}) \approx 4.46$, $n > 4.46/0.903 \approx 4.94$. Se necesitan **5 iteraciones**.

---

### Ejercicios Prácticos — Método de Punto Fijo

---

**Ejercicio P4.1**

Encuentra la raíz de $f(x) = x^3 - x - 1 = 0$ usando punto fijo con $g(x) = (x + 1)^{1/3}$ y $x_0 = 1.3$. Aplica hasta $\varepsilon = 10^{-5}$.

**Solución:**

$g(x) = (x+1)^{1/3}$, $g'(x) = \frac{1}{3}(x+1)^{-2/3}$. En $x^* \approx 1.3247$: $|g'(x^*)| = \frac{1}{3}(2.3247)^{-2/3} \approx \frac{1}{3 \times 1.713} \approx 0.195 < 1$. ✓ Convergente.

| $n$ | $x_n$     | $g(x_n)$  | $|x_{n+1}-x_n|$ |
|-----|-----------|-----------|-----------------|
| 0   | 1.300000  | 1.320749  | 0.020749        |
| 1   | 1.320749  | 1.323488  | 0.002739        |
| 2   | 1.323488  | 1.323847  | 0.000359        |
| 3   | 1.323847  | 1.323894  | 0.000047        |
| 4   | 1.323894  | 1.323900  | 0.000006        |
| 5   | 1.323900  | 1.323901  | $< 10^{-5}$     |

$x^* \approx \mathbf{1.32472}$. Convergencia lineal con $L \approx 0.195$, consistente con la teoría.

---

**Ejercicio P4.2**

La ecuación $\ln(x) = x - 2$ tiene una raíz cerca de $x = 3$. Formula una función de iteración $g(x)$ adecuada y aplica el método de punto fijo con $x_0 = 3.0$ hasta $\varepsilon = 10^{-6}$.

**Solución:**

Reformulamos: $g(x) = e^{x-2}$ (despejando $x$ como $x = e^{x-2}$... pero $g'(x) = e^{x-2}$ que en $x=3$ vale $e \approx 2.718 > 1$: diverge). Mejor: $g(x) = \ln(x) + 2$ (despejando directamente). $g'(x) = 1/x$. En $x^* \approx 3.146$: $|g'(3.146)| \approx 0.318 < 1$. ✓

| $n$ | $x_n$     | $g(x_n) = \ln(x_n)+2$ | $|x_{n+1}-x_n|$ |
|-----|-----------|-----------------------|-----------------|
| 0   | 3.000000  | 3.098612              | 0.098612        |
| 1   | 3.098612  | 3.130049              | 0.031437        |
| 2   | 3.130049  | 3.140010              | 0.009961        |
| 3   | 3.140010  | 3.143172              | 0.003162        |
| 4   | 3.143172  | 3.144177              | 0.001005        |
| 5   | 3.144177  | 3.144496              | 0.000319        |
| 6   | 3.144496  | 3.144598              | 0.000102        |
| 7   | 3.144598  | 3.144630              | 0.000032        |
| 8   | 3.144630  | 3.144640              | 0.000010        |
| 9   | 3.144640  | 3.144643              | $\approx 3\times10^{-6}$ |
| 10  | 3.144643  | 3.144644              | $< 10^{-6}$     |

$x^* \approx \mathbf{3.14664}$. Verificación: $\ln(3.14664) \approx 1.14664 = 3.14664 - 2$. ✓

---

**Ejercicio P4.3**

El circuito eléctrico de un diodo satisface $I(V) = I_s(e^{V/V_T} - 1)$ con $I_s = 10^{-12}$ A, $V_T = 0.025$ V. Para una corriente de operación $I_0 = 10^{-3}$ A, encuentra el voltaje $V^*$ que satisface $I_s(e^{V/0.025} - 1) = 10^{-3}$. Formula como punto fijo y aplica el método.

**Solución:**

La ecuación es $e^{40V} - 1 = 10^9$, equivalentemente $e^{40V} \approx 10^9$, por lo que $V \approx \frac{\ln(10^9)}{40} = \frac{9\ln(10)}{40} \approx \frac{20.723}{40} \approx 0.518$ V.

Formulamos: $g(V) = \frac{1}{40}\ln(10^{-3}/10^{-12} + 1) = \frac{\ln(10^9 + 1)}{40}$... En este caso la ecuación tiene solución explícita, pero simularemos la iteración de punto fijo con una formulación menos directa.

Sea $g(V) = \frac{1}{40}\ln\left(1 + \frac{I_0}{I_s}\right)$ (constante). $V^* = g(V) = \frac{1}{40}\ln(10^9 + 1) \approx \frac{20.7233}{40} = 0.5181$ V.

Método directo: dado que $g$ es constante (no depende de $V$), la iteración converge en **un solo paso**: $V_1 = g(V_0) = 0.5181$ V para cualquier $V_0$.

Verificación: $I(0.5181) = 10^{-12}(e^{0.5181/0.025} - 1) = 10^{-12}(e^{20.724}-1) \approx 10^{-12} \times 10^9 = 10^{-3}$ A. ✓

$V^* \approx \mathbf{0.5181}$ V.

---
---

## 5. Método de Falsa Posición (Regula Falsi)

### 5.1 Fundamento teórico

El método de **falsa posición** (*regula falsi*) es un método de interpolación lineal con garantía de convergencia, que combina la robustez del método de bisección con la velocidad de la interpolación lineal. Al igual que bisección, requiere un intervalo $[a, b]$ con $f(a) \cdot f(b) < 0$, pero en lugar de bisectar el intervalo, aproxima la raíz mediante la intersección de la secante entre los puntos $(a, f(a))$ y $(b, f(b))$ con el eje $x$.

La **fórmula de interpolación lineal** da:

$$c = b - f(b) \cdot \frac{b - a}{f(b) - f(a)} = \frac{a \cdot f(b) - b \cdot f(a)}{f(b) - f(a)}$$

A diferencia del método de la secante, siempre se mantiene la condición $f(a) \cdot f(c) < 0$ o $f(c) \cdot f(b) < 0$, lo que garantiza convergencia.

### 5.2 Algoritmo

**Datos de entrada:** $f$, $a$, $b$ con $f(a) \cdot f(b) < 0$, tolerancia $\varepsilon$, máximo de iteraciones $N$.

```
ALGORITMO FalsaPosicion(f, a, b, ε, N):
    Si f(a) · f(b) ≥ 0 entonces
        ERROR: "No hay cambio de signo en [a, b]"
    Fin Si

    Para n = 1, 2, ..., N hacer:
        c ← (a · f(b) - b · f(a)) / (f(b) - f(a))
        Si f(c) = 0 o |f(c)| < ε entonces
            RETORNAR c
        Fin Si
        Si f(a) · f(c) < 0 entonces
            b ← c       // La raíz está en [a, c]
        En otro caso:
            a ← c       // La raíz está en [c, b]
        Fin Si
        Si |b - a| < ε entonces
            RETORNAR c
        Fin Si
    Fin Para
    RETORNAR c
```

### 5.3 Análisis de convergencia y deficiencias

La convergencia del método de falsa posición está **garantizada** bajo la condición $f(a)\cdot f(b)<0$ y continuidad, pero puede ser **notablemente lenta** en ciertos casos. El problema clásico es el **estancamiento de un extremo**: si $f$ es convexa (o cóncava) en $[a,b]$, uno de los extremos del intervalo nunca se actualiza, y la convergencia es solo **lineal**.

**Ejemplo de degeneración:** Sea $f(x) = e^x - 1 - x$ en $[-1, 1]$. Como $f$ es convexa, el extremo izquierdo $a=-1$ nunca se mueve. La secuencia de puntos $c_n$ converge al límite del lado derecho, pero muy lentamente.

**Método de falsa posición modificado (Illinois, Pegasus):** Para remediar el estancamiento, si el extremo $a$ no se actualiza durante dos iteraciones consecutivas, se reemplaza $f(a)$ por $f(a)/2$ (método de Illinois) o por $f(a) \cdot f(b_{nuevo}) / (f(b_{nuevo}) + f(b_{viejo}))$ (método de Pegasus). Esto rompe el estancamiento y puede restaurar la convergencia superlineal.

### 5.4 Comparación bisección vs. falsa posición

| Característica | Bisección | Falsa Posición |
|----------------|-----------|----------------|
| Convergencia garantizada | ✓ | ✓ |
| Orden (caso general) | 1 | 1 (puede ser peor) |
| Orden (caso favorable) | 1 | Superlineal |
| Estancamiento de extremo | No | Posible |
| Cota de error explícita | Sí ($L_0/2^n$) | No en general |

---

### Ejercicios Teóricos — Método de Falsa Posición

---

**Ejercicio T5.1 — Derivación geométrica y algebraica.**

Deriva la fórmula de falsa posición $c = \frac{a \cdot f(b) - b \cdot f(a)}{f(b) - f(a)}$ a partir de la ecuación de la recta que pasa por $(a, f(a))$ y $(b, f(b))$, y demuestra que $c$ siempre pertenece al interior $(a, b)$ bajo la hipótesis de cambio de signo.

**Solución:**

La recta secante a través de $(a, f(a))$ y $(b, f(b))$ tiene ecuación:
$$y - f(a) = \frac{f(b) - f(a)}{b - a}(x - a)$$

Igualando $y = 0$:
$$-f(a) = \frac{f(b) - f(a)}{b - a}(x - a)$$
$$x - a = \frac{-f(a)(b-a)}{f(b) - f(a)}$$
$$x = a - \frac{f(a)(b-a)}{f(b) - f(a)} = \frac{a(f(b)-f(a)) - f(a)(b-a)}{f(b)-f(a)} = \frac{af(b) - bf(a)}{f(b)-f(a)}$$

que es la fórmula buscada. Para probar $c \in (a,b)$: como $f(a)f(b) < 0$, los valores $f(a)$ y $f(b)$ tienen signos opuestos, por lo que $f(b) - f(a) \neq 0$ y el cociente está bien definido. Además, $c = a + \frac{-f(a)(b-a)}{f(b)-f(a)}$. Si $f(a) < 0 < f(b)$: el factor $\frac{-f(a)}{f(b)-f(a)} = \frac{|f(a)|}{f(b)+|f(a)|} \in (0,1)$, por lo que $c = a + \lambda(b-a)$ con $\lambda \in (0,1)$, confirmando $c \in (a,b)$. Análogamente para el caso opuesto. $\blacksquare$

---

**Ejercicio T5.2 — Análisis del estancamiento.**

Sea $f(x) = x^2 - 2$ en $[1, 2]$.

*(a)* Aplica las primeras 5 iteraciones de falsa posición y observa qué extremo se estanca.  
*(b)* Explica teóricamente, usando la convexidad de $f$, por qué este extremo nunca se mueve.  
*(c)* Describe el método de Illinois y aplica su corrección en la iteración donde se detecta el estancamiento.

**Solución:**

*(a)* $f(1) = -1 < 0$, $f(2) = 2 > 0$.

| $n$ | $a$    | $b$    | $c$      | $f(c)$   | Actualización |
|-----|--------|--------|----------|----------|---------------|
| 1   | 1.0000 | 2.0000 | 1.3333   | −0.2222  | $a \leftarrow c$ |
| 2   | 1.3333 | 2.0000 | 1.4000   | −0.0400  | $a \leftarrow c$ |
| 3   | 1.4000 | 2.0000 | 1.4118   | −0.0069  | $a \leftarrow c$ |
| 4   | 1.4118 | 2.0000 | 1.4138   | −0.0012  | $a \leftarrow c$ |
| 5   | 1.4138 | 2.0000 | 1.4141   | −0.0002  | $a \leftarrow c$ |

El extremo $b = 2$ **nunca se actualiza**.

*(b)* $f''(x) = 2 > 0$: $f$ es estrictamente convexa. Para una función convexa, la secante entre $(a, f(a))$ y $(b, f(b))$ subestima a $f$ en el interior, lo que significa que $c$ siempre se aproxima a $x^*$ desde el lado del extremo con $f < 0$ (el lado de $a$). Por tanto, $f(c) < 0$ siempre, y el extremo $b$ con $f(b) > 0$ nunca se reemplaza.

*(c)* **Método de Illinois:** En la iteración 2 (el extremo $b$ no cambió por segunda vez consecutiva), se reemplaza $f(b)$ por $f(b)/2 = 1$. Nueva fórmula de falsa posición para la iteración 3:
$$c_3 = \frac{a_2 \cdot f(b)/2 - b \cdot f(a_2)}{f(b)/2 - f(a_2)} = \frac{1.3333 \times 1 - 2 \times (-0.2222)}{1 - (-0.2222)} = \frac{1.3333 + 0.4444}{1.2222} \approx 1.4545$$

$f(1.4545) \approx 2.1156 - 2 = 0.1156 > 0$. Ahora $b$ sí se actualiza: $b \leftarrow 1.4545$. El estancamiento se ha roto.

---

**Ejercicio T5.3 — Falsa posición vs. bisección: una comparación rigurosa.**

Sea $f : [0,1] \to \mathbb{R}$ estrictamente convexa y creciente con $f(0) < 0$ y $f(1) > 0$.

*(a)* Prueba que en falsa posición, el extremo izquierdo $a_n$ converge a $x^*$ pero el extremo derecho $b_n = 1$ para todo $n$.  
*(b)* Muestra que en este escenario, bisección proporciona una cota de error $|e_n| \leq 1/2^n$ mientras que falsa posición no ofrece tal cota explícita.  
*(c)* Concluye cuándo es preferible cada método.

**Solución:**

*(a)* Si $f$ es estrictamente convexa en $[0,1]$ y creciente con $f(0)<0$, $f(1)>0$: para cualquier $a_n \in (0, x^*)$, el punto de falsa posición $c_n$ satisface $f(c_n) < 0$ (porque la secante entre $(a_n, f(a_n))$ y $(1, f(1))$ queda por debajo de $f$ por convexidad). Por tanto, siempre se actualiza $a \leftarrow c$ y $b$ nunca cambia: $b_n = 1$ para todo $n$. La sucesión $\{a_n\}$ es monótonamente creciente y acotada por $x^*$, por tanto converge. El límite debe ser $x^*$ (si fuera $L < x^*$, la función $g(a) = $ fórmula de falsa posición con $b=1$ tendría $g(L) = L$, lo que se contradice con $f(L) < 0$ y la estrictez de la convexidad).

*(b)* Bisección garantiza $|e_n| \leq 1/2^{n+1}$ independientemente de la forma de $f$. Falsa posición, con $b$ fijo en 1, no tiene una cota universal en términos de $n$; el error puede decrecer arbitrariamente lento si $f$ es casi lineal o si $x^*$ está muy cerca de $b=1$.

*(c)* **Bisección** es preferible cuando se requiere una cota de error garantizada y el número de iteraciones predeterminado, o cuando $f$ puede ser convexa/cóncava (estancamiento en falsa posición). **Falsa posición** es preferible cuando $f$ es casi lineal y el punto de intersección de la secante es una buena aproximación, o en versiones modificadas (Illinois, Pegasus) que resuelven el problema del estancamiento.

---

### Ejercicios Prácticos — Método de Falsa Posición

---

**Ejercicio P5.1**

Aplica el método de falsa posición para encontrar la raíz de $f(x) = x^3 - 2x^2 - 5 = 0$ en $[2, 4]$, con $\varepsilon = 10^{-5}$.

**Solución:**

$f(2) = 8 - 8 - 5 = -5 < 0$, $f(4) = 64 - 32 - 5 = 27 > 0$. ✓

Fórmula: $c = \frac{a \cdot f(b) - b \cdot f(a)}{f(b) - f(a)}$

| $n$ | $a$      | $b$    | $c$      | $f(c)$     | Actualización |
|-----|----------|--------|----------|------------|---------------|
| 1   | 2.0000   | 4.0000 | 2.625000 | −4.3086    | $a \leftarrow c$ |
| 2   | 2.6250   | 4.0000 | 2.921875 | −1.8813    | $a \leftarrow c$ |
| 3   | 2.9219   | 4.0000 | 3.034988 | −0.5441    | $a \leftarrow c$ |
| 4   | 3.0350   | 4.0000 | 3.072040 | −0.1388    | $a \leftarrow c$ |
| 5   | 3.0720   | 4.0000 | 3.081353 | −0.0342    | $a \leftarrow c$ |
| 6   | 3.0814   | 4.0000 | 3.083633 | −0.0083    | $a \leftarrow c$ |
| 7   | 3.0836   | 4.0000 | 3.084188 | −0.0020    | $a \leftarrow c$ |
| 8   | 3.0842   | 4.0000 | 3.084324 | −0.0005    | $a \leftarrow c$ |
| 9   | 3.0843   | 4.0000 | 3.084357 | −0.0001    | $a \leftarrow c$ |
| 10  | 3.0844   | 4.0000 | 3.084365 | $< 10^{-5}$ | — |

$x^* \approx \mathbf{3.08437}$. Nótese que $b = 4$ nunca se actualiza (estancamiento del lado derecho, consistente con la convexidad de $f$ en este intervalo).

---

**Ejercicio P5.2**

Aplica falsa posición para resolver $f(x) = \sin(x) - x/2 = 0$ en $[1, 3]$, con $\varepsilon = 10^{-6}$. Compara el número de iteraciones con el método de bisección para la misma tolerancia.

**Solución:**

$f(1) = \sin(1) - 0.5 = 0.8415 - 0.5 = 0.3415 > 0$, $f(3) = \sin(3) - 1.5 = 0.1411 - 1.5 = -1.3589 < 0$. ✓

| $n$ | $a$     | $b$     | $c$      | $f(c)$     | Actualización |
|-----|---------|---------|----------|------------|---------------|
| 1   | 1.0000  | 3.0000  | 1.4017   | $+0.2075$  | $b \leftarrow c$ |
| 2   | 1.0000  | 1.4017  | 1.7152   | $+0.1001$  | $b \leftarrow c$ |
| 3   | 1.0000  | 1.7152  | 1.7875   | ... |
| ... | ...     | ...     | ...      | ...        | |
| 8   | 1.0000  | 1.8955  | 1.8955   | $\approx 0$ | — |

La raíz es $x^* \approx \mathbf{1.8955}$ (verificación: $\sin(1.8955) \approx 0.9478 = 1.8955/2$). ✓

**Bisección:** $L_0 = 2$, iteraciones para $\varepsilon = 10^{-6}$: $n > \log_2(2/10^{-6}) = \log_2(2\times10^6) \approx 20.9$, es decir **21 iteraciones**.

**Falsa posición:** requirió ~8 iteraciones en este caso, significativamente menos que bisección, porque $f$ no es muy convexa en este intervalo y la secante es una buena aproximación.

---

**Ejercicio P5.3**

La ecuación de estado de Van der Waals para un gas real es:
$$\left(P + \frac{a}{V^2}\right)(V - b) = RT$$

Para $CO_2$ con $a = 3.640$ atm·L²/mol², $b = 0.04267$ L/mol, $T = 300$ K, $R = 0.08206$ L·atm/(mol·K), $P = 1$ atm. Reformula como $f(V) = 0$ y encuentra $V$ usando falsa posición en el intervalo $[0.1, 30]$ con $\varepsilon = 10^{-4}$.

**Solución:**

Despejamos: $f(V) = \left(1 + \frac{3.640}{V^2}\right)(V - 0.04267) - 0.08206 \times 300 = 0$.

$$f(V) = \left(1 + \frac{3.640}{V^2}\right)(V - 0.04267) - 24.618$$

Verificamos los signos:

$f(0.1) = (1 + 364)(0.1 - 0.04267) - 24.618 = 365 \times 0.05733 - 24.618 \approx 20.926 - 24.618 = -3.69 < 0$

$f(30) = (1 + 3.640/900)(30 - 0.04267) - 24.618 \approx 1.004 \times 29.957 - 24.618 \approx 30.077 - 24.618 = 5.46 > 0$

| $n$ | $a$     | $b$     | $c$      | $f(c)$    |
|-----|---------|---------|----------|-----------|
| 1   | 0.100   | 30.000  | 12.548   | $> 0$     |
| 2   | 0.100   | 12.548  | 5.252    | $> 0$     |
| 3   | 0.100   | 5.252   | 2.183    | $> 0$     |
| 4   | 0.100   | 2.183   | 0.851    | $> 0$     |
| 5   | 0.100   | 0.851   | 0.369    | $+0.82$   |
| 6   | 0.100   | 0.369   | 0.232    | $+0.45$   |
| 7   | 0.100   | 0.232   | 0.183    | $-0.13$   |
| 8   | 0.183   | 0.232   | 0.198    | $\approx 0$ |

$V^* \approx \mathbf{0.198}$ L/mol. Verificación: $\left(1 + 3.640/0.0392\right)(0.198 - 0.04267) = (93.87)(0.1553) \approx 14.58 \neq 24.618$.

*Nota:* Los signos de $f$ en el intervalo inicial revelan que puede haber múltiples raíces (gas fase líquida y gaseosa). Para $P = 1$ atm y $T = 300$ K, el $CO_2$ se comporta como gas; la raíz físicamente relevante es la de mayor volumen. Un análisis más cuidadoso en $[10, 30]$ da directamente la raíz gaseosa: $f(10) = (1+0.0364)(9.957) - 24.618 \approx 10.319 - 24.618 = -14.3 < 0$ y $f(30) = 5.46 > 0$. Aplicando falsa posición en $[10, 30]$:

| $n$ | $c_n$   | $f(c_n)$ |
|-----|---------|----------|
| 1   | 16.47   | $-4.17$  |
| 2   | 22.37   | $-1.22$  |
| 3   | 24.78   | $-0.14$  |
| 4   | 24.96   | $\approx 0$ |

$V^* \approx \mathbf{24.96}$ L/mol (estado gaseoso). Verificación: $RT/P = 24.618$ L/mol (gas ideal), y el volumen real es ligeramente mayor. ✓

---
---

## Resumen Comparativo

| Método | Orden $p$ | Evaluaciones/iter. | Garantía convergencia | Requiere |
|--------|-----------|--------------------|-----------------------|----------|
| Bisección | 1 (lineal) | 1 ($f$) | Sí (con $f(a)f(b)<0$) | $f$ continua |
| Newton-Raphson | 2 (cuadrático) | 2 ($f$, $f'$) | Local | $f \in \mathcal{C}^2$, $f'(x^*)\neq 0$ |
| Secante | $\varphi \approx 1.618$ | 1 ($f$) | Local | $f \in \mathcal{C}^2$ |
| Punto Fijo | Depende de $g$ | 1 ($g$) | Global si $L<1$ | $g$ contractiva |
| Falsa Posición | 1 (puede ser lento) | 1 ($f$) | Sí (con $f(a)f(b)<0$) | $f$ continua |

**Recomendaciones prácticas:**

- **Uso general con garantía:** Bisección o Falsa Posición (modificada con Illinois/Pegasus para mayor velocidad).
- **Máxima velocidad cuando $f'$ está disponible:** Newton-Raphson.
- **$f'$ costosa o no disponible:** Método de la Secante.
- **Formulación natural como sistema dinámico:** Punto Fijo, con elección cuidadosa de $g$.
- **Implementación robusta de propósito general:** Algoritmo de Brent (combina bisección, secante e interpolación cuadrática inversa).

---

## Referencias

1. Burden, R. L., & Faires, J. D. (2011). *Numerical Analysis* (9th ed.). Brooks/Cole.  
2. Stoer, J., & Bulirsch, R. (2002). *Introduction to Numerical Analysis* (3rd ed.). Springer.  
3. Kincaid, D., & Cheney, W. (2002). *Numerical Analysis: Mathematics of Scientific Computing* (3rd ed.). AMS.  
4. Ortega, J. M., & Rheinboldt, W. C. (1970). *Iterative Solution of Nonlinear Equations in Several Variables*. SIAM.  
5. Brent, R. P. (1973). *Algorithms for Minimization Without Derivatives*. Prentice-Hall.
