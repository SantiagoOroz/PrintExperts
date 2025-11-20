# Print Intelligence - Sistema Experto de Diagnóstico 🖨️

**Print Intelligence** es una aplicación web basada en un Sistema Experto diseñada para diagnosticar fallas comunes en impresoras. Utiliza un motor de inferencia basado en reglas para guiar al usuario desde un síntoma observable hasta una solución concreta, permitiendo además la expansión dinámica de su base de conocimiento.

<p align="center">
  <img src="static/images/Print_intelligence_logosinletras.png" width="10%">
</p>


## 🚀 Funcionalidades Principales

### 1\. Diagnóstico Guiado por Pasos

El sistema guía al usuario a través de un flujo lógico para identificar el problema:

  * **Selección de Categoría:** Clasificación del problema (Conectividad, Suministros, Mecánica, Calidad de Imagen, Electrónica).
  * **Síntoma Observable:** Selección de la falla visible o mensaje de error específico.
  * **Interrogatorio Inteligente:** El sistema genera preguntas de "Sí/No" dinámicamente basadas en las reglas asociadas al síntoma.
  * **Resultado:** Entrega una causa probable y una lista de acciones correctivas.

### 2\. Motor de Inferencia y Trazabilidad

  * **Lógica de Diagnóstico:** Evalúa premisas y respuestas para validar hipótesis.
  * **Trazabilidad (Explainability):** Al finalizar el diagnóstico, el sistema muestra el "porqué" de la conclusión, detallando qué premisas se cumplieron, cuáles se descartaron y las respuestas del usuario.

### 3\. Gestión de Conocimiento (Knowledge Base)

  * **Base de Conocimiento Dual:**
      * *Base Estándar (`knowledge_base.json`):* Reglas predefinidas inmutables.
      * *Base de Usuario (`knowledge_user.json`):* Base extendida que permite personalización.
  * **Agregar Conocimiento:** Interfaz gráfica dedicada para que expertos o técnicos agreguen nuevas reglas sin tocar el código fuente.
      * Creación de nuevos síntomas o uso de existentes.
      * Definición de nuevas premisas (preguntas) o reutilización de las ya existentes.
      * Validación lógica para evitar reglas duplicadas o inconsistentes.

### 4\. Interfaz de Usuario

  * Diseño responsivo y limpio con CSS personalizado.
  * Feedback visual mediante barras de progreso (pasos).
  * Validaciones en tiempo real mediante JavaScript en los formularios de carga de datos.

-----

## 🛠️ Tecnologías Utilizadas

### Backend

  * **Python 3:** Lenguaje principal del proyecto.
  * **Flask:** Framework web ligero para el manejo de rutas, sesiones y API.
  * **JSON:** Formato para el almacenamiento estructurado de la base de conocimiento (NoSQL local).

### Frontend

  * **HTML5 (Jinja2):** Motor de plantillas para renderizar las vistas dinámicas.
  * **CSS3:** Estilos personalizados (`style.css`) con diseño moderno y degradados.
  * **JavaScript (Vanilla):** Lógica del lado del cliente para validaciones de formularios y llamadas asíncronas (Fetch API) para cargar síntomas y premisas dinámicamente.

-----

## 📂 Estructura del Proyecto

```text
proyecto/
│
├── app.py                 # Punto de entrada. Rutas Flask y lógica de sesión.
├── config.py              # Configuración de rutas de archivos JSON.
├── motor_inferencia.py    # Lógica pura del sistema experto (evaluación de reglas).
├── utils.py               # Funciones auxiliares (normalización de texto, booleanos).
├── requirements.txt       # Dependencias del proyecto.
│
├── data/ (Implícito)
│   ├── knowledge_base.json    # Base de conocimiento original.
│   └── knowledge_user.json    # Base de conocimiento extendida por el usuario.
│
├── static/
│   ├── css/
│   │   └── style.css      # Estilos de la aplicación.
│   └── images/            # Logotipos y recursos gráficos.
│
└── templates/
    └── index.html         # Plantilla única dinámica (maneja todos los pasos).
```

-----

## ⚙️ Instalación y Ejecución

1.  **Clonar el repositorio o descargar los archivos.**

2.  **Crear un entorno virtual (Opcional pero recomendado):**

    ```bash
    python -m venv venv
    # En Windows
    venv\Scripts\activate
    # En Mac/Linux
    source venv/bin/activate
    ```

3.  **Instalar dependencias:**

    ```bash
    pip install -r requirements.txt
    ```

4.  **Ejecutar la aplicación:**

    ```bash
    python app.py
    ```

5.  **Acceder:**
    Abrir el navegador en `http://127.0.0.1:5000`.

-----

## 🧠 Cómo funciona el Motor de Inferencia

El archivo `motor_inferencia.py` orquesta la lógica:

1.  **Filtrado:** Al seleccionar un síntoma, el motor busca en el JSON todas las reglas que coinciden con ese síntoma (`reglas_candidatas`).
2.  **Unificación:** Recopila todas las preguntas únicas necesarias para evaluar esas reglas.
3.  **Evaluación:**
      * Compara las respuestas del usuario (guardadas en `session`) contra las **premisas** requeridas por cada regla.
      * Si todas las premisas de una regla son `True` (o confirman la hipótesis directa), la regla se acepta.
4.  **Resolución:** Retorna la hipótesis y las acciones de la primera regla que cumpla todas las condiciones.
