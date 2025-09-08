# 🎬 Sistema de Recomendación (Amazon Movies & TV)

Este repositorio contiene la implementación de un **Sistema de Recomendación basado en contenido (Content-Based Filtering)** utilizando el dataset de reseñas y metadatos de **Amazon Movies & TV**.  
El proyecto fue desarrollado en **Google Colab** y consolidado en un único notebook.

---

## 📂 Contenido del Repositorio
- `notebooks/SistemaRecomendacion-AmazonMoviesTV.ipynb` → notebook principal con todo el pipeline:
  - Dependencias
  - Descarga del dataset Amazon Movies_and_TV
  - Procesamiento de datos
  - Carga y exploración de los datos
  - Preparación y limpieza de datos
  - Análisis exploratorio    
  - Clase que implementa el modelo (Content-Based Filtering)
  - Construir el modelo
  - Estadísticas
  - Ejemplos
  - Características
  - Evaluación
- `data/README.md` → explica cómo obtener los datos originales (links).
- `requirements.txt` → dependencias mínimas para ejecutar el notebook.  
- `README.md` → documentación del proyecto **Sistema de Recomendación basado en contenido (Content-Based Filtering)**.  

---

## 🎯 Objetivo
Construir un sistema de recomendación que sugiera productos (películas y series) a los usuarios mediante un enfoque **basado en contenido**, utilizando técnicas de **TF-IDF** sobre títulos, descripciones y categorías, y calculando similitudes con coseno.

---

## ⚙️ Preparación de Datos
1. Descarga y carga de datasets comprimidos de reseñas y metadatos (formato JSONL `.gz`).  
2. Limpieza y normalización:
   - Conversión de IDs (`user_id`, `item_id`).  
   - Manejo de valores nulos.  
   - Creación de campo `combined_features` = `title` (peso 3) + `categories` (peso 2) + `description`.  
   - Normalización de texto (minúsculas, eliminación de caracteres especiales).  
3. Intersección de reseñas y metadata:  
   - **276,165 reseñas**  
   - **74,978 usuarios**  
   - **4,558 productos**  

---

## 🔎 Análisis Exploratorio
- **Ratings**: promedio 4.28, mediana 5.0, moda 5.0 → sesgo hacia calificaciones positivas.  
- **Usuarios**: mayoría con pocas reseñas (media 3.7), algunos “súper usuarios” con hasta 713 reseñas.  
- **Productos**: distribución tipo *long tail*, algunos con más de 1,300 reseñas.  
- **Sparsity**: matriz usuario–producto con ~99.9% de celdas vacías.  
- **Temporalidad**: crecimiento exponencial de reseñas entre 1998–2014, con ratings promedio estables (~4.2–4.4).  

---

## 🤖 Sistema de Recomendación
### Enfoque: *Content-Based Filtering*
- Representación de ítems con **TF-IDF** sobre `combined_features`.  
- Vectorización con hasta 3000 términos (ngrams unigramas y bigramas).  
- Cálculo de similitud coseno entre ítems.  
- Construcción de perfiles de usuario a partir de ítems con ≥4 estrellas.  
- Recomendaciones personalizadas: top-k ítems más similares a su perfil.  
- Fallback: recomendación de ítems populares si el usuario no tiene historial suficiente.  

---

## 📊 Evaluación y Resultados

### Ejemplo de ítem
- Base: *Santa Claus Is Comin to Town [VHS]*  
- Ítems similares sugeridos: *Santa Claus Conquers the Martians*, *The Year Without a Santa Claus*, etc.  
- Palabras clave que explican similitud: `santa`, `claus`, `vhs`.  

### Ejemplo de usuario
- Usuario con 8 reseñas (promedio 4.88).  
- Categorías preferidas: *Movies & TV*.  
- Recomendaciones generadas: títulos de temática navideña y similares a los mejor valorados.  

---

## ✅ Evaluación

1. **Calidad de representación**  
   - El campo `combined_features` capturó semántica básica de los ítems.  
   - TF-IDF (3000 términos) generó una matriz de **4558 × 3000**, adecuada para discriminar similitudes.  

2. **Resultados coherentes**  
   - Ítems similares mantienen coherencia temática (ejemplo: películas navideñas).  
   - Recomendaciones alineadas al perfil del usuario y explicables con features clave.  

3. **Limitaciones y retos**  
   - **Sparsity extremo (~99.9%)** y sesgo hacia popularidad.  
   - Escalabilidad limitada al calcular toda la matriz item–item.  
   - Se recomienda explorar híbridos con filtrado colaborativo.  

---

## 🚀 Próximos pasos
- Desarrollar versión híbrida (colaborativo + contenido).  

---

## 📌 Requisitos
Instalar dependencias mínimas con:

```bash
pip install -r requirements.txt
