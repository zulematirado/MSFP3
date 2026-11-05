[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/)

# Práctica 3: Modelado y control del sistema musculoesquelético

## Información del estudiante
Africa Montserrat Rodriguez Garcia [C21211891]; L21211891@tijuana.tecnm.mx

Modelado de Sistemas Fisiológicos

Ingeniería Biomédica

## Docente
Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica, Tecnológico Nacional de México/IT Tijuana, Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, B.C., México.

## Descripción de la asignatura

El modelizado de sistemas fisiológicos es una herramienta importante en Ingeniería Biomédica, permite comprender el funcionamiento del cuerpo humano, así como diseñar y evaluar terapias y dispositivos médicos; se define como el proceso de formular modelos matemáticos o computacionales que representan el comportamiento y la interacción de los sistemas biológicos y fisiológicos. Esta asignatura pretende aportar al perfil del Ingeniero Biomédico la capacidad de realizar investigación científica en el área de Biología de Sistemas con la finalidad de dirigir y participar en equipos de trabajo interdisciplinarios en contextos nacionales e internacionales, así como de proporcionar soluciones informáticas para resolver problemas en el campo de la Ingeniería Biomédica con ética profesional; lo anterior al proporcionar al estudiante bases sólidas para modelizar sistemas y diseñar controladores para la solución de problemas en las áreas de atención médica y del sector industrial médico. La construcción de analogías entre circuitos eléctricos y sistemas fisiológicos para la formulación de modelos matemáticos y el diseño de controladores mediante la experimentación in silico brindan herramientas de gran aplicación en el quehacer profesional del Ingeniero Biomédico.

La asignatura de Modelado de Sistemas Fisiológicos forma parte del plan de estudios de la carrera en Ingeniería Biomédica con la siguiente competencia general del curso: Utiliza las propiedades de los circuitos RLC para describir la dinámica de sistemas fisiológicos, obtener modelos matemáticos y aplicar el control clásico, esto con el objetivo de integrar los principios de la Ingeniería de Control, la Electrónica Analógica y las Ciencias de la Computación con la Anatomía y Fisiología del cuerpo humano para proporcionar descripciones cuantitativas y cualitativas de sistemas fisiológicos complejos con el objetivo de modelizar, analizar, controlar, ilustrar y predecir su dinámica tanto en el corto como en el largo plazo.

## Objetivos

1. Identificar los elementos del modelo mecánico del sistema musculoesquelético.
2. Construir el circuito eléctrico equivalente mediante la analogía fuerza–voltaje.
3. Determinar las ecuaciones del sistema y obtener la función de transferencia mediante superposición.
4. Establecer el modelo de ecuaciones diferenciales ordinarias de primer orden.
5. Analizar el error en estado estacionario y la estabilidad de las condiciones control y caso.
6. Implementar y simular el modelo en MATLAB, Simulink y Simscape Electrical durante un intervalo de 0 a 10 segundos.
7. Sintonizar un controlador PID para reducir la diferencia entre la respuesta del caso y la del control.
8. Implementar las funciones de transferencia en Python y verificar la consistencia de las diferentes representaciones.
9. Relacionar el modelo con las estructuras musculares mediante un diagrama biológico en BioRender.

## Descripción detallada del sistema

El sistema musculoesquelético se representa mediante un modelo mecánico simplificado formado por un elemento contráctil, un amortiguador viscoso y dos elementos elásticos. El modelo permite estudiar la relación entre la fuerza aplicada, la fuerza transmitida y los desplazamientos del músculo y del tendón.

La fuerza del elemento contráctil se define como $F_0(t)=\alpha F(t)$, con $0<\alpha<1$. El parámetro $R$ representa la componente viscosa del tejido, $C_p$ la compliancia de las estructuras elásticas musculares en paralelo y $C_s$ la compliancia del elemento en serie asociado con el tendón. El desplazamiento total es $x(t)$, el desplazamiento del elemento en serie es $x_1(t)$ y la deformación del amortiguador es $x(t)-x_1(t)$.

### Diagrama biológico

![Modelo biológico del sistema musculoesquelético elaborado en BioRender](imagenes/biorender.jpeg)

Figura 1. Relación entre las estructuras musculares y los elementos del modelo mecánico. Se conserva la imagen proporcionada con sus créditos originales.

### Analogía electromecánica

| Sistema mecánico | Sistema eléctrico equivalente |
| --- | --- |
| Fuerza $F(t)$ | Voltaje $V(t)$ |
| Velocidad $\dot{x}(t)$ | Corriente $I(t)$ |
| Desplazamiento $x(t)$ | Carga eléctrica $q(t)$ |
| Amortiguador viscoso $R$ | Resistencia $R$ |
| Compliancias $C_p$ y $C_s$ | Capacitancias $C_p$ y $C_s$ |
| Masa, cuando esté presente | Inductancia $L$ |

Las conexiones mecánicas en paralelo pueden corresponder a conexiones eléctricas en serie, y las conexiones mecánicas en serie pueden corresponder a conexiones eléctricas en paralelo. Los circuitos equivalentes y los circuitos utilizados para aplicar superposición se incluyen en la primera hoja del análisis original.

### Parámetros de la práctica

| Parámetro | Individuo sano (control) | Caso simulado de atrofia muscular |
| --- | --- | --- |
| Amplitud de entrada $F(t)$ en el circuito equivalente | 1 V | 1 V |
| $\alpha$ | 0.25 | 0.25 |
| $C_s$ | 10 µF | 10 µF |
| $C_p$ | 100 µF | 100 µF |
| $R$ | 100 Ω | 10 kΩ |

El incremento de $R$ representa la modificación de la componente viscosa del caso. La respuesta del control se utiliza como referencia para diseñar el controlador del caso. La práctica se realiza mediante simulación con fines académicos.

### Análisis matemático

Se presenta el desarrollo por superposición de las hojas proporcionadas. La salida utilizada en este análisis es $F_s(t)$, representada como voltaje en la analogía eléctrica. Esta salida es distinta del desplazamiento $x(t)$ indicado como opción de salida en la guía de la práctica. Para evitar confundir desplazamiento con velocidad, las relaciones eléctricas se escriben con $\dot{x}(t)$, que corresponde a la corriente.

Se consideran condiciones iniciales nulas y:

$$
F_0(t)=\alpha F(t).
$$

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

#### Hojas originales del análisis matemático

Se incluyen completas para conservar los diagramas, los circuitos y el procedimiento manuscrito original.

![Análisis matemático original, hoja 1: modelo mecánico, circuitos equivalentes y primera contribución](imagenes/analisis_1.png)

![Análisis matemático original, hoja 2: segunda contribución y función de transferencia](imagenes/analisis_2.png)

[Consultar el análisis matemático original en PDF](analisis_matematico.pdf).

### Funciones de transferencia del control y del caso

Al sustituir los parámetros en la función obtenida:

$$
G_{control}(s)=\frac{0.001s+0.75}{0.011s+1},
$$

$$
G_{caso}(s)=\frac{0.1s+0.75}{1.1s+1}.
$$

### Modelo de ecuaciones diferenciales ordinarias

La ecuación equivalente para la salida $F_s(t)$ es:

$$
R(C_p+C_s)\frac{dF_s(t)}{dt}+F_s(t)
=RC_s\frac{dF(t)}{dt}+(1-\alpha)F(t).
$$

Para implementarla sin derivar la entrada, se puede utilizar un estado auxiliar $z(t)$:

$$
\frac{dz(t)}{dt}=-\frac{z(t)}{R(C_p+C_s)}+\frac{F(t)}{R(C_p+C_s)},
$$

$$
F_s(t)=\frac{C_s}{C_p+C_s}F(t)
+\left(1-\alpha-\frac{C_s}{C_p+C_s}\right)z(t).
$$

### Estabilidad y estado estacionario

El polo de la función de transferencia es:

$$
p=-\frac{1}{R(C_p+C_s)}.
$$

| Resultado analítico | Control | Caso |
| --- | --- | --- |
| Polo | $-90.9091$ s⁻¹ | $-0.9091$ s⁻¹ |
| Constante de tiempo | 0.011 s | 1.1 s |
| Ganancia en estado estacionario | 0.75 | 0.75 |

Ambos modelos son estables porque sus polos son negativos. El caso tiene una constante de tiempo mayor, por lo que su respuesta es más lenta. Para un escalón unitario, el valor final de $F_s(t)$ es 0.75 en ambos modelos. Por lo tanto, la diferencia final entre control y caso es cero, aunque sus respuestas transitorias son distintas. Si se compara cada salida directamente con la entrada unitaria, la diferencia final es 0.25.

### Simulación y controlador PID

Las actividades de la práctica incluyen obtener la respuesta al impulso en el intervalo de 0 a 10 segundos, comparar control y caso e incorporar un controlador PID. La señal de error se define como la diferencia entre la salida del control y la salida del caso, usando la misma variable de salida en ambas implementaciones.

$$
C_{PID}(s)=k_P+\frac{k_I}{s}+k_Ds.
$$

Las ganancias deben registrarse a partir de la herramienta Tune de Simulink. Los archivos proporcionados no contienen las ganancias sintonizadas ni las gráficas de las simulaciones; por ello, no se asignan valores ni se presentan resultados experimentales de lazo cerrado. Los valores de polos y ganancias estáticas anteriores corresponden al cálculo analítico de la función de transferencia del manuscrito.

Palabras clave: Sistema musculoesquelético; Analogía electromecánica; Superposición; Controlador PID; Modelo matemático; Simulaciones numéricas.

## Lista de archivos incluidos en el repositorio

Archivos incluidos con este README:

1. Archivo README.md con la información de la práctica y el desarrollo matemático.
2. Diagrama biológico elaborado en BioRender [imagenes/biorender.jpeg].
3. Hojas originales del análisis matemático [imagenes/analisis_1.png e imagenes/analisis_2.png].
4. Análisis matemático original [analisis_matematico.pdf].

Archivos del desarrollo computacional que deben integrarse al repositorio de la práctica:

1. Cuaderno computacional de MATLAB [.mlx].
2. Modelo de Simulink [.slx], con las representaciones por EDOs y función de transferencia.
3. Circuito eléctrico equivalente en Simscape Electrical.
4. Código fuente desarrollado en Python [.py].
5. Evidencia de las ganancias y sintonización del controlador PID.
6. Figuras de las simulaciones de MATLAB y Python [.pdf].
7. Capturas de las simulaciones correspondientes al control, caso y sistema controlado.
8. Archivo CITATION.cff con la información de la alumna y del repositorio.

## Referencias

[1] P. A. Valle, Syllabus para Modelado de Sistemas Fisiológicos, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. Apartado 5.5: Modelado y control del sistema musculoesquelético. https://biomath.xyz/course/

[2] M. C. Khoo, Physiological Control Systems: Analysis, Simulation, and Estimation, 2nd ed. IEEE Press, 2018.

[3] N. S. Nise, Control Systems Engineering, 8th ed. John Wiley & Sons, 2020.

[4] K. Ogata, Ingeniería de Control Moderna, 5a ed. Pearson Educación, 2010.
