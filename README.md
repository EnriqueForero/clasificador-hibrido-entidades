# 🏢 Clasificador híbrido de empresas y personas

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![Pandas](https://img.shields.io/badge/Pandas-1.x+-blue.svg)](https://pandas.pydata.org/)
[![Openpyxl](https://img.shields.io/badge/Openpyxl-3.x+-green.svg)](https://openpyxl.readthedocs.io/en/stable/)
[![Google Colab](https://colab.research.google.com/assets/colab-badge.svg)]([https://colab.research.google.com/](https://colab.research.google.com/drive/1qFzpTWwm2tudSYiRQmF19gawhI0BZDfT#scrollTo=5zNJTO-0nXYH)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## 📝 Descripción

El **Clasificador híbrido de empresas y personas** es una herramienta desarrollada para apoyar a la **DIAN (Dirección de Impuestos y Aduanas Nacionales)** en la clasificación automática de entidades entre **empresas** y **personas naturales**.

Este sistema combina un **motor de reglas especializado** con **análisis estadístico** para lograr alta precisión en la identificación de entidades empresariales en bases de datos de exportaciones y registros comerciales.

## 🌟 Características Principales

### 🎯 **Regla de oro**
- Sistema especializado que garantiza clasificación correcta para sufijos legales colombianos (S.A.S., LTDA, E.U., etc.)
- Reconocimiento de patrones corporativos internacionales y entidades especiales

### 🧠 **Clasificación híbrida**
- **Motor de reglas**: Identifica patrones específicos de empresas y personas
- **Motor estadístico**: Analiza patrones de palabras residuales para casos ambiguos
- **Umbrales configurables**: Permite ajustar la sensibilidad del clasificador

### 🇨🇴 **Especialización colombiana**
- Diccionario extenso de sufijos legales colombianos
- Patrones de sectores industriales locales
- Reconocimiento de estados legales empresariales (reestructuración, liquidación, etc.)

### ⚡ **Alto rendimiento**
- Procesamiento eficiente de grandes volúmenes de datos
- Optimizado para datasets de exportaciones de la DIAN
- Resultados con métricas de confianza detalladas

## 🎯 Casos de uso

- **Análisis de exportaciones**: Clasificación de exportadores en bases de datos de la DIAN
- **Estadísticas comerciales**: Generación de reportes diferenciados por tipo de entidad
- **Validación de datos**: Identificación de inconsistencias en registros comerciales
- **Investigación económica**: Análisis del comportamiento empresarial vs. personal en comercio exterior

## 🧠 Cómo Funciona

### 1. **Normalización de Texto**
```
"TECNOLOGÍAS GLOBALES S.A.S." → "TECNOLOGIAS GLOBALES S A S"
```

### 2. **Motor de reglas (Puntaje Inicial)**
- **Sufijos Empresariales**: S.A.S.(+10), LTDA(+9), E.U.(+9)
- **Títulos Personales**: DR.(-7), SR.(-5), JR.(-5)
- **Patrones Corporativos**: GRUPO(+7), HOLDING(+7), TECH(+12)

### 3. **Regla de oro (Activación Especial)**
```python
# Si encuentra términos clave, garantiza clasificación empresarial
golden_rule_terminos = ['S.A.S.', 'LTDA', 'CORPORATION', ...]
if termino_oro_encontrado:
    puntaje += 100  # Garantiza clasificación como empresa
```

### 4. **Motor estadístico (Refinamiento)**
- Analiza palabras residuales después de eliminar términos conocidos
- Aplica pesos estadísticos basados en frecuencia de aparición
- Limita impacto para evitar sobreajuste

### 5. **Clasificación final**
```python
if puntaje_total > umbral_empresa (0.01):
    clasificacion = 'empresa'
elif puntaje_total < umbral_persona (-0.01):
    clasificacion = 'persona'
else:
    clasificacion = 'incierto'
```

## 📊 Ejemplo de Resultados

| RAZON_SOCIAL_FINAL | clasificacion | puntaje_total | terminos_encontrados | regla_oro_aplicada |
|-------------------|---------------|---------------|---------------------|-------------------|
| A & D ALVARADO & DURING S A S | empresa | 109.19 | ['S A S(+10)'] | VERDADERO |
| ABBOTT LABORATORIES DE COLOMBIA SAS | empresa | 115.88 | ['SAS(+10)'] | VERDADERO |
| WILSON JR ROBERT RONALD | persona | -10.37 | ['JR(-5)'] | FALSO |
| YASOFSKY JR MICHAEL EDWARD | persona | -8.90 | ['JR(-5)'] | FALSO |
| 11001 | incierto | 0.00 | [] | FALSO |

## ⚡ Inicio Rápido

### 📋 Ejemplo Mínimo

```python
# 1. Configurar el clasificador
config = {
    'umbral_empresa': 0.01,
    'umbral_persona': -0.01,
    'golden_rule_terminos': ['S.A.S.', 'LTDA', 'E.U.'],
    'empresa_strong_indicators': {'S.A.S.': 10, 'LTDA': 9},
    'persona_strong_indicators': {'JR.': -5, 'DR.': -7}
}

# 2. Crear y entrenar el clasificador
clasificador = ClasificadorHibrido(config)
clasificador.entrenar(df_empresas, 'RAZON_SOCIAL', df_personas, 'RAZON_SOCIAL')

# 3. Clasificar datos
resultado = clasificador.clasificar_nombre("TECNOLOGIAS AVANZADAS S.A.S.")
print(resultado['clasificacion'])  # Output: 'empresa'
```

## 🛠️ Instalación y configuración

### 🐍 **Opción 1: Google Colab (Recomendado)**

```bash
# Instalar dependencias
!pip install fuzzywuzzy rapidfuzz openpyxl

# Clonar repositorio
!git clone https://github.com/enriqueforero/clasificador-hibrido-entidades.git
%cd clasificador-hibrido-entidades

# Ejecutar
%run clasificador_hibrido.py
```

### 💻 **Opción 2: Instalación local**

```bash
# Clonar repositorio
git clone https://github.com/enriqueforero/clasificador-hibrido-entidades.git
cd clasificador-hibrido-entidades

# Crear entorno virtual
python -m venv env
source env/bin/activate  # Linux/Mac
# env\Scripts\activate   # Windows

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar
python clasificador_hibrido.py
```

## 📁 Estructura del proyecto

```
clasificador-hibrido-entidades/
├── clasificador_hibrido.py      # Script principal
├── requirements.txt             # Dependencias Python
├── README.md                   # Documentación (este archivo)
├── datos_ejemplo/              # Datos de ejemplo para pruebas
│   ├── empresas_muestra.xlsx
│   ├── personas_muestra.xlsx
│   └── datos_clasificar.xlsx
├── resultados/                 # Directorio de salida
└── notebooks/                  # Notebooks de análisis
    └── analisis_rendimiento.ipynb
```

## 📋 Requisitos del sistema

### **Python 3.9+**

### **Librerías Principales:**
- `pandas >= 1.3.0`: Manipulación y análisis de datos
- `numpy >= 1.21.0`: Operaciones numéricas
- `openpyxl >= 3.0.0`: Lectura/escritura de archivos Excel
- `fuzzywuzzy >= 0.18.0`: Coincidencia aproximada de cadenas
- `rapidfuzz >= 2.0.0`: Algoritmos de distancia de cadenas optimizados
- `pytz`: Manejo de zonas horarias
- `unicodedata`: Normalización de texto (incluido en Python)
- `re`: Expresiones regulares (incluido en Python)

## 📊 Preparación de datos de entrada

### **Formato Requerido:**

1. **Archivo de empresas para entrenamiento**: Excel con columna `RAZON_SOCIAL`
2. **Archivo de personas para entrenamiento**: Excel con columna `RAZON_SOCIAL`  
3. **Archivo a clasificar**: Excel con columna `RAZON_SOCIAL_FINAL`. Puede incluir otro nombre. 

### **Ejemplo de estructura:**

```excel
# empresas_entrenamiento.xlsx
RAZON_SOCIAL
TECNOLOGIAS GLOBALES S.A.S.
COMERCIALIZADORA ANDINA LTDA
GRUPO EMPRESARIAL COLOMBIA E.U.

# personas_entrenamiento.xlsx  
RAZON_SOCIAL
JUAN CARLOS PEREZ GOMEZ
DR. ROBERTO MORALES SILVA
MARIA FERNANDA RODRIGUEZ JR.

# datos_clasificar.xlsx
RAZON_SOCIAL_FINAL
INNOVACIONES TECH S.A.S.
CARLOS ALBERTO MENDEZ
DISTRIBUIDORA NACIONAL LTDA
```

## ⚙️ Configuración y personalización

### **Ajuste de umbrales:**
```python
config = {
    'umbral_empresa': 0.01,    # Valores > 0.01 = empresa
    'umbral_persona': -0.01,   # Valores < -0.01 = persona
    'max_stat_score': 20.0     # Límite del puntaje estadístico
}
```

### **Personalización de términos:**
```python
# Agregar nuevos sufijos empresariales
config['empresa_strong_indicators']['NUEVA_FORMA_JURIDICA'] = 8

# Agregar términos a la Regla de Oro
config['golden_rule_terminos'].append('NUEVO_SUFIJO_CRITICO')
```

## 📈 Rendimiento y métricas

### **Métricas de salida por registro:**
- `clasificacion`: empresa / persona / incierto / invalido
- `puntaje_total`: Puntaje final de clasificación
- `puntaje_reglas`: Contribución del motor de reglas
- `puntaje_estadistico`: Contribución del motor estadístico
- `terminos_encontrados`: Lista de términos detectados con sus pesos
- `regla_oro_aplicada`: Si se activó la regla de oro

### **Interpretación de resultados:**
- **Empresa**: Puntaje > 0.01 (alta confianza si >10)
- **Persona**: Puntaje < -0.01 (alta confianza si <-5)
- **Incierto**: Puntaje entre -0.01 y 0.01
- **Inválido**: Texto insuficiente o datos corruptos

## ⚠️ Limitaciones y consideraciones

1. **Actualización de diccionarios**: Es necesario mantener actualizados los diccionarios de términos conforme aparezcan nuevos patrones en los nombres empresariales.

2. **Datos de entrenamiento**: La calidad del motor estadístico depende de la representatividad de los datos de entrenamiento.

3. **Casos ambiguos**: Nombres muy cortos o poco descriptivos pueden clasificarse como "incierto".

4. **Contexto cultural**: Optimizado para el contexto legal y empresarial colombiano.

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Este proyecto busca mejorar continuamente para apoyar mejor a entidades que buscan clasificar mejor empresas y personas naturales.

### **Cómo contribuir:**
1. **Fork** el repositorio
2. Crea una **rama feature** (`git checkout -b feature/mejora-clasificacion`)
3. **Commit** tus cambios (`git commit -m 'Agregar nueva regla de clasificación'`)
4. **Push** a la rama (`git push origin feature/mejora-clasificacion`)
5. Abre un **Pull Request**

### **Áreas de mejora prioritarias:**
- Nuevos patrones empresariales
- Optimización de rendimiento para bases de más de 10M de registros. 
- Casos edge específicos
- Mejoras en la documentación
- Tests unitarios

## 📄 Licencia

Este proyecto está bajo la **Licencia MIT**. Consulta el archivo `LICENSE` para más detalles.

## ✉️ Contacto

**Autor**: Néstor Enrique Forero Herrera  
**Email**: enrique.economista@gmail.com  
**GitHub**: https://github.com/EnriqueForero 
**LinkedIn**: https://www.linkedin.com/in/enriqueforero/

---

## 📚 Referencias y recursos adicionales

- [DIAN - Dirección de Impuestos y Aduanas Nacionales](https://www.dian.gov.co/)
- [Registro Único Empresarial y Social (RUES)](https://www.rues.org.co/)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Fuzzy String Matching](https://en.wikipedia.org/wiki/Approximate_string_matching)

---

### 🏛️ **Proyecto desarrollado en apoyo al Gobierno de Colombia**

> *"Mejorando la precisión en la clasificación de entidades para fortalecer las estadísticas comerciales del país."*
