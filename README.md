# Optimización de Eficiencia Aerodinámica

Este repositorio contiene el código y los datos para un proyecto de Métodos Numéricos. El objetivo principal del script es analizar datos de simulación aerodinámica y encontrar el **ángulo de ataque óptimo ($\alpha_{opt}$)** que maximiza la **eficiencia aerodinámica ($E = |C_L|/C_D$)** de un perfil alar.

## 🚀 Descripción del Problema

En la ingeniería de competición, existe un equilibrio fundamental ("trade-off") entre:
* **Downforce (Carga Aerodinámica, $C_L$):** Fuerza que "aplasta" al vehículo contra el suelo, permitiendo mayor agarre en curvas.
* **Drag (Resistencia, $C_D$):** Fuerza que se opone al movimiento, limitando la velocidad en recta.

El objetivo de un ingeniero no es maximizar uno u otro, sino encontrar el ángulo de ataque ($\alpha$) que ofrece la mejor relación entre ambos. Este script automatiza la búsqueda de ese "punto dulce".

## 🛠️ Metodología

El script utiliza una estrategia híbrida de modelado y optimización para encontrar el ángulo óptimo:

1.  **Modelado (Regresión Polinomial):**
    * Carga los datos de simulación (`Datos de Simulacion.csv`).
    * Para cada `Caso_ID` diferente, ajusta dos polinomios de **grado 4** para crear modelos matemáticos continuos que describen la relación entre el ángulo y los coeficientes: $C_L(\alpha)$ y $C_D(\alpha)$. Se usa `numpy.polyfit`.

2.  **Optimización (Newton-Raphson):**
    * Define la función de eficiencia $E(\alpha) = |C_L(\alpha)|/C_D(\alpha)$.
    * Calcula la **derivada analítica** de la eficiencia ($dE/d\alpha$) usando la regla del cociente.
    * Utiliza el método `scipy.optimize.newton` para encontrar la raíz de la derivada (es decir, el punto donde $dE/d\alpha = 0$), que corresponde al ángulo de máxima eficiencia.

## 📂 Archivos en este Repositorio

* **`optimizador_aero.ipynb`** (o `.py`): El script principal de Python que carga los datos, realiza el modelado polinomial y ejecuta la optimización de Newton-Raphson.
* **`Datos de Simulacion.csv`**: El conjunto de datos de entrada que contiene los `Caso_ID`, `Alpha_Grados`, `CL_Downforce`, y `CD_Drag` de las simulaciones.

## ⚙️ Requisitos

Para ejecutar este script, necesitarás Python 3.x y las siguientes librerías:

* **pandas**
* **numpy**
* **scipy**

Puedes instalarlas usando pip:
```bash
pip install pandas numpy scipy
```
## ▶️ Cómo Usar

1.  Asegúrate de tener todas las librerías de la sección de Requisitos instaladas.
2.  Coloca el archivo `Datos de Simulacion.csv` en el mismo directorio que el script (`optimizador_aero.ipynb` o `.py`).
3.  Ejecuta el script (o las celdas del cuaderno de Jupyter/Colab).
4.  Los resultados de la optimización para cada `Caso_ID` se imprimirán en la consola.

## 📊 Ejemplo de Salida

Al ejecutar el script, se imprimirá un análisis detallado para cada caso, similar a este:

```text
================================================================================
VALIDACIÓN Y PRUEBAS: OPTIMIZACIÓN DEL ÁNGULO DE ATAQUE ÓPTIMO
================================================================================

--- CASO 1: MAXIMA_EFICIENCIA ---
|  MÉTODO DE ENTRADA Y MODELADO:
|    -> Puntos de datos: 1000
|    -> RMSE CL (Error de Regresión): 0.005160
|    -> RMSE CD (Error de Regresión): 0.001001
|    -> Estimación Inicial (α_guess): 0.76 grados
|
|  RESULTADO DE OPTIMIZACIÓN (NEWTON-RAPHSON):
|    -> ÁNGULO ÓPTIMO (α_optimo): 0.6797 grados
|    -> COEF. DOWNFORCE (CL): -0.7398
|    -> COEF. DRAG (CD): 0.02454
|    -> EFICIENCIA MÁXIMA (|CL/CD|): 30.146

--- CASO 2: ALTA_CARGA ---
|  MÉTODO DE ENTRADA Y MODELADO:
|    -> Puntos de datos: 1000
|    -> RMSE CL (Error de Regresión): 0.005182
...
```
## 👨‍💻 Autores

* **Brandon Cáceres**
* **Joaquin Contreras**
* **Josue Huaiquil**
* **Ignacio Muñoz**
