# PROYECTO MÓDULO 4 MINERÍA DE DATOS

## Integrantes:

- Ana María Barajas Otálora
- Dorys Trujillo Beltrán
- Eduardo Alberto Camargo
- Jenny Consuelo Barrera
- Joan Steven Rigeros Mateus
- Stefania Coronell Solano


# Código para R:

# 1. Exploración: transformación y limpieza

## Cargar librerías

```{r}
library(dplyr)
```

```{r}
library(reshape)
```

Definimos ruta de trabajo

```{r}
setwd('D:/Documentos/CURSOS/DIPLOMADO CIENCIA DE DATOS/Modulo 4/proyecto/mineria_de_datos') 
```

```{r}
glassdoor <- read.csv('glassdoordata.csv',header=T,sep=',')
head(glassdoor)
```

## Contexto y descripción de las variables:

Contexto: El dataset contiene información sobre 1000 empleados de diferentes profesiones, niveles educativos, salarios, edades y departamentos donde se desempeñan.

Problema de negocio: Identificar patrones en la data que permitan explorar el comportamiento del campo laboral, con base en la demanda profesional, el nivel educativo o sus salarios.

Variables:

- jobtitle: profesión, nominal
- gender: género, dicotómica
- age: edad, numérica
- performance: desempeño, categórica ordinal
- education: Nivel educativo, nominal
- department: Departamento donde se desempeña, nominal
- seniority: antiguedad, numérica si se considera como número de años en la empresa
- income: salario, numérica
- bonus: extras, numérica

## Estadística descriptiva

Estructura del conjunto de datos

```{r}
str(glassdoor)
```

Librerías para descripción

```{r}
library("tidyverse")
library("skimr")
library("summarytools")
library("modeest")
library("ggpubr")
```

## Convertimos variables respectivas a factores (si se considera)
```{r}
factores <- c("performance")
glassdoor %>% mutate_at(factores,factor) -> data
```

```{r}
str(data)
```

```{r}
summary(data)
```

Otra forma de obtener un resumen y adicionalmente contar datos faltantes

```{r}
skim(data)
```

## Preguntas exploratorias

Cómo se distribuye la edad?

```{r}
plot(density(data$age))
```

```{r}
boxplot(data$age)
```

La edad tuvo un valor mínimo de 18 y máximo de 65. El valor central corresponde a una edad de 41 años (mediana). El 50% de los datos está distribuido entre 30 y 55 años y tienen una distribución más o menos simétrica.
