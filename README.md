# 📊 Análisis de comportamiento y segmentación de clientes | ConectaTel

## 🎯 Objetivo del proyecto

Analizar el comportamiento de los clientes de una compañía de telecomunicaciones a partir de información demográfica, planes contratados y registros de uso, con el propósito de identificar patrones de consumo, segmentar usuarios y detectar oportunidades comerciales.

El análisis busca responder principalmente:

- ¿Cómo se comportan los usuarios según su edad y nivel de uso?
- ¿Existen diferencias relevantes entre los planes Básico y Premium?
- ¿Qué usuarios presentan patrones de consumo atípicos?
- ¿Qué segmentos pueden representar oportunidades comerciales para ConectaTel?

---

## 📁 Datos utilizados

El proyecto utiliza tres datasets:

### `users`
Información a nivel cliente:

- `user_id`
- edad
- ciudad
- fecha de registro
- fecha de churn
- plan contratado

### `usage`
Registros de utilización de los servicios:

- `id`
- `user_id`
- fecha
- tipo de servicio (`call` o `text`)
- duración de llamadas
- longitud de mensajes

### `plans`
Características de los dos planes comercializados por la compañía:

- Básico
- Premium

La relación principal entre las tablas se realizó mediante `user_id`, mientras que la información de planes puede relacionarse mediante el plan contratado por cada usuario.

---

## 🛠️ Herramientas utilizadas

- Python
- Pandas
- Matplotlib
- Seaborn
- Google Colab
- GitHub

---

## 🔎 Proceso de análisis

### 1. Exploración de datos

Se revisaron:

- estructura y dimensiones de los datasets;
- tipos de datos;
- valores únicos;
- valores ausentes;
- sentinels;
- duplicados;
- consistencia lógica entre variables;
- rangos y distribuciones.

### 2. Calidad y limpieza de datos

Durante la exploración se identificaron distintos problemas:

- `age` contenía el sentinel `-999` en aproximadamente 1.37% de los registros.
- `city` contenía `"?"` en 96 registros, utilizado como indicador de información desconocida.
- `reg_date` contenía 40 registros correspondientes a 2026, fuera del periodo esperado.
- `usage.date` contenía 50 fechas inválidas, aproximadamente 0.13% de los registros.
- Los valores nulos de `duration` y `length` se identificaron principalmente como nulos estructurales asociados al tipo de servicio.
- Se detectaron 28 excepciones a esta estructura: 12 registros `call` con información en `length` y 16 registros `text` con información en `duration`.

Las decisiones de limpieza se realizaron considerando el significado de cada variable, evitando eliminar o imputar información de forma automática.

---

## 🔗 Integración de datos

Los registros de `usage` se agregaron a nivel usuario para obtener:

- cantidad de mensajes;
- cantidad de llamadas;
- minutos acumulados de llamada.

Posteriormente, estas métricas se integraron con la información disponible en `users` mediante `user_id`, generando un perfil consolidado por cliente.

---

## 📈 Análisis exploratorio

Se analizaron las distribuciones de:

- edad;
- cantidad de mensajes;
- cantidad de llamadas;
- minutos de llamada.

La edad presentó una distribución relativamente uniforme y sin valores atípicos visibles.

Las variables asociadas al consumo presentaron distribuciones sesgadas hacia la derecha: la mayoría de los usuarios mantiene niveles bajos o moderados de actividad, mientras que un grupo reducido registra niveles considerablemente superiores.

La comparación visual entre los planes Básico y Premium no mostró diferencias marcadas en la forma de las distribuciones analizadas.

---

## 🚨 Identificación de outliers

Los valores extremos se analizaron mediante boxplots y el método IQR.

### Cantidad de mensajes

- Q1: 4
- Q3: 7
- Límite superior IQR: 11.5
- Máximo observado: 17

Los usuarios con 12 mensajes o más son considerados estadísticamente atípicos.

### Cantidad de llamadas

- Q1: 3
- Q3: 6
- Límite superior IQR: 10.5
- Máximo observado: 15

Los usuarios con 11 llamadas o más son considerados estadísticamente atípicos.

### Minutos de llamada

- Q1: 11.12
- Q3: 31.42
- Límite superior IQR: 61.86 minutos
- Máximo observado: 155.69 minutos

Esta variable presentó los valores extremos más pronunciados.

Los outliers no se interpretaron automáticamente como errores, ya que pueden representar clientes reales con un nivel de consumo intensivo.

---

## 👥 Segmentación de clientes

### Segmentación por edad

Los usuarios se clasificaron en:

- **Joven:** menores de 30 años.
- **Adulto:** entre 30 y 59 años.
- **Adulto Mayor:** 60 años o más.

El segmento Adulto concentra la mayor cantidad de clientes, seguido por Adulto Mayor y Joven.

### Segmentación por nivel de uso

Se definieron tres grupos:

- **Bajo uso:** menos de 5 llamadas y menos de 5 mensajes.
- **Uso medio:** menos de 10 llamadas y menos de 10 mensajes, después de excluir los usuarios clasificados como Bajo uso.
- **Alto uso:** resto de los usuarios.

La mayoría de la base se concentra en **Uso medio**, mientras que Alto uso representa el segmento de menor tamaño.

---

## 💡 Principales hallazgos

1. **El cliente típico presenta un consumo moderado.**  
   El segmento Uso medio concentra ampliamente la mayor parte de los usuarios.

2. **Existe un grupo reducido de usuarios intensivos.**  
   Aunque algunos registros son clasificados como outliers mediante IQR, sus valores son plausibles y pueden representar clientes con necesidades de consumo distintas.

3. **La edad no parece explicar por sí sola el comportamiento de consumo.**  
   Los patrones observados sugieren que la intensidad de uso constituye una dimensión más accionable para la segmentación comercial.

4. **Básico y Premium presentan distribuciones de comportamiento similares.**  
   Visualmente no se identificaron diferencias suficientemente marcadas para concluir que pertenecer a uno de los planes determine por sí mismo una mayor intensidad de uso.

---

## 💼 Recomendaciones de negocio

### 🎯 Desarrollar oportunidades de upselling

Analizar dentro del segmento de Uso medio a los usuarios que se encuentren próximos a los niveles de Alto uso y evaluar su plan actual. Estos clientes podrían representar candidatos para migraciones hacia Premium.

### 📱 Profundizar en usuarios de Alto uso

Los clientes con mayor número de llamadas, mensajes o minutos pueden tener necesidades diferentes al usuario promedio. Se recomienda analizar su distribución entre Básico y Premium y evaluar si la oferta actual satisface adecuadamente su comportamiento.

### 📉 Investigar clientes de Bajo uso

El bajo consumo puede representar clientes con necesidades reducidas, pero también usuarios con menor involucramiento con el servicio. Se recomienda estudiar este segmento junto con variables de churn y plan contratado.

### 👤 Utilizar comportamiento antes que edad para segmentar

La edad aporta contexto demográfico, pero los patrones de llamadas, mensajes y minutos ofrecen señales más directamente relacionadas con la utilización del servicio y, por lo tanto, con posibles oportunidades comerciales.

---

## ✅ Conclusión

El análisis permitió transformar registros individuales de clientes y eventos de uso en segmentos interpretables para el negocio.

La principal oportunidad identificada se encuentra en utilizar los patrones de consumo como base para una segmentación comercial más precisa. Los usuarios de Uso medio representan una oportunidad relevante por el tamaño del segmento, mientras que los usuarios de Alto uso pueden resultar estratégicos debido a su intensidad de consumo.

Los valores extremos detectados no deben eliminarse automáticamente, ya que pueden representar clientes reales de alto consumo. En futuras etapas sería conveniente complementar este análisis con métricas de ingresos, costos y churn para determinar si los segmentos de mayor utilización también representan una mayor rentabilidad o riesgo de abandono.

---

## 📚 Aprendizajes del proyecto

Este proyecto permitió aplicar conceptos de análisis de datos con Python como:

- exploración y diagnóstico de datasets;
- tratamiento de valores ausentes y sentinels;
- conversión y validación de fechas;
- identificación de nulos estructurales;
- agregaciones mediante `groupby()`;
- integración de datasets mediante `merge()`;
- creación de funciones;
- segmentación mediante condiciones lógicas;
- visualización mediante histogramas, countplots y boxplots;
- detección de outliers mediante IQR;
- interpretación de resultados desde una perspectiva de negocio.

---

## 👤 Autora

**Evelyn Miranda Sevilla**

Proyecto desarrollado como parte de mi formación práctica en análisis de datos con Python.
