# Análisis Detallado de `app/utils/qr_generator.py`

Este documento explica de forma detallada la estructura y funcionamiento del módulo generador de códigos QR del sistema **SIVA**, diferenciando claramente su código fuente ejecutable de sus secciones de documentación y comentarios.

---

## 1. Estructura General del Archivo

El archivo consta de **67 líneas** distribuidas funcionalmente como se detalla en el siguiente cuadro:

| Sección | Rango de Líneas | Tipo | Descripción |
| :--- | :---: | :---: | :--- |
| **Importación de Librerías** | 1 - 5 | Código | Carga los módulos requeridos para el proceso. |
| **Función `sumar_anios`** | 8 - 19 | Código / Docstring | Calcula de forma segura años hacia el futuro. |
| **Función `generar_qr`** | 22 - 67 | Código / Docstring | Genera el código QR con datos del ciudadano en JSON. |

---

## 2. Sección por Sección: Código vs. Documentación

A continuación se presenta un desglose de cada sección indicando explícitamente cuáles líneas corresponden a código y cuáles son comentarios explicativos o docstrings.

### 2.1 Importación de Librerías (Líneas 1-5)

#### Código Ejecutable:
```python
import qrcode
import json
import os
from io import BytesIO
from datetime import datetime
```

* **qrcode**: Librería externa que genera la matriz visual del código de barras QR.
* **json**: Para estructurar y serializar la información del usuario a formato string.
* **os**: Para interactuar con carpetas del sistema operativo (crear directorios).
* **BytesIO**: Estructura de memoria virtual que actúa como un archivo temporal en la memoria RAM.
* **datetime**: Utilidad para gestionar fechas y horas reales del sistema.

*(Esta sección no contiene comentarios ni documentación).*

---

### 2.2 Función `sumar_anios` (Líneas 8-19)

Esta función calcula de forma segura una fecha sumándole una cantidad fija de años, previniendo fallos en años bisiestos.

#### Documentación (Docstring) (Líneas 9-15):
```python
    """
    Suma años a una fecha de forma segura.

    Usar fecha.replace(year=...) directamente lanza ValueError cuando la fecha es
    el 29 de febrero y el año destino no es bisiesto. En ese caso se ajusta al
    28 de febrero (comportamiento estándar para vencimientos de documentos).
    """
```
* **Explicación**: Es una cadena multilínea (delimitada por `"""`) que la computadora ignora durante la ejecución. Sirve exclusivamente para explicar por qué no se usa un reemplazo directo del año y cuál es la solución implementada.

#### Código Ejecutable (Líneas 8, 16-19):
```python
def sumar_anios(fecha, anios):
    try:
        return fecha.replace(year=fecha.year + anios)
    except ValueError:
        return fecha.replace(year=fecha.year + anios, month=2, day=28)
```
* **Explicación**:
  * `try`: Intenta incrementar el año directamente.
  * `except ValueError`: Captura el error en caso de que sea el 29 de febrero de un año bisiesto y el año objetivo no lo sea. Ajusta de forma segura el día al 28 de febrero para evitar que el backend colapse.

---

### 2.3 Función Principal `generar_qr` (Líneas 22-67)

Se encarga de recibir los datos del ciudadano, empaquetarlos en formato JSON y renderizar la imagen del código QR.

#### Documentación (Docstring) (Líneas 23-32):
```python
    """
    Genera un código QR con los datos del usuario.
    
    Args:
        datos_usuario: dict con 'nombres', 'apellidos', 'cedula', 'fecha_nacimiento'
        ruta_salida: opcional path donde guardar la imagen del QR
    
    Returns:
        str o BytesIO: ruta del archivo QR guardado o un flujo de bytes en memoria
    """
```
* **Explicación**: Especifica formalmente los parámetros que requiere la función (`Args`) y el tipo de dato que devolverá tras finalizar (`Returns`).

#### Código Ejecutable (Líneas 33-67):

##### A. Obtención de Fechas y Declaración del Contenido (Líneas 33-44)
```python
    hoy = datetime.now()
    vencimiento = sumar_anios(hoy, 10)

    contenido = {
        "sistema": "SIVA",
        "cedula": datos_usuario.get('cedula', ''),
        "nombres": datos_usuario.get('nombres', ''),
        "apellidos": datos_usuario.get('apellidos', ''),
        "fecha_nacimiento": datos_usuario.get('fecha_nacimiento', ''),
        "fecha_expedicion": hoy.strftime('%d/%m/%Y'),
        "valido_hasta": vencimiento.strftime('%d/%m/%Y')
    }
```
* **Explicación**: Determina la fecha de generación e invoca a `sumar_anios` para establecer la expiración a los 10 años. Posteriormente, construye un diccionario Python que empaqueta la información personal necesaria formateando las fechas como `Día/Mes/Año`.

##### B. Configuración e instanciación del QR (Líneas 46-56)
```python
    qr = qrcode.QRCode(
        version=1,
        error_correction=qrcode.constants.ERROR_CORRECT_M,
        box_size=10,
        border=2,
    )
    
    qr.add_data(json.dumps(contenido))
    qr.make(fit=True)
    
    img = qr.make_image(fill_color="black", back_color="white")
```
* **Explicación**: instancía un objeto de tipo `QRCode` con parámetros estandarizados (tamaño mínimo 1, corrección de errores nivel medio para permitir desgaste físico, tamaño de bloques y bordes). Serializa el diccionario del paso anterior a una cadena JSON pura (`json.dumps(contenido)`), lo añade a la biblioteca y genera una imagen clásica en blanco y negro.

##### C. Guardado y Retorno Condicionado (Líneas 58-67)
```python
    if ruta_salida:
        os.makedirs(os.path.dirname(ruta_salida), exist_ok=True)
        img.save(ruta_salida)
        return ruta_salida
    else:
        buffer = BytesIO()
        img.save(buffer, format="PNG")
        buffer.seek(0)
        return buffer
```
* **Explicación**: Procesa el archivo resultante dependiendo de si se pasa o no una ruta física:
  - **Con ruta física**: Crea directorios padre de no existir, guarda allí la imagen en formato PNG y retorna el String de la ruta.
  - **Sin ruta física**: Inicializa un buffer binario virtual `BytesIO()`, vuelca la imagen en formato PNG en dicho buffer, rebobina el puntero al inicio (`buffer.seek(0)`) y lo devuelve para ser consumido en caliente (por ejemplo, para incrustarlo en el ReportLab del PDF).
