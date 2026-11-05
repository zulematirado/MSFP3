[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/)

# Práctica 3: Modelado y control del sistema musculoesquelético

## Información de la estudiante

Tirado Manzo Zulema Guadalupe [23210724]; L23210724@tijuana.tecnm.mx

Modelado de Sistemas Fisiológicos

Ingeniería Biomédica

## Docente

Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica, Tecnológico Nacional de México/IT Tijuana, Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, B.C., México.

## Descripción de la asignatura

El modelado de sistemas fisiológicos permite representar mediante modelos matemáticos y computacionales el comportamiento de diferentes sistemas del cuerpo humano. En Ingeniería Biomédica se utilizan analogías físicas y eléctricas, ecuaciones diferenciales y técnicas de control para estudiar su respuesta dinámica y analizar posibles modificaciones mediante simulación.

En esta práctica se relacionan los principios de la mecánica, los circuitos eléctricos y la Ingeniería de Control con el funcionamiento del sistema musculoesquelético, utilizando MATLAB, Simulink, Simscape Electrical y Python. El trabajo se desarrolla mediante experimentación *in silico* con fines académicos.

## Objetivos

1. Identificar los elementos contráctiles, elásticos y viscosos del modelo musculoesquelético.
2. Construir el circuito equivalente mediante la analogía fuerza–voltaje.
3. Obtener las ecuaciones del sistema y desarrollar el análisis mediante superposición y transformada de Laplace.
4. Determinar las funciones de transferencia del individuo sano (control) y del caso simulado de atrofia muscular.
5. Calcular el error en estado estacionario, los polos y la estabilidad de ambos modelos.
6. Simular la respuesta al impulso unitario de 0 a 10 segundos en MATLAB/Simulink y Simscape Electrical.
7. Diseñar y sintonizar un controlador PID para reducir la diferencia entre el caso y la referencia del individuo sano.
8. Comparar las respuestas del control, el caso y el sistema controlado en lazo cerrado.
9. Reproducir las simulaciones en Python y verificar la concordancia entre plataformas.
10. Elaborar el diagrama biológico del sistema y documentar los resultados en GitHub.

## Descripción detallada del sistema

Se estudia un modelo mecánico simplificado del sistema musculoesquelético, integrado por una fuerza contráctil activa, un amortiguador viscoso y dos elementos elásticos. La fuerza F0(t) representa la acción contráctil del músculo, F(t) la fuerza transmitida, R la amortiguación viscosa, Cp la compliancia del elemento paralelo y Cs la compliancia del elemento en serie. Los desplazamientos se representan por x(t) y x1(t). La relación entre fuerzas es F0(t) = α F(t), con 0 < α < 1.

Mediante la analogía fuerza–voltaje, la fuerza corresponde al voltaje, la velocidad a la corriente, el desplazamiento a la carga, el amortiguador a la resistencia y la compliancia mecánica a la capacitancia. La conversión requiere revisar las conexiones, porque la topología mecánica no necesariamente se conserva en el circuito eléctrico.

### Diagrama biológico del sistema musculoesquelético

<img width="720" height="504" alt="image" src="https://github.com/user-attachments/assets/1bf5bc10-f671-4bc6-b97b-033ee435c4bb" />


**Figura 1.** Representación biológica del modelo musculoesquelético. El elemento contráctil genera la fuerza activa, las estructuras musculares pasivas aportan elasticidad, el tendón transmite la fuerza y el componente viscoso representa la resistencia a la deformación. Se conserva la autoría indicada en la imagen original.

| Elemento del modelo | Interpretación biológica | Función en el modelo |
| --- | --- | --- |
| $F_0(t)$ | Elemento contráctil muscular | Representa la contribución activa del músculo |
| $R$ | Comportamiento viscoso del tejido muscular | Relaciona fuerza viscosa y velocidad relativa |
| $C_p$ | Estructuras elásticas musculares pasivas | Representa la compliancia del elemento paralelo |
| $C_s$ | Elemento elástico en serie, asociado al tendón | Representa la deformación del componente en serie |
| $x(t)$ | Desplazamiento total | Describe la deformación global del sistema |
| $x_1(t)$ | Desplazamiento del elemento en serie | Describe la deformación asociada a $C_s$ |
| $x(t)-x_1(t)$ | Deformación de la rama muscular | Determina la velocidad relativa del amortiguador |

Las compliancias son inversas de las rigideces: $k_p=1/C_p$ y $k_s=1/C_s$. Una compliancia mayor representa una mayor deformación ante la misma fuerza. Los valores en microfaradios pertenecen al circuito eléctrico equivalente; su conversión a unidades mecánicas requiere definir la escala de la analogía.

### Analogía entre el sistema mecánico y el eléctrico

| Sistema mecánico | Sistema eléctrico equivalente |
|---|---|
| Fuerza F(t) | Voltaje V(t) |
| Velocidad dx(t)/dt | Corriente I(t) |
| Desplazamiento x(t) | Carga eléctrica q(t) |
| Amortiguador | Resistencia R |
| Compliancia mecánica | Capacitancia C |
| Masa | Inductancia L |

En esta analogía, las conexiones en serie y en paralelo del modelo mecánico pueden intercambiarse al construir el circuito equivalente, por lo que es importante identificar los desplazamientos y las referencias antes de conectarlo.

### Parámetros del circuito equivalente

| Parámetro | Control (sano) | Caso (atrofia muscular) |
|---|---:|---:|
| F(t) | 1 V | 1 V |
| α | 0.25 | 0.25 |
| Cs | 10 µF | 10 µF |
| Cp | 100 µF | 100 µF |
| R | 100 Ω | 10 kΩ |

La diferencia entre ambos modelos se introduce modificando únicamente R. El individuo sano se utiliza como referencia y la señal de error para el control se define como:

$$
e(t)=x_{control}(t)-x_{caso}(t)
$$

### Análisis matemático del circuito por superposición

Para el análisis se consideran las contribuciones de las dos fuentes por separado y después se suman. Se supone un modelo lineal, parámetros constantes y condiciones iniciales nulas. Se escribe la corriente como $\dot{x}(t)$ porque en la analogía fuerza–voltaje corresponde a la velocidad, mientras que $x(t)$ representa desplazamiento. Las fuentes se consideran por separado como entradas auxiliares; la relación $F_0=\alpha F$ se aplica al combinar sus contribuciones. Se utiliza Fs(s) para la salida de fuerza del circuito equivalente. Este desarrollo corresponde a una transferencia de fuerza; si se requiere específicamente la transferencia de desplazamiento X(s)/F(s), debe obtenerse de la variable de desplazamiento del circuito.

#### Circuito 1: apagando $F_0(t)$

Al apagar una fuente ideal de voltaje, se sustituye por un cortocircuito. La corriente total es la suma de las corrientes de ambas ramas:

$$
\dot{x}(t)=\dot{x}_1(t)+\dot{x}_2(t).
$$

Las relaciones para los elementos son:

$$
\dot{x}(t)=C_p\frac{dF_{s1}(t)}{dt},
$$

$$
\dot{x}_1(t)=C_s\frac{d[F(t)-F_{s1}(t)]}{dt},
$$

$$
\dot{x}_2(t)=\frac{F(t)-F_{s1}(t)}{R}.
$$

Sustituyendo:

$$
C_p\frac{dF_{s1}(t)}{dt}
=C_s\frac{d[F(t)-F_{s1}(t)]}{dt}
+\frac{F(t)-F_{s1}(t)}{R}.
$$

Aplicando la transformada de Laplace:

$$
C_psF_{s1}(s)=C_ss[F(s)-F_{s1}(s)]+\frac{F(s)-F_{s1}(s)}{R}.
$$

Desarrollando los términos:

$$
C_psF_{s1}(s)=C_ssF(s)-C_ssF_{s1}(s)+\frac{F(s)}{R}-\frac{F_{s1}(s)}{R}.
$$

Agrupando:

$$
\left(C_ps+C_ss+\frac{1}{R}\right)F_{s1}(s)
=\left(C_ss+\frac{1}{R}\right)F(s).
$$

$$
\frac{R(C_p+C_s)s+1}{R}F_{s1}(s)
=\frac{C_sRs+1}{R}F(s).
$$

Por lo tanto:

$$
F_{s1}(s)=\frac{\frac{C_sRs+1}{R}}{\frac{R(C_p+C_s)s+1}{R}}F(s)
=\frac{C_sRs+1}{R(C_p+C_s)s+1}F(s).
$$

#### Circuito 2: apagando $F(t)$

Se apaga la fuente $F(t)$ y se conserva la contribución de $F_0(t)$. La relación $F_0(t)=\alpha F(t)$ se utiliza al combinar ambas contribuciones. En este circuito las capacitancias equivalentes se suman:

$$
C_{eq}=C_p+C_s.
$$

La ecuación de la malla es:

$$
-\alpha F(t)=R\dot{x}(t)+\frac{1}{C_p+C_s}\int\dot{x}(t)\,dt.
$$

La salida correspondiente es:

$$
F_{s2}(t)=\frac{1}{C_p+C_s}\int\dot{x}(t)\,dt.
$$

Denotando por $V_x(s)=\mathcal{L}\{\dot{x}(t)\}$ la corriente de la analogía eléctrica:

$$
-\alpha F(s)=RV_x(s)+\frac{V_x(s)}{(C_p+C_s)s},
$$

$$
F_{s2}(s)=\frac{V_x(s)}{(C_p+C_s)s}.
$$

Despejando la entrada:

$$
F(s)=-\frac{V_x(s)}{\alpha}\frac{R(C_p+C_s)s+1}{(C_p+C_s)s}.
$$

Al dividir la salida entre la entrada:

$$
\frac{F_{s2}(s)}{F(s)}=
\frac{\frac{V_x(s)}{(C_p+C_s)s}}
{-\frac{V_x(s)}{\alpha}\frac{R(C_p+C_s)s+1}{(C_p+C_s)s}}
=-\frac{\alpha}{R(C_p+C_s)s+1}.
$$

Entonces:

$$
F_{s2}(s)=-\frac{\alpha F(s)}{R(C_p+C_s)s+1}.
$$

#### Función de transferencia por superposición

La salida total corresponde a la suma de ambas contribuciones:

$$
F_s(s)=F_{s1}(s)+F_{s2}(s).
$$

$$
F_s(s)=\frac{C_sRs+1}{R(C_p+C_s)s+1}F(s)
-\frac{\alpha}{R(C_p+C_s)s+1}F(s).
$$

$$
F_s(s)=\frac{C_sRs+1-\alpha}{R(C_p+C_s)s+1}F(s).
$$

La función de transferencia obtenida en el análisis es:

$$
\boxed{G(s)=\frac{F_s(s)}{F(s)}=\frac{C_sRs+1-\alpha}{R(C_p+C_s)s+1}}.
$$


**4. Sustitución de parámetros**

Para el individuo sano, R = 100 Ω:

$$
\boxed{H_{control}(s)=\frac{0.001s+0.75}{0.011s+1}}
$$

Para el caso de atrofia muscular, R = 10 000 Ω:

$$
\boxed{H_{caso}(s)=\frac{0.1s+0.75}{1.1s+1}}
$$

**5. Análisis de estabilidad y estado estacionario**

Las funciones de transferencia obtenidas son de primer orden, con un polo real cada una:

$$
p_{control}=-\frac{1}{0.011}\approx-90.91
$$

$$
p_{caso}=-\frac{1}{1.1}\approx-0.9091
$$

Ambos polos son negativos, por lo que los dos modelos de transferencia de fuerza son estables. El caso presenta una dinámica más lenta debido al incremento de R. Las constantes de tiempo correspondientes son 0.011 s para el control y 1.1 s para el caso.

Ante una entrada escalón unitario, el valor final de ambas salidas de fuerza es:

$$
F_s(\infty)=H(0)=1-\alpha=0.75
$$

Por lo tanto, el error estacionario entre ambas respuestas de fuerza es cero, aunque sus respuestas transitorias son diferentes. Si el error se define entre la entrada unitaria y la salida de fuerza, es 1 − 0.75 = 0.25. Estos resultados no sustituyen el análisis de error del desplazamiento solicitado para el sistema de control.

### Ecuación diferencial y representación de estado

A partir de la función de transferencia de fuerza:

$$
[R(C_p+C_s)s+1]F_s(s)=[RC_s s+(1-\alpha)]F(s).
$$

Aplicando la transformada inversa de Laplace:

$$
R(C_p+C_s)\dot{F}_s(t)+F_s(t)=RC_s\dot{F}(t)+(1-\alpha)F(t).
$$

Despejando la derivada de la salida:

$$
\dot{F}_s(t)=-\frac{F_s(t)}{R(C_p+C_s)}
+\frac{C_s}{C_p+C_s}\dot{F}(t)
+\frac{1-\alpha}{R(C_p+C_s)}F(t).
$$

Para evitar derivar numéricamente una entrada con saltos se define:

$$
\tau=R(C_p+C_s),\qquad d=\frac{C_s}{C_p+C_s},\qquad k=1-\alpha.
$$

Entonces:

$$
H(s)=d+\frac{k-d}{\tau s+1}.
$$

Una realización con un estado auxiliar $z(t)$ es:

$$
\dot{z}(t)=-\frac{1}{\tau}z(t)+\frac{1}{\tau}F(t),
\qquad F_s(t)=(k-d)z(t)+dF(t).
$$

| Matriz del modelo de estado | Expresión |
| --- | --- |
| $A$ | $-1/\tau$ |
| $B$ | $1/\tau$ |
| $C$ | $k-d$ |
| $D$ | $d$ |

El término $D$ es la transmisión directa de la entrada a la salida. Para ambos modelos, $d=1/11$ y $k-d=29/44$.

### Relación con la salida de desplazamiento

La relación del capacitor paralelo es:

$$
\dot{x}(t)=C_p\dot{F}_s(t).
$$

Integrando:

$$
x(t)-x(0)=C_p[F_s(t)-F_s(0)].
$$

Con condiciones iniciales nulas y la escala de la analogía utilizada:

$$
X(s)=C_pF_s(s).
$$

Por lo tanto, la transferencia de desplazamiento es:

$$
\boxed{G_x(s)=\frac{X(s)}{F(s)}
=C_p\frac{RC_s s+1-\alpha}{R(C_p+C_s)s+1}}.
$$

Al sustituir los valores del circuito equivalente:

$$
G_{x,control}(s)=\frac{10^{-7}s+7.5\times10^{-5}}{0.011s+1},
$$

$$
G_{x,caso}(s)=\frac{10^{-5}s+7.5\times10^{-5}}{1.1s+1}.
$$

Estos valores representan la variable equivalente de desplazamiento; no se interpretan directamente como metros sin establecer la escala mecánica. Ambas transferencias conservan los polos de las transferencias de fuerza. Para un escalón unitario, el desplazamiento equivalente final es $7.5\times10^{-5}$ en ambos modelos y la diferencia estacionaria control–caso es cero.

### Respuesta analítica al escalón, al impulso y al pulso

Para un escalón unitario y condiciones iniciales nulas:

$$
F_s(t)=k+(d-k)e^{-t/\tau},\qquad t\geq0.
$$

Sustituyendo:

$$
F_{s,control}(t)=\frac34-\frac{29}{44}e^{-t/0.011},
$$

$$
F_{s,caso}(t)=\frac34-\frac{29}{44}e^{-t/1.1}.
$$

La diferencia entre ambas salidas es:

$$
e_F(t)=\frac{29}{44}\left(e^{-t/1.1}-e^{-t/0.011}\right).
$$

Ambas salidas comienzan en $1/11$ inmediatamente después del escalón y convergen a $3/4$. El caso tarda más en alcanzar ese valor. La constante de tiempo aumenta por un factor de 100 al incrementar $R$ de 100 Ω a 10 kΩ.

La respuesta ideal al impulso se expresa como:

$$
h(t)=d\delta(t)+\frac{k-d}{\tau}e^{-t/\tau}u(t).
$$

La presencia de $d\delta(t)$ se debe a la transmisión directa. Un impulso ideal no equivale a un pulso rectangular de amplitud 1 y duración 1 s. Para aproximar un impulso unitario mediante un pulso de anchura $\Delta t$, su amplitud debe ser $1/\Delta t$ para que su área sea uno.

Para una entrada rectangular $F(t)=A[u(t-t_a)-u(t-t_b)]$, definiendo la respuesta al escalón $S(t)=[k+(d-k)e^{-t/\tau}]u(t)$:

$$
F_s(t)=A[S(t-t_a)-S(t-t_b)].
$$

Si el código utiliza un pulso entre 1 y 2 segundos, las gráficas deben identificarse como respuestas al pulso. Para comparar plataformas se debe utilizar exactamente la misma entrada, amplitud, duración, muestreo y convención de extremos.

### Sustitución de parámetros y ceros del sistema

Para el control:

$$
RC_s=100(10\times10^{-6})=0.001,
$$

$$
R(C_p+C_s)=100(100\times10^{-6}+10\times10^{-6})=0.011.
$$

Para el caso:

$$
RC_s=10\,000(10\times10^{-6})=0.1,
$$

$$
R(C_p+C_s)=10\,000(110\times10^{-6})=1.1.
$$

El cero se obtiene de $RC_s s+1-\alpha=0$:

$$
s_z=-\frac{1-\alpha}{RC_s}.
$$

| Propiedad | Control | Caso |
| --- | ---: | ---: |
| Polo | $-90.9091$ s⁻¹ | $-0.9091$ s⁻¹ |
| Cero | $-750$ s⁻¹ | $-7.5$ s⁻¹ |
| Constante de tiempo | 0.011 s | 1.1 s |
| Ganancia estática de fuerza | 0.75 | 0.75 |
| Transmisión directa de fuerza | $1/11$ | $1/11$ |

### Aplicación del teorema del valor final

Para una entrada escalón unitario $F(s)=1/s$:

$$
\lim_{t\to\infty}F_s(t)=\lim_{s\to0}sH(s)\frac1s
=H(0)=1-\alpha=0.75.
$$

Se puede aplicar el teorema porque los polos de $sF_s(s)=H(s)$ están en el semiplano izquierdo. Para la diferencia entre control y caso:

$$
e_F(\infty)=H_{control}(0)-H_{caso}(0)=0.
$$

La modificación de $R$ cambia la velocidad de respuesta, aunque no modifica la ganancia estática de este modelo. Este resultado corresponde al escalón y debe distinguirse de la respuesta al pulso, cuya salida regresa a cero después de que termina la excitación.

### Simulaciones y resultados

La respuesta al impulso unitario se estudia de 0 a 10 segundos. En MATLAB/Simulink se comparan las ecuaciones del sistema y las funciones de transferencia; en Simscape Electrical se verifica el circuito equivalente. Las mismas condiciones se reproducen en Python para comprobar los resultados.

| Condición | Respuesta a comparar | Herramientas |
|---|---|---|
| Individuo sano (control) | Respuesta al impulso en lazo abierto | MATLAB, Simulink, Simscape y Python |
| Atrofia muscular (caso) | Respuesta al impulso en lazo abierto | MATLAB, Simulink, Simscape y Python |
| Caso con controlador | Seguimiento de la respuesta del control | MATLAB/Simulink y Python |

### Diseño del controlador PID

El controlador recibe la diferencia entre la referencia y la salida realimentada del caso. Si la salida elegida es el desplazamiento:

$$
e(t)=x_{control}(t)-x_{PID}(t).
$$

El caso sin controlador se conserva como comparación de lazo abierto. La acción de control ideal es:

$$
u_c(t)=k_Pe(t)+k_I\int_0^t e(\lambda)\,d\lambda+k_D\frac{de(t)}{dt}.
$$

En Laplace:

$$
C_{PID}(s)=k_P+\frac{k_I}{s}+k_Ds.
$$

Para una estructura estándar de realimentación unitaria y una planta $P(s)$ correspondiente al caso:

$$
T(s)=\frac{C_{PID}(s)P(s)}{1+C_{PID}(s)P(s)},\qquad
\frac{E(s)}{X_{control}(s)}=\frac{1}{1+C_{PID}(s)P(s)}.
$$

Si se controla fuerza, se utiliza $P(s)=H_{caso}(s)$; si se controla desplazamiento, se utiliza $P(s)=G_{x,caso}(s)$. La variable de referencia, la salida y las ganancias deben corresponder a la misma elección. Las ganancias se obtienen con la herramienta Tune y se registran junto con la estructura y el filtro derivativo utilizado en Simulink.

Con $P(s)=(bs+k)/(\tau s+1)$ y PID ideal, la ecuación característica de esta estructura es:

$$
s(\tau s+1)+(k_Ds^2+k_Ps+k_I)(bs+k)=0.
$$

Desarrollando:

$$
bk_Ds^3+(\tau+kk_D+bk_P)s^2
+(1+kk_P+bk_I)s+kk_I=0.
$$

Para la transferencia de fuerza del caso: $b=0.1$, $k=0.75$ y $\tau=1.1$. La estabilidad del lazo cerrado requiere verificar las raíces del polinomio después de seleccionar las ganancias. Si Simulink incorpora un filtro derivativo, la ecuación característica debe calcularse con esa función de controlador. No se asignan ganancias porque no se proporcionaron los resultados de la sintonización.

### Procedimiento de implementación y verificación

1. Introducir los parámetros del control y del caso en MATLAB.
2. Construir las funciones de transferencia con numeradores de dos coeficientes: `[R*Cs, 1-alpha]` y denominadores `[R*(Cp+Cs), 1]`.
3. Implementar la ecuación diferencial o la realización de estado y comprobar que coincide con la función de transferencia.
4. Construir el circuito equivalente en Simscape Electrical y seleccionar la medición correspondiente a la variable de salida.
5. Configurar condiciones iniciales nulas y un intervalo de simulación de 0 a 10 s. Para el código de la práctica puede utilizarse un paso de muestreo de 0.001 s; el solver y sus tolerancias deben registrarse.
6. Aplicar la misma entrada en todas las representaciones. Distinguir entre impulso ideal y pulso rectangular.
7. Incorporar el PID al caso, ajustar sus ganancias y conservar la respuesta de control como referencia.
8. Reproducir la estructura y los parámetros en Python. Para la transferencia de fuerza, el numerador debe ser `[R*Cs, 1-alpha]`; sumar ambos términos en un único coeficiente eliminaría el término en $s$ y modificaría el modelo.
9. Exportar las figuras, identificando tiempo, variable de salida, unidades equivalentes y condición simulada.

Los datos proporcionados permiten calcular las funciones de transferencia, polos, ceros y respuestas analíticas anteriores. La concordancia numérica entre plataformas y el desempeño del PID deben establecerse con las gráficas y ganancias obtenidas al ejecutar los archivos.

### Discusión

El modelo muestra que un cambio en la componente viscosa puede modificar significativamente la rapidez de la respuesta sin cambiar su valor final ante una entrada constante. El incremento de $R$ desplaza el polo hacia el origen y aumenta la constante de tiempo. Esto permite distinguir una diferencia transitoria de una diferencia estacionaria.

La elección de la salida también cambia la interpretación del resultado. La transferencia de fuerza del análisis manuscrito y la transferencia de desplazamiento se relacionan mediante $C_p$ bajo las condiciones iniciales y la escala de la analogía establecidas. Esta relación debe mantenerse en MATLAB, Simulink, Simscape y Python para que la comparación sea consistente.

La respuesta al impulso incluye una componente ideal instantánea debido a la transmisión directa. Por ello, la forma de implementar la entrada puede explicar diferencias entre gráficas. También deben revisarse el paso de muestreo, la interpolación de señales, las condiciones iniciales y el filtro del controlador.

### Conclusiones

El análisis por superposición permite obtener una función de transferencia de primer orden con transmisión directa. Las contribuciones de la fuerza aplicada y de la fuerza contráctil se combinan para describir la salida del modelo.

Los modelos control y caso son estables en lazo abierto. Al incrementar la resistencia equivalente de 100 Ω a 10 kΩ, la constante de tiempo pasa de 0.011 s a 1.1 s. En ambos casos, la ganancia estática de fuerza es 0.75.

El controlador PID se plantea para reducir la diferencia dinámica entre la referencia del individuo sano y la respuesta del caso. Su eficacia debe evaluarse después de sintonizarlo y verificar la estabilidad del lazo cerrado. El modelo constituye una simplificación académica y no una validación de un tratamiento clínico.

**Palabras clave:** Sistema musculoesquelético; Atrofia muscular; Analogía fuerza–voltaje; Superposición; Función de transferencia; Controlador PID; MATLAB; Python.

## Lista de archivos incluidos en el repositorio

1. Cuaderno computacional de MATLAB [.mlx] y su exportación [.pdf].
2. Modelos de Simulink [.slx].
3. Código fuente de Python [.py].
4. Análisis matemático: ecuaciones, superposición, funciones de transferencia, estabilidad y error estacionario.
5. Gráficas de lazo abierto y lazo cerrado del control, caso y sistema con PID [.pdf].
6. Diagrama biológico del sistema musculoesquelético.
7. Archivo README.md y archivo CITATION.cff con los datos del repositorio.

## Referencias

[1] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. https://biomath.xyz/course/

[2] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.

[3] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

[4] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

[5] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

[6] N. S. Nise, *Control Systems Engineering*, 8th ed., John Wiley & Sons, 2020.
