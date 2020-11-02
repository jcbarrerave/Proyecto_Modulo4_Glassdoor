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

### Análisis univariado

Cómo se distribuye la edad?

```{r}
hist(data$age)
```

```{r}
plot(density(data$age))
```

```{r}
boxplot(data$age)
```

La edad tuvo un valor mínimo de 18 y máximo de 65. El valor central corresponde a una edad de 41 años (mediana). El 50% de los datos está distribuido entre 30 y 55 años y tienen una distribución más o menos simétrica.


Frecuencia del género

```{r}
ggplot(data, aes(x = gender))+
  geom_bar(stat="count")+
  theme_minimal()
```

Frecuencia del nivel educativo

```{r}
# Ordenado
# Función para reordenar los factores de un vector en orden decreciente
reorder_size <- function(x) {
        factor(x, levels = names(sort(table(x), decreasing = TRUE)))
}
ggplot(data, aes(x = reorder_size(education)))+
  geom_bar(stat="count")+
  theme_minimal()
```
Frecuencia según desempeño

```{r}
# Sin orden
ggplot(data, aes(x = performance))+
  geom_bar(stat="count")+
  theme_minimal()
```

Frecuencia según la profesión

```{r}
reorder_size <- function(x) {
        factor(x, levels = names(sort(table(x), decreasing = TRUE)))
}
ggplot(data, aes(x = reorder_size(jobtitle)))+
  geom_bar(stat="count")+
  theme_minimal()
```

Frecuencia según el departamento

```{r}
reorder_size <- function(x) {
        factor(x, levels = names(sort(table(x), decreasing = TRUE)))
}
ggplot(data, aes(x = reorder_size(department)))+
  geom_bar(stat="count")+
  theme_minimal()
```

### Histogramas para los ingresos y extras:

```{r}
p1 <- ggplot(data, aes(x=income)) + geom_histogram(color="blue", fill="blue")
p2 <- ggplot(data, aes(x=bonus)) + geom_histogram(color="blue", fill="blue")

ggarrange(p1, p2)
```

Con base en éstas gráfica, se puede observar que un número alto de individuos presenta ingresos alrededor de los 100000 con ingresos extras de 6000. Mientras que unos pocos pueden ganar menos de 50000 o más de 150000.

### Análisis Bivariado

Número de empleados por departamento y desempeño

```{r}
empleados_peformance_department <- as.data.frame(data %>% count(department,performance))
```
```{r}
p <- ggplot(empleados_peformance_department, aes(performance, n, colour = factor(department))) +
  geom_bar(stat="identity")+ylab("Empleados")

p + facet_grid(vars(department), scales = "fixed")
```
Con base en la gráfica no se observa un patrón claro entre el número de empleados y su desempeño por cada departamento.

Número de empleados por género y desempeño

```{r}
empleados_peformance_gender <- as.data.frame(data %>% count(gender,performance))
```


```{r}
p <- ggplot(empleados_peformance_gender, aes(performance, n, colour = factor(gender))) +
  geom_bar(stat="identity")+ylab("Empleados")

p + facet_grid(vars(gender), scales = "fixed")
```

Al parecer hay un mayor número de hombres con desempeño 5 y mujeres con desempeño 1.

Gráficos bivariados Box-plot:

Salario y antiguedad en la empresa:

```{r}
p <- ggplot(data, aes(as.factor(seniority), income))
p + geom_boxplot()
```
```{r}
kruskal.test(data$income, data$seniority)
```
Los salarios fueron diferentes según la antiguedad del trabajador, entre más tiempo lleva en la empresa el salario es mayor y se observaron algunos puntos atípicos.

Salario por profesión:

```{r}
p <- ggplot(data, aes(as.factor(jobtitle), income))
p + geom_boxplot()
```

```{r}
kruskal.test(data$income, data$jobtitle)
```

Entre las profesiones también se presentan diferencias en los ingresos. El gerente y el ingeniero de software tienden a presentar los mayores ingresos. Se presentó un valor atípico muy alto en la profesión de IT (tecnologías de la información) y un valor muy bajo para los asociados a ventas.

¿Cuáles fueron las profesiones mejor pagadas?

```{r}
mean_profesion <- data %>%
                      group_by(jobtitle) %>%
                      summarise(mean(income)) 

as.data.frame(mean_profesion)-> df_profesion
colnames(df_profesion)[2] <- "Promedio"
arrange(df_profesion, -Promedio)
```
La profesión mejor pagada es Manager (gerente), luego está el ingeniero de software. Por último los que tienen menos ingresos son asociado de almacén, conductor y asociado de mercadeo.



