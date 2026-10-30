# Práctica 3 — Modelado y control del sistema musculoesquelético

Repositorio académico de **Modelado de Sistemas Fisiológicos**, Ingeniería Biomédica, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana. Se analiza un modelo mecánico viscoelástico del músculo, se construye su analogía eléctrica fuerza–voltaje y se comparan las respuestas de un **individuo sano (control)** y un **caso simulado de atrofia muscular**. Se estudia además un controlador PID mediante simulación *in silico*.

**Estudiante:** Nombre completo · Número de control · Correo institucional  
**Docente:** Dr. Paul Antonio Valle Trujillo · paul.valle@tectijuana.edu.mx  
**Departamento:** Ingeniería Eléctrica y Electrónica  
**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana  
**Sede:** Blvd. Alberto Limón Padilla s/n, Tijuana, C. P. 22454, B. C., México.

---

## Contenido

1. [Resumen](#resumen)
2. [Competencia y objetivos](#competencia-y-objetivos)
3. [Descripción del modelo mecánico](#descripción-del-modelo-mecánico)
4. [Analogía electromecánica](#analogía-electromecánica)
5. [Parámetros: control y caso](#parámetros-control-y-caso)
6. [Análisis matemático por superposición](#análisis-matemático-por-superposición)
7. [Función de transferencia y alcance de la salida](#función-de-transferencia-y-alcance-de-la-salida)
8. [Análisis de estabilidad](#análisis-de-estabilidad)
9. [Análisis de estado estacionario](#análisis-de-estado-estacionario)
10. [Simulación en MATLAB, Simulink y Simscape](#simulación-en-matlab-simulink-y-simscape)
11. [Diseño del controlador PID](#diseño-del-controlador-pid)
12. [Implementación en Python](#implementación-en-python)
13. [Resultados y verificación cruzada](#resultados-y-verificación-cruzada)
14. [Diagrama biológico](#diagrama-biológico)
15. [Discusión](#discusión)
16. [Conclusiones](#conclusiones)
17. [Estructura del repositorio](#estructura-del-repositorio)
18. [Requisitos y reproducción](#requisitos-y-reproducción)
19. [Referencias](#referencias)

---

## Resumen

La práctica estudia la respuesta dinámica de un sistema musculoesquelético simplificado, formado por un elemento contráctil activo, un amortiguador viscoso y dos elementos elásticos. El modelo permite describir la relación entre fuerza y deformación y analizar el efecto de modificar el parámetro de amortiguamiento. Se emplea la analogía fuerza–voltaje para representar el sistema como un circuito eléctrico equivalente. A partir de las relaciones constitutivas y el principio de superposición se desarrolla una función de transferencia, que después se utiliza para comparar un control sano con un caso de atrofia muscular simulado.

Las implementaciones solicitadas incluyen MATLAB/Simulink, Simscape Electrical y Python. La respuesta al impulso se estudia durante **0 a 10 segundos**. Finalmente, se plantea un controlador PID para acercar la respuesta del caso a la referencia sana y se comparan los resultados entre plataformas. El trabajo es exclusivamente académico: no representa un tratamiento médico.

**Palabras clave:** sistema musculoesquelético; analogía fuerza–voltaje; viscoelasticidad; superposición; función de transferencia; PID; Simulink; Simscape; Python.

## Competencia y objetivos

**Competencia.** Modelar sistemas fisiológicos mediante analogías físicas, ecuaciones diferenciales y funciones de transferencia, y analizar estrategias de control clásico aplicadas a un modelo musculoesquelético.

**Objetivo general.** Formular, simular y comparar las respuestas del modelo musculoesquelético para las condiciones de control sano y atrofia muscular simulada, y estudiar un controlador PID que reduzca su diferencia dinámica.

**Objetivos específicos:**

1. Identificar fuerzas, desplazamientos y elementos viscosos y elásticos del modelo.
2. Construir la analogía electromecánica fuerza–voltaje y reconocer la topología del circuito equivalente.
3. Desarrollar las ecuaciones y la función de transferencia mediante superposición.
4. Evaluar polos, estabilidad y respuesta en estado estacionario.
5. Comparar la respuesta al impulso de los dos conjuntos de parámetros.
6. Implementar el sistema en Simulink, Simscape Electrical y Python.
7. Sintonizar un PID y comparar lazo abierto con lazo cerrado.
8. Documentar resultados, gráficas, diagrama biológico y archivos en GitHub.

## Descripción del modelo mecánico

El diagrama del instructivo (Figura 5.3, página 87) representa un elemento contráctil que genera **F₀(t)**, una fuerza transmitida **F(t)**, un amortiguador **R**, un elemento elástico paralelo **Cₚ** y uno en serie **Cₛ**. Los desplazamientos son **x(t)** y **x₁(t)**; el desplazamiento relativo del amortiguador es **x(t) − x₁(t)**. La fuente activa cumple:

\[
F_0(t)=\alpha F(t),\qquad 0<\alpha<1.
\]

**Interpretación de las variables:**

| Variable | Significado |
|---|---|
| F(t) | Fuerza transmitida por el sistema |
| F₀(t) | Fuerza del elemento contráctil activo |
| α | Proporción de fuerza activa |
| R | Amortiguamiento viscoso equivalente |
| Cₚ | Compliancia del elemento elástico paralelo |
| Cₛ | Compliancia del elemento elástico en serie |
| x(t) | Desplazamiento total |
| x₁(t) | Desplazamiento del elemento en serie |

El modelo es una simplificación: no describe todas las propiedades anatómicas y fisiológicas del músculo real.

## Analogía electromecánica

Se utiliza la analogía **fuerza–voltaje** indicada por el profesor:

| Sistema mecánico | Circuito eléctrico |
|---|---|
| Fuerza F(t) | Voltaje V(t) |
| Velocidad dx/dt | Corriente I(t) |
| Desplazamiento x(t) | Carga eléctrica q(t) |
| Amortiguador viscoso | Resistencia R |
| Compliancia mecánica | Capacitancia C |
| Masa, si estuviera presente | Inductancia L |

La equivalencia de variables no implica conservar las conexiones originales: una asociación mecánica en paralelo puede corresponder a una conexión eléctrica en serie, y viceversa. Los nodos y las referencias de desplazamiento deben establecerse antes de dibujar el circuito. El archivo `analisis.pdf`, página 1, contiene el croquis del modelo, los circuitos equivalentes y su separación en dos circuitos para aplicar superposición.

## Parámetros: control y caso

Los valores del instructivo, página 89, son:

| Parámetro | Control sano | Caso: atrofia simulada |
|---|---:|---:|
| F(t) | 1 V | 1 V |
| α | 0.25 | 0.25 |
| Cₛ | 10 μF | 10 μF |
| Cₚ | 100 μF | 100 μF |
| R | 100 Ω | 10 kΩ |

Se conserva la misma excitación y las mismas capacitancias y se modifica únicamente **R**, que aumenta de 100 Ω a 10 kΩ. Así, la comparación aísla el efecto del cambio de amortiguamiento equivalente en el modelo propuesto.

## Análisis matemático por superposición

Esta sección transcribe y organiza el procedimiento de las **dos páginas de `analisis.pdf`**. Para distinguir la salida usada en los apuntes, se emplea **Fₛ(t)** como fuerza equivalente de salida y **Fₛ₁(t)**, **Fₛ₂(t)** como contribuciones. Las relaciones se muestran con las convenciones algebraicas de esos apuntes.

### Circuito 1: apagando F₀(t)

Se considera únicamente la excitación F(t). El análisis manuscrito parte de la suma de dos componentes de desplazamiento:

\[
x(t)=x_1(t)+x_2(t).
\]

Para las ramas equivalentes se escriben:

\[
x(t)=C_p\frac{dF_{s1}(t)}{dt},
\]

\[
x_1(t)=C_s\frac{d[F(t)-F_{s1}(t)]}{dt},
\]

\[
x_2(t)=\frac{F(t)-F_{s1}(t)}{R}.
\]

Sustituyendo en la ecuación inicial:

\[
C_p\frac{dF_{s1}(t)}{dt}
=C_s\frac{d[F(t)-F_{s1}(t)]}{dt}
+\frac{F(t)-F_{s1}(t)}{R}.
\]

Con condiciones iniciales nulas, la transformada de Laplace produce:

\[
C_p sF_{s1}(s)=C_s sF(s)-C_s sF_{s1}(s)
+\frac{F(s)-F_{s1}(s)}{R}.
\]

Agrupando los términos de salida:

\[
\left[(C_p+C_s)s+\frac{1}{R}\right]F_{s1}(s)
=\left(C_s s+\frac{1}{R}\right)F(s).
\]

Multiplicando por R y despejando:

\[
\boxed{\frac{F_{s1}(s)}{F(s)}=
\frac{RC_s s+1}{R(C_p+C_s)s+1}}.
\]

### Circuito 2: apagando F(t)

Se conserva la contribución de la fuente dependiente **F₀(t)=αF(t)**, siguiendo el segundo circuito de los apuntes (página 2). En la notación manuscrita, el balance de la rama queda:

\[
-\alpha F(t)=R x(t)+\frac{1}{C_p+C_s}\int x(t)\,dt.
\]

Aplicando Laplace con condiciones iniciales nulas:

\[
-\alpha F(s)=R X(s)+\frac{X(s)}{(C_p+C_s)s}.
\]

De aquí:

\[
X(s)=-\alpha F(s)\frac{(C_p+C_s)s}{R(C_p+C_s)s+1}.
\]

En los apuntes se utiliza la relación entre desplazamiento y fuerza de salida de la rama:

\[
F_{s2}(s)=\frac{X(s)}{(C_p+C_s)s}.
\]

Por tanto:

\[
\boxed{\frac{F_{s2}(s)}{F(s)}=
-\frac{\alpha}{R(C_p+C_s)s+1}}.
\]

### Suma de las contribuciones

Por superposición:

\[
F_s(s)=F_{s1}(s)+F_{s2}(s).
\]

Sustituyendo ambas expresiones:

\[
F_s(s)=\left[\frac{RC_s s+1}{R(C_p+C_s)s+1}
-\frac{\alpha}{R(C_p+C_s)s+1}\right]F(s).
\]

Se obtiene la expresión final escrita en la segunda hoja del análisis:

\[
\boxed{H(s)=\frac{F_s(s)}{F(s)}=
\frac{RC_s s+1-\alpha}{R(C_p+C_s)s+1}}.
\]

**Importante:** el resultado manuscrito es una relación **fuerza de salida/fuerza de entrada**, no la relación **desplazamiento/fuerza** que el instructivo también solicita cuando se selecciona x(t) como salida. No se deben identificar ambas funciones sin una derivación adicional. El análisis manuscrito tampoco aporta mediciones o ganancias PID.

## Función de transferencia y alcance de la salida

Para el análisis algebraico disponible, la función es de **primer orden**:

\[
H(s)=\frac{RC_s s+1-\alpha}{R(C_p+C_s)s+1}.
\]

Sustituyendo los parámetros del instructivo en unidades eléctricas:

**Control sano (R = 100 Ω):**

\[
\boxed{H_{control}(s)=\frac{0.001s+0.75}{0.011s+1}}.
\]

**Caso de atrofia simulada (R = 10 000 Ω):**

\[
\boxed{H_{caso}(s)=\frac{0.1s+0.75}{1.1s+1}}.
\]

La función solicitada en el instructivo para salida de desplazamiento se expresa como **G(s)=X(s)/F(s)**. Para obtenerla rigurosamente se requiere fijar qué desplazamiento se mide y cómo se relaciona con la salida de fuerza de los circuitos del análisis. Por ello, las funciones numéricas anteriores corresponden exclusivamente a **H(s)=Fₛ(s)/F(s)**, no se presentan como si fueran G(s).

## Análisis de estabilidad

El polo de la función de transferencia desarrollada es:

\[
p=-\frac{1}{R(C_p+C_s)}.
\]

| Modelo | Denominador | Polo (s⁻¹) | Constante de tiempo |
|---|---|---:|---:|
| Control | 0.011s + 1 | −90.9091 | 0.011 s |
| Caso | 1.1s + 1 | −0.9091 | 1.1 s |

Ambos polos son reales y negativos: **ambas funciones son estables**. El polo del caso está más próximo al origen, por lo que su respuesta asociada a esta función de transferencia decae más lentamente. La constante de tiempo del caso es 100 veces la del control. Este resultado se deduce del modelo matemático y debe comprobarse con las simulaciones.

## Análisis de estado estacionario

Para una entrada escalón unitario y suponiendo estabilidad, el teorema del valor final proporciona:

\[
\lim_{t\to\infty}F_s(t)=\lim_{s\to0}H(s)=1-\alpha=0.75.
\]

En consecuencia, para **la salida de fuerza H(s)**, ambos modelos tienen la misma ganancia estática de 0.75. Si el error se define entre la entrada escalón unitario y esa salida, el error estacionario es **1 − 0.75 = 0.25**. En cambio, si se define como diferencia entre las salidas de control y caso ante el mismo escalón, el error estacionario entre ambas es **0**. Estos resultados no deben extrapolarse automáticamente a una salida de desplazamiento G(s) ni a la respuesta al impulso, que son análisis distintos.

## Simulación en MATLAB, Simulink y Simscape

El instructivo solicita tres representaciones para **control** y **caso**: (1) ecuaciones del sistema, (2) función de transferencia y (3) circuito equivalente en Simscape Electrical. Todas deben utilizar las mismas condiciones iniciales, parámetros y señal de entrada.

La excitación solicitada es un **impulso unitario**, con intervalo **0 ≤ t ≤ 10 s**. En cada plataforma se deben registrar las respuestas de la salida seleccionada e indicar sus unidades. Si se simula la función H(s) desarrollada en los apuntes, la gráfica corresponde a la salida de fuerza equivalente; si se simula G(s), corresponde al desplazamiento. Es importante mantener la misma definición de salida en toda comparación.

**Evidencias pendientes de incorporar:** diagrama de Simulink, circuito en Simscape, configuración de la señal, gráficas del control y caso, y exportaciones PDF del cuaderno MATLAB.

## Diseño del controlador PID

El modelo del individuo sano se utiliza como referencia. Para la salida de desplazamiento indicada en el instructivo, el error es:

\[
e(t)=x_{control}(t)-x_{caso}(t).
\]

El controlador PID ideal puede expresarse como:

\[
u(t)=k_Pe(t)+k_I\int_0^t e(\tau)\,d\tau+k_D\frac{de(t)}{dt}.
\]

Se debe incorporar el bloque **PID Controller** en Simulink y utilizar **Tune** para determinar **kP**, **kI** y **kD**. El objetivo es una respuesta estable que reduzca la diferencia entre el caso simulado y el control sano. Después se comparan las tres respuestas: **control**, **caso sin controlador** y **caso con PID**.

| Parámetro | Valor obtenido |
|---|---|
| kP | Pendiente de sintonización |
| kI | Pendiente de sintonización |
| kD | Pendiente de sintonización |

No se asignan ganancias ficticias ni se afirma un desempeño que todavía no esté respaldado por resultados.

## Implementación en Python

Se deben implementar las funciones de transferencia del control y del caso con las mismas condiciones que MATLAB/Simulink, calcular la respuesta al impulso y reproducir el sistema con las ganancias PID obtenidas en **Tune**. El código puede ejecutarse en Visual Studio Code y debe guardar figuras para comparar lazo abierto y lazo cerrado. Las librerías dependen del archivo Python efectivamente utilizado; normalmente pueden emplearse NumPy, Matplotlib y `python-control`.

## Resultados y verificación cruzada

La verificación requiere comparar cinco implementaciones: ecuaciones en Simulink, función de transferencia en Simulink, circuito equivalente en Simscape, cuaderno MATLAB y script Python. Las respuestas deben compararse con parámetros, entrada, salida y condiciones iniciales iguales.

| Resultado | Estado |
|---|---|
| Parámetros control/caso | Identificados en el instructivo |
| Análisis por superposición | Desarrollado a partir de `analisis.pdf` |
| Funciones Hcontrol y Hcaso | Calculadas |
| Polos y estabilidad de H(s) | Calculados |
| Estado estacionario de H(s) | Calculado |
| Simulaciones en MATLAB/Simulink | Agregar resultados reales |
| Simscape Electrical | Agregar circuito y resultados |
| Ganancias PID | Agregar valores de Tune |
| Gráficas de Python | Agregar resultados reales |
| Comparación entre plataformas | Completar con evidencias |

### Interpretación de los resultados analíticos

La variación de R modifica la rapidez de la respuesta, aunque no cambia la ganancia estática de la función de fuerza H(s) obtenida. En el control, el polo está más alejado del origen y el transitorio asociado es más rápido; en el caso simulado, la constante de tiempo aumenta y la respuesta decae más lentamente. Para evaluar el efecto del PID sobre el desplazamiento, deben añadirse las simulaciones y verificar que todas utilicen la función de transferencia correspondiente a esa variable.

## Diagrama biológico

El instructivo solicita un diagrama en **BioRender** o una herramienta equivalente que identifique el elemento contráctil muscular, estructuras elásticas, tendón, fuerza generada, desplazamiento y elementos viscosos y elásticos. La figura debe relacionar el componente activo con F₀(t), el amortiguamiento con R, las propiedades elásticas con Cₚ y Cₛ, y las deformaciones con x(t) y x₁(t). **Agregar aquí la figura propia** y su pie descriptivo.

## Discusión

El modelo reproduce de manera simplificada propiedades elásticas y viscosas del sistema musculoesquelético. La comparación entre control y caso está construida modificando únicamente R, lo que permite identificar su influencia sobre la dinámica. El análisis por superposición de los apuntes proporciona una función de transferencia de primer orden para una salida de fuerza equivalente, con un polo estable para ambos conjuntos de parámetros. La diferencia principal es temporal: la respuesta del caso presenta una constante de tiempo considerablemente mayor.

Una limitación importante es la **definición de la salida**. El instructivo propone analizar desplazamiento, mientras que las hojas manuscritas concluyen con una relación de fuerzas. La comparación cuantitativa de las simulaciones debe resolver esta diferencia de variables y documentar las ecuaciones y conexiones realmente implementadas. También deben revisarse diferencias por solver, discretización del impulso, condiciones iniciales y elección de parámetros del PID. Sin las gráficas y los valores sintonizados, no es posible asegurar el nivel de mejora del sistema controlado.

## Conclusiones

El modelo musculoesquelético permite estudiar el efecto de la elasticidad y el amortiguamiento sobre una respuesta dinámica mediante una representación mecánica y su analogía eléctrica. El procedimiento por superposición desarrollado en los apuntes conduce a una función de transferencia de primer orden para la fuerza equivalente, con polos negativos en las dos condiciones estudiadas. El aumento de R produce una respuesta temporal más lenta en ese modelo, sin modificar su ganancia estática. La implementación en Simulink, Simscape y Python permitirá comprobar estas predicciones y estudiar el efecto de un controlador PID; para establecer conclusiones experimentales o de simulación definitivas es necesario incorporar las gráficas y las ganancias obtenidas.

## Estructura del repositorio

Organización sugerida (ajustar a los nombres reales de los archivos):

```text
.
├── README.md
├── CITATION.cff
├── pract3.pdf                  # Instructivo de la práctica
├── analisis.pdf                # Desarrollo matemático manuscrito
├── practica3.mlx               # Cuaderno MATLAB
├── practica3.slx               # Modelo Simulink/Simscape
├── practica3.py                # Código Python
├── resultados/
│   ├── control_matlab.pdf
│   ├── caso_matlab.pdf
│   ├── control_python.pdf
│   ├── caso_python.pdf
│   └── comparacion_pid.pdf
├── figuras/
│   ├── circuito_equivalente.png
│   ├── simulink.png
│   ├── simscape.png
│   └── diagrama_biologico.png
└── evidencias/
    └── sintonia_pid.png
```

Los nombres son sugerencias organizativas, no una afirmación de que los archivos ya existen. El repositorio final debe incluir también capturas, archivos complementarios y los resultados reales.

## Requisitos y reproducción

**Software:** MATLAB, Simulink, Simscape Electrical, herramientas de sintonización PID, Python, editor de código (por ejemplo, Visual Studio Code), Git y GitHub. BioRender o equivalente para el diagrama biológico.

**Secuencia:** (1) revisar el modelo mecánico; (2) construir la analogía fuerza–voltaje; (3) desarrollar las ecuaciones y definir la salida; (4) obtener las funciones de transferencia; (5) analizar polos y estado estacionario; (6) simular el impulso en control y caso durante 10 s; (7) sintonizar el PID; (8) reproducir los resultados en Python; (9) contrastar plataformas; (10) añadir figuras, discusión y `CITATION.cff`.

## Referencias

[1] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.  
[2] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.  
[3] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.  
[4] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.  
[5] N. S. Nise, *Control Systems Engineering*, 8th ed., John Wiley & Sons, 2020.  
[6] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, TecNM / Instituto Tecnológico de Tijuana, 2026.  
[7] Tecnológico Nacional de México, *Programa de estudio de Modelado de Sistemas Fisiológicos, IBF-1020*.  
[8] *Práctica 3: Modelado y control del sistema musculoesquelético*, instructivo proporcionado, sección 5.5, pp. 87–95.  
[9] *Análisis matemático por superposición*, apuntes manuscritos proporcionados, 2 páginas.

---

**Uso académico:** las estrategias de control y los parámetros del caso representan una simulación didáctica y no una propuesta de tratamiento de atrofia muscular.
