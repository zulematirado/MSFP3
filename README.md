<div align="center">

# INFORME DE PRÁCTICA 03
## Modelado y control del sistema musculoesquelético

**Sección 5.5 del manual · Reporte de simulación**

**Modelo mecánico simplificado · Analogía electromecánica · Control PID**

**MATLAB / SIMULINK · SIMSCAPE ELECTRICAL · PYTHON · CONTROL PID**

**Tecnológico Nacional de México · Instituto Tecnológico de Tijuana**  
**Ingeniería Biomédica | Modelado de Sistemas Fisiológicos**

</div>

---

## FICHA ACADÉMICA

| Campo | Información |
|:--|:--|
| **Estudiante** | María Luisa Michelle Muñoz Meneses |
| **Número de control** | 23210714 |
| **Correo institucional** | l23210714@tectijuana.edu.mx |
| **Asignatura** | Modelado de Sistemas Fisiológicos |
| **Carrera** | Ingeniería Biomédica |
| **Docente** | Dr. Paul Antonio Valle Trujillo |
| **Institución** | Tecnológico Nacional de México / Instituto Tecnológico de Tijuana |
| **Departamento** | Ingeniería Eléctrica y Electrónica |
| **Modalidad** | Simulación *in silico* |

> **Nota:** El identificador «MSFP3 · Práctica 3» es una propuesta de organización del repositorio; en el instructivo la actividad corresponde a la **sección 5.5**.

## ÍNDICE GENERAL

1. [Resumen](#resumen)
2. [Palabras clave](#palabras-clave)
4. [Competencia y objetivos](#competencia-y-objetivos)
4. [Introducción](#introducción)
5. [Modelo mecánico](#modelo-mecánico)
6. [Analogía electromecánica](#analogía-electromecánica)
7. [Parámetros de control y caso](#parámetros-de-control-y-caso)
8. [Formulación matemática](#formulación-matemática)
9. [Función de transferencia y estabilidad](#función-de-transferencia-y-estabilidad)
10. [Condiciones de simulación](#condiciones-de-simulación)
11. [Implementación en MATLAB y Simulink](#implementación-en-matlab-y-simulink)
12. [Respuesta en lazo abierto](#respuesta-en-lazo-abierto)
13. [Controlador PID y lazo cerrado](#controlador-pid-y-lazo-cerrado)
14. [Implementación en Python](#implementación-en-python)
15. [Verificación cruzada](#verificación-cruzada)
16. [Diagrama biológico](#diagrama-biológico)
17. [Actividades y evidencias](#actividades-y-evidencias)
18. [Organización del repositorio](#organización-del-repositorio)
19. [Resultados y discusión](#resultados-y-discusión)
20. [Conclusiones](#conclusiones)
21. [Bibliografía](#bibliografía)

---

<a id="resumen"></a>

---

## 01 / RESUMEN

<p align="justify">Esta práctica analiza el comportamiento dinámico de un modelo simplificado del **sistema musculoesquelético**, formado por un elemento contráctil activo, un amortiguador viscoso y dos elementos elásticos. Se estudian dos condiciones: un individuo sano (**control**) y un caso simulado de **atrofia muscular** (**caso**). La diferencia entre ambos se representa modificando la resistencia del circuito eléctrico equivalente, manteniendo los demás parámetros constantes.</p>

<p align="justify">Se construye una analogía fuerza–voltaje, se formulan las ecuaciones del sistema y se obtienen sus funciones de transferencia. Las respuestas al impulso unitario se comparan mediante MATLAB/Simulink, Simscape Electrical y Python. Finalmente, se diseña un controlador PID para reducir la diferencia entre la respuesta del caso y la referencia sana. Todo el trabajo es **académico y exclusivamente computacional**.</p>

<a id="palabras-clave"></a>

---

## 02 / PALABRAS CLAVE

**Sistema musculoesquelético; modelo mecánico; viscoelasticidad; analogía fuerza–voltaje; control PID; Simulink; Simscape Electrical; Python; atrofia muscular.**

<a id="competencia-y-objetivos"></a>

---

## 03 / COMPETENCIA Y OBJETIVOS

### Competencia a desarrollar

<p align="justify">Modelizar sistemas fisiológicos mediante analogías entre sistemas físicos, ecuaciones diferenciales y funciones de transferencia, para analizar su dinámica y diseñar estrategias de control clásico.</p>

### Objetivo general

<p align="justify">Representar un sistema musculoesquelético simplificado mediante un modelo mecánico y su circuito eléctrico equivalente, comparar las respuestas de un individuo sano y un caso de atrofia muscular simulada y evaluar el efecto de un controlador PID sobre el seguimiento de la referencia.</p>

### Objetivos específicos

- Identificar los elementos contráctiles, elásticos y viscosos del modelo.
- Establecer la analogía electromecánica fuerza–voltaje.
- Formular las ecuaciones y obtener las funciones de transferencia de control y caso.
- Analizar estabilidad, polos y respuesta al impulso.
- Implementar y comparar modelos en Simulink y Simscape Electrical.
- Sintonizar un controlador PID mediante **Tune** y comparar el caso controlado con la referencia.
- Reproducir las respuestas en Python y documentar las evidencias en GitHub.

<a id="introducción"></a>

---

## 04 / INTRODUCCIÓN

<p align="justify">Las propiedades mecánicas de músculos y tendones pueden representarse de forma simplificada mediante elementos contráctiles, resortes y amortiguadores. Los resortes modelan la respuesta elástica ante la deformación, mientras que los amortiguadores representan la oposición viscosa al movimiento. Este enfoque permite relacionar la fuerza aplicada, los desplazamientos y las velocidades.</p>

<p align="justify">La práctica utiliza un modelo inspirado en Hill y Hoo, con un elemento activo $F_0(t)$, una resistencia viscosa $R$ y dos elementos elásticos $C_p$ y $C_s$. Para estudiar la diferencia entre el control y el caso se modifica únicamente el valor de $R$ del circuito eléctrico equivalente.</p>

<a id="modelo-mecánico"></a>

---

## 05 / MODELO MECÁNICO

### Elementos y variables

| Símbolo | Interpretación |
|:--|:--|
| $F(t)$ | Fuerza transmitida por el sistema |
| $F_0(t)$ | Fuerza del elemento contráctil activo |
| $R$ | Amortiguamiento viscoso |
| $C_p$ | Compliancia del elemento elástico paralelo |
| $C_s$ | Compliancia del elemento elástico en serie |
| $x(t)$ | Desplazamiento total |
| $x_1(t)$ | Desplazamiento del elemento elástico en serie |
| $x(t)-x_1(t)$ | Deformación asociada al amortiguador |

<p align="justify">La fuerza activa se relaciona con la fuerza aplicada mediante</p>

$$
F_0(t)=\alpha F(t),\qquad 0<\alpha<1.
$$

<p align="justify">La velocidad relativa en el amortiguador es</p>

$$
\frac{d}{dt}\left[x(t)-x_1(t)\right].
$$

> **Figura del modelo:** incorporar aquí la imagen del modelo mecánico proporcionada en la práctica, una vez guardada en `imagenes/modelo_mecanico.png`.
>
> `![Modelo mecánico musculoesquelético](imagenes/modelo_mecanico.png)`

<a id="analogía-electromecánica"></a>

---

## 06 / ANALOGÍA ELECTROMECÁNICA

<p align="justify">Se emplea la analogía **fuerza–voltaje**. En esta representación, las magnitudes mecánicas se corresponden con magnitudes eléctricas de la siguiente manera:</p>

| Sistema mecánico | Sistema eléctrico equivalente |
|:--|:--|
| Fuerza $F(t)$ | Voltaje $V(t)$ |
| Velocidad $\dot{x}(t)$ | Corriente $I(t)$ |
| Desplazamiento $x(t)$ | Carga eléctrica $q(t)$ |
| Amortiguador | Resistencia $R$ |
| Compliancia mecánica | Capacitancia $C$ |
| Masa, cuando exista | Inductancia $L$ |

**Importante:** la topología puede cambiar durante la conversión: elementos mecánicos en paralelo pueden corresponder a elementos eléctricos en serie, y viceversa. Se deben identificar primero los desplazamientos y la referencia mecánica antes de conectar el circuito en Simscape.

<a id="parámetros-de-control-y-caso"></a>

---

## 07 / PARÁMETROS DE CONTROL Y CASO

<p align="justify">Los valores suministrados para el **circuito eléctrico equivalente** son:</p>

| Parámetro | Control (sano) | Caso (atrofia simulada) |
|:--|--:|--:|
| $F(t)$, entrada equivalente | $1\,\mathrm{V}$ | $1\,\mathrm{V}$ |
| $\alpha$ | $0.25$ | $0.25$ |
| $C_s$ | $10\,\mu\mathrm{F}$ | $10\,\mu\mathrm{F}$ |
| $C_p$ | $100\,\mu\mathrm{F}$ | $100\,\mu\mathrm{F}$ |
| $R$ | $100\,\Omega$ | $10\,\mathrm{k}\Omega$ |

<p align="justify">El caso presenta una resistencia eléctrica equivalente **100 veces mayor** que el control. Esta modificación se utiliza para estudiar cómo cambia la dinámica del modelo; no representa una medición clínica de atrofia muscular.</p>

<a id="formulación-matemática"></a>

---

## 08 / FORMULACIÓN MATEMÁTICA

<p align="justify">Las relaciones constitutivas del modelo mecánico son:</p>

**Elemento viscoso:**

$$
F_R(t)=R_m\,\frac{d}{dt}\left[x(t)-x_1(t)\right].
$$

**Elementos elásticos, expresados mediante compliancias mecánicas:**

$$
F_{C_p}(t)=\frac{x(t)}{C_{p,m}},\qquad
F_{C_s}(t)=\frac{x_1(t)}{C_{s,m}}.
$$

<p align="justify">Aquí el subíndice $m$ distingue los parámetros mecánicos de los valores eléctricos en ohmios y faradios de la tabla anterior. Estas ecuaciones expresan las relaciones constitutivas; para formular el sistema completo deben combinarse con las condiciones de equilibrio y la topología exacta del diagrama.</p>

<p align="justify">En el circuito equivalente, las relaciones son:</p>

$$
V_R(t)=R\,I_R(t),\qquad
I_C(t)=C\frac{dV_C(t)}{dt}.
$$

**Pendiente de documentar:** ecuaciones completas de equilibrio, elección de estados y deducción a partir del circuito eléctrico efectivamente construido. No se asigna una ecuación global sin verificar la topología y la variable de salida.

<a id="función-de-transferencia-y-estabilidad"></a>

---

## 09 / FUNCIÓN DE TRANSFERENCIA Y ESTABILIDAD

<p align="justify">Con condiciones iniciales nulas, la práctica solicita obtener la relación entre la fuerza de entrada y el desplazamiento de salida:</p>

$$
\boxed{G(s)=\frac{X(s)}{F(s)}}.
$$

<p align="justify">Se deben determinar independientemente</p>

$$
G_{\mathrm{control}}(s),\qquad G_{\mathrm{caso}}(s),
$$

<p align="justify">utilizando los parámetros correspondientes. Posteriormente se calculan los polos de ambas funciones y se analiza su ubicación en el plano complejo. También se estudia el error en estado estacionario ante una entrada apropiada y el efecto de aumentar $R$.</p>

> **Pendiente:** incorporar las funciones de transferencia desarrolladas y sus polos cuando se haya verificado el circuito equivalente. El instructivo mostrado no proporciona sus coeficientes finales.

<a id="condiciones-de-simulación"></a>

---

## 10 / CONDICIONES DE SIMULACIÓN

| Condición | Configuración |
|:--|:--|
| Señal de prueba | Impulso unitario |
| Intervalo | $0\le t\le 10\,\mathrm{s}$ |
| Modelos | Control y caso |
| Referencia del PID | Respuesta del individuo sano |
| Comparación | Mismas condiciones iniciales y de simulación |

**Precaución de unidades:** la tabla del instructivo da $1\,\mathrm{V}$ como valor eléctrico equivalente de entrada, mientras que el análisis temporal solicita un **impulso unitario**. No se debe confundir un impulso ideal con un escalón constante de $1\,\mathrm{V}$; registrar la realización utilizada en cada herramienta.

<a id="implementación-en-matlab-y-simulink"></a>

---

## 11 / IMPLEMENTACIÓN EN MATLAB Y SIMULINK

<p align="justify">Se requieren tres representaciones para **ambas condiciones**:</p>

1. Modelo construido a partir de las ecuaciones del sistema.
2. Modelo mediante la función de transferencia.
3. Circuito eléctrico equivalente mediante **Simscape Electrical**.

<p align="justify">El cuaderno MATLAB debe ejecutar las simulaciones, recuperar las señales en el *Workspace*, representar las variables con unidades y guardar las figuras en PDF. Se debe verificar que los tres enfoques utilicen parámetros y entradas equivalentes.</p>

<a id="respuesta-en-lazo-abierto"></a>

---

## 12 / RESPUESTA EN LAZO ABIERTO

<p align="justify">Aplicar la misma entrada impulso al control y al caso, sin controlador PID, y comparar:</p>

$$
x_{\mathrm{control}}(t),\qquad x_{\mathrm{caso}}(t).
$$

<p align="justify">Analizar amplitud, velocidad de respuesta, amortiguamiento y diferencias temporales. Explicar cómo el incremento de $R$ afecta el comportamiento del sistema.</p>

**Evidencia por agregar:** `resultados/lazo_abierto_matlab.pdf`.

<a id="controlador-pid-y-lazo-cerrado"></a>

---

## 13 / CONTROLADOR PID Y LAZO CERRADO

### Error de seguimiento

<p align="justify">La respuesta del individuo sano se toma como referencia del caso:</p>

$$
\boxed{e(t)=x_{\mathrm{control}}(t)-x_{\mathrm{caso}}(t)}.
$$

### Ley de control

$$
u_c(t)=k_Pe(t)+k_I\int_0^t e(\tau)\,d\tau+k_D\frac{de(t)}{dt}.
$$

<p align="justify">En el dominio de Laplace:</p>

$$
K(s)=k_P+\frac{k_I}{s}+k_Ds.
$$

<p align="justify">La herramienta **Tune** del bloque `PID Controller` de Simulink permite obtener las ganancias del controlador para el caso. Se busca una respuesta estable y una reducción del error frente al control sano.</p>

| Ganancia | Valor obtenido |
|:--|:--|
| $k_P$ | Pendiente de sintonización |
| $k_I$ | Pendiente de sintonización |
| $k_D$ | Pendiente de sintonización |

### Comparación en lazo cerrado

<p align="justify">Se deben representar conjuntamente:</p>

$$
x_{\mathrm{control}}(t),\qquad
x_{\mathrm{caso}}(t),\qquad
x_{\mathrm{PID}}(t).
$$

<p align="justify">El análisis debe describir el efecto del PID sobre amplitud, rapidez, amortiguamiento y seguimiento. **Evidencia por agregar:** `resultados/lazo_cerrado_matlab.pdf` y `imagenes/sintonizacion_pid.png`.</p>

<a id="implementación-en-python"></a>

---

## 14 / IMPLEMENTACIÓN EN PYTHON

<p align="justify">En **Visual Studio Code** se implementarán las funciones de transferencia del control y del caso, se calcularán las respuestas al impulso con las mismas condiciones de MATLAB/Simulink y se incorporarán las ganancias $k_P$, $k_I$ y $k_D$ obtenidas con **Tune**.</p>

<p align="justify">El programa deberá generar las gráficas de lazo abierto y lazo cerrado y guardarlas para su comparación. Registrar las bibliotecas utilizadas en el archivo `.py` y asegurar que la entrada impulso se interprete de manera consistente entre programas.</p>

<a id="verificación-cruzada"></a>

---

## 15 / VERIFICACIÓN CRUZADA

| Representación | Herramienta | Comprobación |
|:--|:--|:--|
| Ecuaciones del sistema | Simulink | Dinámica de los desplazamientos |
| Funciones de transferencia | Simulink | Relación entrada–salida |
| Circuito eléctrico equivalente | Simscape Electrical | Concordancia con la analogía |
| Cuaderno computacional | MATLAB | Extracción de datos y figuras |
| Implementación independiente | Python | Reproducción de las respuestas |

<p align="justify">Las diferencias observadas pueden deberse a condiciones iniciales, aproximación del impulso, configuración numérica, unidades, conexión de elementos o implementación del controlador. No se afirmará equivalencia cuantitativa sin revisar los resultados.</p>

<a id="diagrama-biológico"></a>

---

## 16 / DIAGRAMA BIOLÓGICO

<p align="justify">Se elaborará en **BioRender** un diagrama que identifique como mínimo:</p>

- Elemento contráctil muscular.
- Estructuras elásticas musculares.
- Tendón.
- Desplazamiento.
- Fuerza generada.
- Elementos asociados al comportamiento viscoso y elástico.

| Elemento biológico | Modelo mecánico | Equivalente eléctrico |
|:--|:--|:--|
| Contracción muscular activa | $F_0(t)$ | Fuente de voltaje equivalente |
| Componente elástico paralelo | $C_p$ | Capacitor equivalente |
| Componente elástico en serie / tendón | $C_s$ | Capacitor equivalente |
| Resistencia viscosa del tejido | $R$ | Resistencia eléctrica |
| Desplazamiento | $x(t)$, $x_1(t)$ | Carga eléctrica análoga |
| Fuerza transmitida | $F(t)$ | Voltaje equivalente |

**Evidencia por agregar:** `imagenes/diagrama_biologico.png`.

<a id="actividades-y-evidencias"></a>

---

## 17 / ACTIVIDADES Y EVIDENCIAS

### Etapa 1 · Modelo y análisis

- [ ] Identificar los elementos y desplazamientos del modelo mecánico.
- [ ] Construir y explicar la analogía electromecánica.
- [ ] Formular las ecuaciones del sistema y verificar el circuito.
- [ ] Obtener las funciones de transferencia del control y del caso.
- [ ] Calcular polos, estabilidad y error en estado estacionario.

### Etapa 2 · Simulación

- [ ] Construir las ecuaciones en Simulink.
- [ ] Implementar las funciones de transferencia en Simulink.
- [ ] Construir el circuito en Simscape Electrical.
- [ ] Simular la respuesta al impulso entre $0$ y $10$ s.
- [ ] Comparar control y caso en lazo abierto.

### Etapa 3 · Control y verificación

- [ ] Incorporar el PID al caso y sintonizarlo con Tune.
- [ ] Registrar $k_P$, $k_I$ y $k_D$ con evidencia.
- [ ] Comparar control, caso y caso con PID.
- [ ] Desarrollar el cuaderno MATLAB y exportar gráficas PDF.
- [ ] Implementar las respuestas en Python.
- [ ] Verificar los cinco métodos.

### Etapa 4 · Documentación

- [ ] Elaborar el diagrama biológico en BioRender.
- [ ] Incorporar capturas, archivos y resultados.
- [ ] Completar discusión y conclusiones propias.
- [ ] Incluir `README.md` y `CITATION.cff`.

<a id="organización-del-repositorio"></a>

---

## 18 / ORGANIZACIÓN DEL REPOSITORIO

<p align="justify">La siguiente estructura es **sugerida**; adaptar los nombres a los archivos reales:</p>

```text
.
├── README.md
├── CITATION.cff
├── matlab/
│   └── practica_musculoesqueletica.mlx
├── simulink/
│   └── sistema_musculoesqueletico.slx
├── python/
│   └── sistema_musculoesqueletico.py
├── resultados/
│   ├── lazo_abierto_matlab.pdf
│   ├── lazo_cerrado_matlab.pdf
│   ├── lazo_abierto_python.pdf
│   └── lazo_cerrado_python.pdf
└── imagenes/
    ├── modelo_mecanico.png
    ├── circuito_equivalente.png
    ├── sintonizacion_pid.png
    └── diagrama_biologico.png
```

<a id="resultados-y-discusión"></a>

---

## 19 / RESULTADOS Y DISCUSIÓN

**Por completar con las simulaciones propias.** Registrar las funciones de transferencia verificadas, los polos, las ganancias PID y las figuras de control, caso y sistema controlado. La discusión deberá indicar si las diferentes implementaciones producen respuestas consistentes y explicar cualquier diferencia observada.

| Aspecto | Resultado / evidencia |
|:--|:--|
| Función de transferencia del control | Pendiente |
| Función de transferencia del caso | Pendiente |
| Polos y estabilidad | Pendiente |
| Respuesta al impulso en lazo abierto | Pendiente |
| Ganancias del PID | Pendiente |
| Respuesta en lazo cerrado | Pendiente |
| Comparación MATLAB–Python–Simscape | Pendiente |
| Diagrama biológico | Pendiente |

<a id="conclusiones"></a>

---

## 20 / CONCLUSIONES

<p align="justify">El modelo mecánico simplificado permite estudiar cómo interactúan los componentes activos, viscosos y elásticos del sistema musculoesquelético. La analogía fuerza–voltaje ofrece una representación eléctrica que facilita la formulación matemática y la simulación de las respuestas dinámicas. En esta práctica, la modificación de la resistencia equivalente distingue el control del caso simulado de atrofia muscular.</p>

<p align="justify">La efectividad del controlador PID y la concordancia entre MATLAB, Simulink, Simscape Electrical y Python **deberán evaluarse a partir de las gráficas y los valores realmente obtenidos**. El modelo y las estrategias de control se utilizan exclusivamente con fines educativos y no constituyen una intervención terapéutica.</p>

<a id="bibliografía"></a>

---

## 21 / BIBLIOGRAFÍA

1. M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2.ª ed., IEEE Press, 2018.
2. F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.
3. J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14.ª ed., Elsevier, 2021.

---

<div align="center">

**Modelado de Sistemas Fisiológicos · Ingeniería Biomédica · IT Tijuana**  
<p align="justify">*Práctica académica de simulación in silico*</p>

</div>
