<div align="center">

# MODELADO DE SISTEMAS FISIOLÓGICOS

## Práctica 3 — Sistema musculoesquelético

*Modelado, simulación y control PID*

**TecNM | Instituto Tecnológico de Tijuana**

</div>

---

> **Autora:** María Luisa Michelle Muñoz Meneses  
> **Control:** 23210714  
> **Correo:** l23210714@tectijuana.edu.mx  
> **Carrera:** Ingeniería Biomédica  
> **Docente:** Dr. Paul Antonio Valle Trujillo  
> **Asignatura:** Modelado de Sistemas Fisiológicos

## Resumen

<div align="justify">

La práctica estudia un modelo musculoesquelético con componentes contráctiles, elásticos y viscosos. Se construye su analogía eléctrica, se compara una condición sana con una simulación de atrofia muscular y se plantea un controlador PID para disminuir el error entre las respuestas. Se emplean MATLAB, Simulink, Simscape Electrical y Python.

**Palabras clave:** Sistema musculoesquelético · Analogía electromecánica · Atrofia muscular · Amortiguamiento · Función de transferencia · PID · Simulación.

</div>

---

## I. Propósito

<div align="justify">

**Objetivo general.** Modelar el sistema musculoesquelético, evaluar el efecto de aumentar su amortiguamiento equivalente y diseñar un control PID que siga la referencia sana.

**Objetivos específicos.** Identificar elementos, establecer la analogía eléctrica, deducir funciones de transferencia, analizar estabilidad y error, simular en lazo abierto/cerrado y verificar los resultados con Python.

</div>

## II. Descripción del sistema

<div align="justify">

El sistema musculoesquelético integra músculos y tendones para producir movimiento. El modelo simplificado considera fuerza activa $F_0(t)$, fuerza aplicada $F(t)$, resistencia viscosa $R$, elasticidades $C_p$ y $C_s$, y desplazamientos $x(t)$ y $x_1(t)$.

$$
F_0(t)=\alpha F(t),\qquad \alpha=0.25
$$

| Componente | Significado |
|---|---|
| $F_0(t)$ | Fuerza contráctil activa |
| $R$ | Amortiguamiento viscoso |
| $C_p$ | Compliancia elástica paralela |
| $C_s$ | Compliancia elástica en serie |
| $x(t),x_1(t)$ | Desplazamientos del modelo |

</div>

## III. Conversión al circuito equivalente

La analogía fuerza–voltaje establece:

$$
\begin{aligned}
F(t)&\leftrightarrow V(t)\\
\dot{x}(t)&\leftrightarrow I(t)\\
x(t)&\leftrightarrow q(t)
\end{aligned}
$$

| Mecánica | Electricidad |
|---|---|
| Amortiguador | Resistencia |
| Compliancia | Capacitancia |
| Masa | Inductancia |

### Parámetros

| Variable | Control (sano) | Caso (atrofia simulada) |
|---|---:|---:|
| Fuente equivalente | 1 V | 1 V |
| $\alpha$ | 0.25 | 0.25 |
| $C_s$ | 10 µF | 10 µF |
| $C_p$ | 100 µF | 100 µF |
| $R$ | 100 Ω | 10 kΩ |

<div align="justify">

El cambio de resistencia es de un factor de **100**; es una hipótesis de simulación para representar una alteración del comportamiento dinámico, no una medición clínica de atrofia.

</div>

## IV. Desarrollo matemático

Las relaciones de los elementos son:

$$
F_R(t)=R\frac{d}{dt}\bigl[x(t)-x_1(t)\bigr]
$$

$$
F_{C_p}(t)=\frac{x(t)}{C_p},\qquad F_{C_s}(t)=\frac{x_1(t)}{C_s}
$$

Se busca la relación entrada–salida:

$$
\boxed{G(s)=\frac{X(s)}{F(s)}}
$$

Se determinan por separado $G_{\mathrm{control}}(s)$ y $G_{\mathrm{caso}}(s)$. **Las expresiones numéricas definitivas requieren confirmar la conexión de los elementos del circuito.**

### Estabilidad

Para $G(s)=N(s)/D(s)$, los polos satisfacen $D(s)=0$. Si todos tienen parte real negativa, el sistema continuo es asintóticamente estable.

### Estado estacionario

Cuando se cumplen las hipótesis del teorema del valor final:

$$
x(\infty)=\lim_{s\to0}sX(s)
$$

## V. Metodología de simulación

| Etapa | Procedimiento | Herramienta |
|---|---|---|
| 1 | Construir ecuaciones diferenciales | Simulink |
| 2 | Implementar $G(s)$ | Simulink |
| 3 | Armar el circuito equivalente | Simscape Electrical |
| 4 | Comparar control y caso | MATLAB |
| 5 | Ajustar controlador | PID Tune |
| 6 | Reproducir y comparar gráficas | Python |

**Condiciones:** entrada impulso unitario; tiempo de simulación de **0 a 10 segundos**; parámetros constantes salvo $R$.

### Lazo abierto

Comparar $x_{\mathrm{control}}(t)$ y $x_{\mathrm{caso}}(t)$, evaluando amplitud, rapidez y estabilidad.

### Lazo cerrado y PID

La referencia es la respuesta del individuo sano:

$$
e(t)=x_{\mathrm{control}}(t)-x_{\mathrm{caso}}(t)
$$

$$
u(t)=k_Pe(t)+k_I\int_0^t e(\tau)\,d\tau+k_D\frac{de(t)}{dt}
$$

$$
K(s)=k_P+\frac{k_I}{s}+k_Ds
$$

| $k_P$ | $k_I$ | $k_D$ |
|:--:|:--:|:--:|
| Pendiente | Pendiente | Pendiente |

Se comparan las respuestas **control**, **caso** y **caso con PID**.

## VI. Representación anatómica

<div align="justify">

En BioRender se relacionan el elemento contráctil con el músculo, la compliancia en serie con el tendón, la compliancia paralela con la elasticidad muscular y la resistencia con el comportamiento viscoso. También se identifican fuerza y desplazamiento.

</div>

## VII. Evidencias y resultados

| Evidencia | Estado |
|---|---|
| Circuito equivalente y función de transferencia | Pendiente |
| Gráfica de lazo abierto | Pendiente |
| Sintonización PID | Pendiente |
| Gráfica de lazo cerrado | Pendiente |
| Comparación MATLAB/Python | Pendiente |
| Ilustración BioRender | Pendiente |
| Archivos `README.md` y `CITATION.cff` | Pendiente |

| Indicador | Control | Caso | Con PID |
|---|---|---|---|
| Amplitud máxima | — | — | — |
| Tiempo de establecimiento | — | — | — |
| Estabilidad | — | — | — |

## VIII. Conclusión

<div align="justify">

El modelo permite estudiar las propiedades elásticas y viscosas del sistema musculoesquelético mediante una analogía eléctrica. El incremento de resistencia modifica la dinámica simulada; el desempeño del PID se evaluará con las respuestas obtenidas. La comparación entre MATLAB y Python servirá para comprobar la consistencia de las simulaciones.

</div>

## IX. Referencias

1. Khoo, M. C. (2018). *Physiological Control Systems* (2.ª ed.). IEEE Press.
2. Hoppensteadt, F. C., y Peskin, C. S. *Modeling and Simulation in Medicine and the Life Sciences*. Springer.
3. Hall, J. E. (2021). *Guyton and Hall Textbook of Medical Physiology* (14.ª ed.). Elsevier.
4. Valle Trujillo, P. A. *Material de Modelado de Sistemas Fisiológicos*. Instituto Tecnológico de Tijuana.

---

<div align="center">

*Ingeniería Biomédica · Instituto Tecnológico de Tijuana*

</div>
