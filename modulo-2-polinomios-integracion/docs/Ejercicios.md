## Heath. Scientific Computing
### Chapter 7. Interpolation

**7.1**  Dados los tres puntos de datos $(−1, 1)$, $(0, 0)$, $(1, 1)$, determine el polinomio interpolador de grado dos:

_(a)_ Usando la base monomial

_(b)_ Usando la base de Lagrange

_(c)_ Usando la base de Newton

Muestre que las tres representaciones dan el mismo polinomio.

**7.2**  Exprese el siguiente polinomio en la forma correcta para su evaluación mediante el método de Horner: 
$$p(t) = 5t^3 - 3t^2 + 7t - 2.$$

**7.3** Escriba un algoritmo formal para evaluar un polinomio en un argumento dado utilizando el esquema de evaluación anidada de Horner.

_(a)_ Para un polinomio expresado en términos de la base
monomial.

_(b)_ Para un polinomio expresado en forma de Newton.

**7.4** ¿Cuántas multiplicaciones se requieren para evaluar un polinomio $p(t)$ de grado $n-1$ en un punto dado $t$.

_(a)_ Representado en base monomial

_(b)_ Representado en base de Lagrange

_(c)_ Representado en base de Newton

**7.5**  En general, ¿es posible interpolar $n$ puntos de datos mediante un polinomio _cuadrático_ por partes, con nodos en los puntos de datos dados, de manera que el interpolante sea

_(a)_ ¿Continuamente diferenciable una vez?

_(b)_ ¿Continuamente diferenciable dos veces?

En cada caso, si la respuesta es "sí", explique por qué, y si la respuesta es "no", indique el valor máximo de _n_ para el cual _es_ posible.

**7.6** Suponiendo que $t_1, t_2, ..., t_n$ son distintos, demuestre que la matriz de Vandermonde **A** dada por $a_{ij} = t_i^{j-1}$ es no singular.

**7.7** Compare el costo de formar una matriz de Vandermonde inductivamente, como en la Sección 7.2.1 con el costo utilizando exponenciación explícita.

**7.2.1 Evaluación de Polinomios**

Además del costo de determinar la función interpolante, el costo de evaluarla en un punto dado es un factor importante al elegir un método de interpolación. Cuando se representa en la base monomial, un polinomio

$p_{n-1}(t) = x_1 + x_2 t + \cdots + x_n t^{n-1}$

puede evaluarse de manera muy eficiente utilizando el *método de Horner*, también conocido como *evaluación anidada* o *división sintética*:

$p_{n-1}(t) = x_1 + t(x_2 + t(x_3 + t(\cdots (x_{n-1} + x_n t)\cdots)))$

lo cual requiere únicamente $n$ sumas y $n$ multiplicaciones. Por ejemplo,

$1 - 4t + 5t^2 - 2t^3 + 3t^4 = 1 + t(-4 + t(5 + t(-2 + 3t)))$.

El mismo principio se aplica al formar una matriz de Vandermonde:

$a_{i,j} = \phi_j(t_i) = t_i^{j-1} = t_i \phi_{j-1}(t_i) = t_i a_{i,j-1} \quad \text{para } j = 2, \ldots, n,$

lo cual es superior al uso de exponenciación explícita.

Otras manipulaciones del polinomio interpolante, como la diferenciación o la integración, también son relativamente sencillas con la representación en base monomial.

**7.8** Utilice la interpolación de Lagrange para derivar la fórmulas dadas en la Sección 5.2.5 para inversa interpolación cuadrática.

**5.2.5 Interpolación Inversa**

En cada iteración del método de la secante, se ajusta una línea recta a dos valores de la función cuyo cero se busca. Se puede obtener una mayor tasa de convergencia (pero no superior a $r = 2$) ajustando un polinomio de mayor grado al número apropiado de valores de la función. Por ejemplo, se podría ajustar un polinomio cuadrático a tres iteraciones sucesivas y usar una de sus raíces como la siguiente solución aproximada. Sin embargo, existen varias dificultades con esta idea: el polinomio puede no tener raíces reales, e incluso si las tiene, pueden no ser fáciles de calcular, y puede no ser sencillo elegir cuál raíz usar como la siguiente iteración. (Por otro lado, si se busca una raíz compleja, entonces es deseable un polinomio con raíces complejas; en el método de Müller, por ejemplo, se utiliza un polinomio cuadrático para aproximar raíces complejas).

Una respuesta a estas dificultades se proporciona mediante la interpolación inversa, en la cual se ajustan los valores $x_k$ como función de los valores $y_k = f(x_k)$, por ejemplo, mediante un polinomio $p(y)$, de modo que la siguiente solución aproximada sea simplemente $p(0)$. Esta idea se ilustra en la Fig. 5.7, donde una parábola que ajusta $y$ como función de $x$ no tiene raíz real (es decir, no cruza el eje $x$), pero una parábola que ajusta $x$ como función de $y$ simplemente se evalúa en cero para obtener la siguiente iteración.

<img src="../../images/interpolation.png" alt="interpolation" width="75%"/>

Utilizando la interpolación cuadrática inversa, en cada iteración se tienen tres valores aproximados de la solución, que denotamos por $a$, $b$ y $c$, con los valores correspondientes de la función $f_a$, $f_b$ y $f_c$, respectivamente. La siguiente solución aproximada se encuentra ajustando un polinomio cuadrático a $a$, $b$ y $c$ como una función de $f_a$, $f_b$, y $f_c$, y luego evaluando el polinomio en 0. Esta tarea se realiza mediante las siguientes fórmulas, cuya deducción se aclarará después de que estudiemos la interpolación de Lagrange en la Sección 7.2.2.

$$u = frac{f_b}{f_c}, v=frac{f_b}{f_a}, w=frac{f_a}{f_c}$$
$$p=v(w(u-w)(c-b)-(1-u)(b-a)), q=(w-1)(u-1)(v-1)$$

La interpolación fraccional lineal también es efectiva como un método general para encontrar ceros en una dimensión, como ilustra el siguiente ejemplo. Su tasa de convergencia asintótica es la misma que la de la interpolación cuadrática (inversa o regular), $r \approx 1.839$. Una vez más, este resultado es local, y las iteraciones deben iniciarse lo suficientemente cerca de la solución para obtener convergencia.

**Ejemplo 5.10 Interpolación Fraccional Lineal.** Ilustramos la interpolación fraccional lineal encontrando nuevamente una raíz de la ecuación

$f(x) = x^2 - 4 \sin(x) = 0.$

Tomando $a = 1$, $b = 2$ y $c = 3$ como valores iniciales, la secuencia de iteraciones se muestra a continuación.

| $x$       | $f(x)$    | $h$        |
|----------|----------|------------|
| 1.000000 | -2.365884 |            |
| 2.000000 | 0.362810  |            |
| 3.000000 | 8.435520  |            |
| 1.906953 | -0.139647 | -1.093047  |
| 1.93335  | -0.002131 | 0.026398   |
| 1.933756 | 0.000013  | -0.000406  |
| 1.933754 | 0.000000  | -0.000003  |

---

#### Computing Problems

**7.1** _(a)_  Escriba una rutina que utilice la regla de Horner para evaluar un polinomio $p(t)$ dado su grado $n$, un array $x$ que contiene sus coeficientes, y el valor $t$ de la variable independiente en que debe ser evaluado.

_(b)_ Agregue opciones a su rutina para evaluar la derivada $p'(t)$ o la integral $\int_a^b p(t)dt$, dados $a$ y $b$.

**7.2** _(a)_  Escriba una rutina para calcular el
Interpolante polinomial de Newton para un conjunto dado
de puntos de datos, y una segunda rutina para evaluar el
interpolante de Newton en un valor de argumento dado
utilizando la regla de Horner.

_(b)_ Escriba una rutina para calcular el nuevo Interpolante polinomial de Newton cuando un se agrega un nuevo punto de datos.

_(c)_ Si su lenguaje de programación admite recursión,
escriba una rutina recursiva que implemente la parte _(a)_ llamando a su rutina para la parte _(b)_ recursivamente. Compare su rendimiento con la de su implementación original.

**7.3** _(a)_ Escriba el sistema de ecuaciones derivado en el Ejemplo 7.6 en forma matricial.

**7.6 Interpolación con splines cúbicos**

Para ilustrar la interpolación con splines, determinaremos el spline cúbico natural interpolante a través de tres puntos de datos $(t_i, y_i)$, $i = 1, 2, 3$. 

El interpolante requerido es una función cúbica por tramos definida por polinomios cúbicos separados en cada uno de los dos intervalos $[t_1, t_2]$ y $[t_2, t_3]$. Denotamos estos dos polinomios por:

$p_1(t) = \alpha_1 + \alpha_2 t + \alpha_3 t^2 + \alpha_4 t^3$

$p_2(t) = \beta_1 + \beta_2 t + \beta_3 t^2 + \beta_4 t^3$

Hay ocho parámetros por determinar, por lo que necesitaremos ocho ecuaciones. 

Exigir que la función cúbica interpole los datos en los extremos del primer intervalo produce las dos ecuaciones:

$\alpha_1 + \alpha_2 t_1 + \alpha_3 t_1^2 + \alpha_4 t_1^3 = y_1$

$\alpha_1 + \alpha_2 t_2 + \alpha_3 t_2^2 + \alpha_4 t_2^3 = y_2$

_(b)_ Utilice una rutina de biblioteca o una propia.
diseño, para resolver el sistema lineal $8 x 8$ resultante utilizando los datos proporcionados en el Ejemplo 7.1.

**Ejemplo 7.1 Base monomial.** Para ilustrar la interpolación polinómica usando la base monomial, encontraremos un polinomio de grado dos que interpole los tres puntos de datos $(-2,-27)$, $(0,-1)$, $(1,0)$. En general, existe un único polinomio

$p_2(t) = x_1 + x_2 t + x_3 t^2$

de grado dos que interpola tres puntos $(t_1, y_1)$, $(t_2, y_2)$, $(t_3, y_3)$. Con la base monomial, los coeficientes del polinomio satisfacen el sistema de ecuaciones lineales

$$
\begin{bmatrix}
1 & t_1 & t_1^2 \\
1 & t_2 & t_2^2 \\
1 & t_3 & t_3^2
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
=
\begin{bmatrix}
y_1 \\
y_2 \\
y_3
\end{bmatrix}
$$

Para este conjunto particular de datos, el sistema se convierte en

$$
\begin{bmatrix}
1 & -2 & 4 \\
1 & 0 & 0 \\
1 & 1 & 1
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
=
\begin{bmatrix}
-27 \\
-1 \\
0
\end{bmatrix}
$$

Resolviendo este sistema por eliminación gaussiana se obtiene la solución $x = [-1 \ \ 5 \ \ -4]^T$, por lo que el polinomio interpolante es

$$p_2(t) = -1 + 5t - 4t^2.$$

**Nota**: Los valores son $(-2,-27)$, $(0,-1)$, $(1,0)$.

_(c)_ Grafique la spline cúbica natural resultante,
junto con los puntos de datos proporcionados. También grafique el primera y segunda derivadas de la spline cúbica y confirmar que se cumplen todas las condiciones requeridas se cumplen.

**7.4** Un experimento ha producido los siguientes datos:

| t   | 0.0 | 0.5 | 1.0 | 6.0 | 7.0 | 9.0 |
|-----|-----|-----|-----|-----|-----|-----|
| y   | 0.0 | 1.6 | 2.0 | 2.0 | 1.5 | 0.0 |

Deseamos interpolar los datos con una curva suave con la esperanza de obtener valores razonables de $y$ para valores de $t$ entre los puntos en qué medidas se tomaron.

_(a)_ Utilizando cualquier método que prefiera, determine el polinomio de grado cinco que interpola el
dados los datos, y hacer un gráfico suave de ellos sobre
el rango $0 ≤ t ≤ 9.$

_(b)_ De forma similar, determine una función spline cúbica que interpole los datos proporcionados y cree una gráfica suavizada de la misma en el mismo rango.

_(c)_ ¿Qué interpolador parece dar valores más razonables entre los puntos de datos dados? ¿Puedes explicar por qué cada curva se comporta de esa manera?

_(d)_ ¿Podría la interpolación lineal por tramos ser una mejor opción para estos datos en particular? ¿Por qué?

**7.5** Interpolación de los puntos de datos

| $t$ | 0 | 1 | 4 | 9 | 16 | 25 | 36 | 49 | 64 |
|-----|---|---|---|---|----|----|----|----|----|
| $y$ | 0 | 1 | 2 | 3 | 4  | 5  | 6  | 7  | 8  |

debería proporcionar una aproximación a la función de raíz cuadrada.

_(a)_ Calcule el polinomio de grado ocho que interpola estos nueve puntos de datos. Represente gráficamente el polinomio resultante, así como los valores correspondientes dados por la función sqrt integrada en el dominio $[0, 64]$.

_(b)_ Utilice una rutina de spline cúbico para interpolar los mismos datos y vuelva a graficar la curva resultante junto con la función _sqrt_ integrada.

_(c)_ ¿Cuál de los dos interpolantes es más preciso en la mayor parte del dominio?

_(d)_ ¿Cuál de los dos interpolantes es más preciso entre $0$ y $1$?

**7.6** La función gamma esta definida por


$$\Gamma(x) = \int_{0}^{\infty} t^{x-1} e^{-t} \, dt, \quad x > 0.$$

Para un argumento entero n, la función gamma tiene el valor

$$\Gamma(n) = (n - 1)!$$

por lo tanto, interpolando los puntos de datos

| $t$ | 1 | 2 | 3 | 4 | 5 |
|-----|---|---|---|---|---|
| $y$ | 1 | 1 | 2 | 6 | 24 |

