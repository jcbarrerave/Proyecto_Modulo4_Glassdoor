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
