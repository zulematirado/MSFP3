[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=zulematirado/MSFP3)

# Práctica 3: Modelado y control del sistema musculoesquelético

## Información de la estudiante

Zulema Guadalupe Tirado Manzo[23210724]; l23210724@tectijuana.edu.mx

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

Para el análisis se consideran las contribuciones de las dos fuentes por separado y después se suman. Se utiliza Fs(s) para la salida de fuerza del circuito equivalente. Este desarrollo corresponde a una transferencia de fuerza; si se requiere específicamente la transferencia de desplazamiento X(s)/F(s), debe obtenerse de la variable de desplazamiento del circuito.

**1. Primera contribución: apagando F0(t)**

La relación de las variables del circuito es:

$$
x(t)=x_1(t)+x_2(t)
$$

$$
x(t)=C_p\frac{dF_{s1}(t)}{dt}
$$

$$
x_1(t)=C_s\frac{d[F(t)-F_{s1}(t)]}{dt}
$$

$$
x_2(t)=\frac{F(t)-F_{s1}(t)}{R}
$$

Sustituyendo:

$$
C_p\frac{dF_{s1}(t)}{dt}=C_s\frac{d[F(t)-F_{s1}(t)]}{dt}+\frac{F(t)-F_{s1}(t)}{R}
$$

Aplicando la transformada de Laplace con condiciones iniciales nulas:

$$
C_p sF_{s1}(s)=C_s s[F(s)-F_{s1}(s)]+\frac{F(s)-F_{s1}(s)}{R}
$$

Agrupando términos:

$$
\left[(C_p+C_s)s+\frac{1}{R}\right]F_{s1}(s)=\left(C_s s+\frac{1}{R}\right)F(s)
$$

La primera contribución queda:

$$
\boxed{\frac{F_{s1}(s)}{F(s)}=\frac{RC_s s+1}{R(C_p+C_s)s+1}}
$$

**2. Segunda contribución: apagando F(t)**

Para la contribución de la fuente contráctil, con la convención de signos del circuito:

$$
-\alpha F(t)=R x(t)+\frac{1}{C_p+C_s}\int x(t)\,dt
$$

En el dominio de Laplace:

$$
-\alpha F(s)=\left[R+\frac{1}{(C_p+C_s)s}\right]X(s)
$$

Despejando:

$$
X(s)=\frac{-\alpha(C_p+C_s)s}{R(C_p+C_s)s+1}F(s)
$$

Como la contribución de salida es:

$$
F_{s2}(s)=\frac{X(s)}{(C_p+C_s)s}
$$

se obtiene:

$$
\boxed{\frac{F_{s2}(s)}{F(s)}=\frac{-\alpha}{R(C_p+C_s)s+1}}
$$

**3. Función de transferencia por superposición**

Sumando las dos contribuciones:

$$
F_s(s)=F_{s1}(s)+F_{s2}(s)
$$

$$
F_s(s)=\frac{RC_s s+1}{R(C_p+C_s)s+1}F(s)-\frac{\alpha}{R(C_p+C_s)s+1}F(s)
$$

Por lo tanto:

$$
\boxed{H(s)=\frac{F_s(s)}{F(s)}=\frac{RC_s s+1-\alpha}{R(C_p+C_s)s+1}}
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

**Palabras clave:** Sistema musculoesquelético; Atrofia muscular; Analogía fuerza–voltaje; Superposición; Función de transferencia; Controlador PID; MATLAB; Python.

## Lista de archivos incluidos en el repositorio

1. Cuaderno computacional de MATLAB [.mlx] y su exportación [.pdf].
2. Modelos de Simulink [.slx] y circuito equivalente en Simscape Electrical.
3. Código fuente de Python [.py].
4. Análisis matemático: ecuaciones, superposición, funciones de transferencia, estabilidad y error estacionario.
5. Gráficas de lazo abierto y lazo cerrado del control, caso y sistema con PID [.pdf y .png].
6. Capturas de la sintonización del controlador y sus ganancias kP, kI y kD.
7. Diagrama biológico del sistema musculoesquelético.
8. Capturas de las simulaciones y archivos complementarios.
9. Archivo README.md y archivo CITATION.cff con los datos del repositorio.

## Referencias

[1] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. https://biomath.xyz/course/

[2] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.

[3] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

[4] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

[5] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

[6] N. S. Nise, *Control Systems Engineering*, 8th ed., John Wiley & Sons, 2020.
