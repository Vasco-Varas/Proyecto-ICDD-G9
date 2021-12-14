# Efecto de Twitter en el Bitcoin

## Objetivos:

- El principal objetivo de este proyecto es comprobar si existe una relación entre las publicaciones hechas en Twitter por distintas cuentas (con diferentes dimensiones de repercusión mediatica) sobre el precio de la criptomoneda Bitcoin. Esto considerando distintos tipos de variables como lo son los seguidores, cuentas seguidas y estado de verificación de la cuenta.
- Estimar si es que es posible, luego de que tipo de publicación es ideal el invertir en la criptomoneda Bitcoin para obtener un redito económico.

## Hallazgos:

Para empezar hay que destcar que los principales filtros aplicados a los datos son: tweets con el tag @bitcoin y que solo trabajamos con cuentas con mas de 150.000 seguidores.

### Analisis de sentimientos:

Para el proyecto usamos un algoritmo de analisis de sentimiento sobre los tweets, esto consiste en que el algoritmo determina si es que lo que dice en el tweet es de connotación positiva, neutral o negativa con el fin de poder estudiar como pueden afectar tweets con distinta connotación al precio del Bitcoin. Destacar que siempre existe un margen de error en el que se puede evaluar un comentario positivo como negativo y viceversa.

- Del algoritmo de analisis de sentimiento llegamos a que según este, hay mas tweets de connotación negativa y neutral por sobre los de connotación positiva, dictando una tendencia que influirá en el desrrollo del proyecto.

### Analisis y relaciones entre variables

Se realizó una comparación de todas las variables a estudiar y el valor registrado del Bitcoin cada 5 minutos, 30 minutos, 1 hora, 5 horas, 1 día y 7 días. De algunos de estos graficos y el analisis de ellos rescatamos que:

1. Seguidores vs Delta del valor (1 día), diferenciando verificados de no verificados

   ![Seguidores vs delta 1 día/ verificados](images/followers_1d_verified.png)

   Con esto podemos ver que todas las cuentas con mas de 1.000.000 de seguidores estan verificadas y en principio pensamos que podrían influir en la estimación del valor del Bitcoin.

2. Seguidores vs Delta del valor (1 día), diferenciando tweets con más de dos frases positivas

   ![Seguidores vs delta 1 día/ positivos](images/followers_1d_positive_m2.png)

   En base a este grafico se puede decir que:
   - Las cuentas con más seguidores tienden a no decantarse por manifestarse positivamente con respecto al Bitcoin.
   - Hay cuentas con no tantos seguidores que se manifiestan de forma claramente positiva pudiendo tener posiblemente un impacto positivo en el valor del Bitcoin.

3. Cantidad de tweets vs Delta del valor

   ![heatmap tweets/delta](images/correlations_05.png)
   
   ![scatter tweets/delta](images/tweets_deltaabs.png)
   
   Con esto podemos decir que existe bastante relación entre estas variables lo que nos induce a pensar que si puede tener un rol fundamental en la estimación del valor del Bitcoin

### Modelo de Regresión Lineal

Se crearon varios modelos de Regresión Multilineal con el fin de probar deltas del valor en distintos periodos y los resultados en base a la metrica [R^2](https://sitiobigdata.com/2018/09/03/como-seleccionar-la-metrica-de-evaluacion-correcta-para-los-modelos-de-aprendizaje-automatico-parte-2-metricas-de-regresion/#:~:text=R%C2%B2%20muestra%20qu%C3%A9%20tan%20bien,el%20R%20cuadrado%20ajustado%20disminuir%C3%A1.) fueron los siguientes:

| Delta del valor | R^2     |
|-----------------|---------|
| 5 minutos       | -0.0067 |
| 30 minutos      | -0.0053 |
| 1 hora          | -0.0090 |
| 5 horas         | -0.0091 |
| 1 día           |  0.0029 |
| 7 días          | -0.0036 |

En base a esto podemos decir que el modelo de Regresión Multilineal no es para nada optimo para estimar el valor del Bitcoin con estas variables.

Cabe destacar que este proceso fue replicado con modelos Ridge y Lasso consiguiendo resultados muy similares.

Frente a esta situación decidimos hacer una Regresión Lineal solo con la variable de cantidad de tweets la cual fue la variable que mas podía estar relacionada con el precio, obteniendo un resultado de un R^2 de -0.0209 y si el delta es netamente positivo es de -0.0569. Siendo resultados igual de bajos que los anteriores.

### Modelo K-Nearest Neighbors (K-vecinos mas cercanos)

Frente a los malos resultados con el modelo de Regresión Multilineal, consideramos que el modelo K-NN podía tener sentido con lo que buscamos que es ver como se va a comportar el precio (subir o bajar) y tal vez conseguir mejores resultados, este modelo fue evaluado al igual que el de Regresión Multilineal con distintos periodos de deltas del valor. Los resultados fueron los siguientes:

| Delta del valor | R^2     |
|-----------------|---------|
| 5 minutos       | -0.8571 |
| 30 minutos      | -1.0618 |
| 1 hora          | -1.0656 |
| 5 horas         | -0.9580 |
| 1 día           | -1.0332 |
| 7 días          | -1.0422 |

Se observa que se consiguieron resultados del mismo calibre, siendo este este muy bajo y que no permiten estimar el precio del Bitcoin como esperamos.

Otra vez frente a los malos resultados probamos hacer el modelo pero esta vez solo con un usuario (el que tiene mayor cantidad de tweets) obteniendo los siguientes resultados:

| Delta del valor | R^2     |
|-----------------|---------|
| 5 minutos       | -0.7810 |
| 30 minutos      | -1.1364 |
| 1 hora          | -1.1768 |
| 5 horas         | -1.0842 |
| 1 día           | -0.8420 |
| 7 días          | -0.5463 |

Otra vez no se consiguen buenos resultados a pesar de tomar datos con caracteristicas en común.

Ahora probaremos separar a los tweets por su cantidad de seguidores estableciendo además la varible 'increment' la cual determina si el precio del Bitcoin subió o bajó cuando se publicó ese tweet, obteniendo de resultado un R^2 de -0.8847, siendo todavía un resultado muy bajo.

***En base al estudio de las variables y los modelos creados extraemos las siguientes conclusiones:***

## Conclusiones

- Existe una relación mas estrecha de lo que estimabamos entre la cantidad de tweets publicados por una cuenta con más de 150.000 seguidores que habla sobre el Bitcoin y el precio de este.
- En base a los resultados conseguidos de los modelos de Regresión Lineal y K-NN podemos concluir que con estos modelos y veriables no es posible estimar el precio del Bitcoin o bien el comportamiento de este. Por lo que para una estimación precisa se debería utilizar otras variables y tal vez modelos mas complejos de Analisis de sintimientos o de estimación.
