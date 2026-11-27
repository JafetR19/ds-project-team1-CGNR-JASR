# Título del proyecto

## Objetivo del proyecto

Descripción

### Funcionamiento de uv

Para agregar paquetes a uv

```
uv add paquete
```

Para agregar paquetes en grupos

```
uv add --group nombre_del_grupo paquete
```


Para actualizar grupos

```
uv sync --groups nombre_del_grupo
```



Para actualizar grupos

```
uv sync --groups nombre_del_grupo
```


Para actualizar todos los grupos

```
uv sync --all-groups
```
**Nombre del proyecto**: Exploración de datos y trabajo colaborativo

**Integrantes**
Cintia Gisela Nava Roa
Jafet Abimael Sánchez Romero

**Fuente de los datos y objetivo del análisis**: La fuente Incidentes viales reportados por C5 del gobierno de la CDMX, el objetivo del análisis es contar con una visión general del tipo de indicentes que se reportan, desde qué medios y alcaldías. Así como cuanto se tardan en responder y si existen patrones temporales.

**Instrucciones para reproducir el entorno: uv sync** 
-	Primero se debe hacer una copia del repositorio 
-	Poner en la consola ´ uv sync´
-	Después seleccionar en el kernel el ambiente virtual

**¿Qué realiza cada integrante y cómo se conectan ambas partes del análisis?**
La primera persona hizo una evaluación de los tipos de datos, su calidad y algunos pasos de limpieza. Después un perfilado por cada variable que ayudó a identificar categorías únicas y su distribución. Así como algunos valores atípicos.
Partiendo de dichos resultados el estudiante dos se concentró en responde algunas preguntas de análisis. 


**Revisar los notebook en el siguiente orden**
nb01_estructura_perfilado
nb02_preguntas_visualizaciones

## Contexto

En este proyecto analizamos los reportes del C5 desde dos perspectivas. Por un lado, utilizamos el aprendizaje supervisado para clasificar los reportes de acuerdo con su código de cierre. Por otro lado, utilizamos el aprendizaje no supervisado para identificar las zonas donde se concentra una mayor cantidad de reportes mediante DBSCAN. 

Ambos notebooks funcionan de manera independiente, por lo que no es necesario ejecutar uno antes que el otro. Lo importante es que la base de datos se encuentre en la ruta señalada en cada notebook.

## Notebooks

- notebooks/nb03_supervisado_c5.ipynb: se entrenan y evalúan modelos de aprendizaje supervisado para identificar los reportes con código de cierre F. Se incluye la preparación de las variables, la validación temporal y las métricas para evaluar los modelos.

- notebooks/nb04_aprendizaje_no_supervisado.ipynb: se utiliza DBSCAN y la distancia Haversine para identificar agrupaciones geográficas de los reportes. Se prueban distintas combinaciones de parámetros y se analiza cómo se distribuyen los códigos de cierre dentro de los clústeres y entre los puntos considerados como ruido.

## Requisitos

Para este proyecto utilizamos Python y uv para administrar el entorno de trabajo y las librerías.

Desde la carpeta principal del repositorio, se pueden instalar las librerías necesarias con el siguiente comando:

uv add pandas numpy matplotlib seaborn scikit-learn jupyter

En caso de que el notebook de aprendizaje supervisado utilice XGBoost, también se debe instalar esta librería:

uv add xgboost

Si las librerías ya están incluidas en el archivo pyproject.toml y se cuenta con el archivo uv.lock, basta con ejecutar:

uv sync

## Datos
Antes de ejecutar los notebooks, es importante revisar que la base de datos se encuentre en la ruta que indicamos en la celda donde se importa. Si se cambia el nombre del archivo o se guarda en otra carpeta, será necesario modificar esa ruta.

Para el aprendizaje supervisado se utiliza el código de cierre y las demás variables seleccionadas en el notebook. En el caso del aprendizaje no supervisado, las principales variables que utilizamos son la latitud, la longitud y el código de cierre de los reportes.

Si se trabaja con una muestra de la base de datos, es importante mantener el mismo orden de las observaciones para que las etiquetas de los clústeres correspondan a las coordenadas utilizadas en DBSCAN.

## Consideraciones 

- En el aprendizaje supervisado, la variable que buscamos predecir se construye a partir del código de cierre F. Para la validación temporal, los reportes se ordenan por fecha de creación, de manera que cada periodo de prueba se evalúa utilizando información de periodos anteriores.

- En el aprendizaje no supervisado, DBSCAN nos permite agrupar los reportes de acuerdo con su cercanía geográfica. El parámetro eps indica la distancia máxima entre vecinos, mientras que min_samples indica el número mínimo de observaciones dentro de esa distancia, incluyendo el propio punto, para que este sea considerado un punto central.

- Para utilizar correctamente la distancia Haversine, debemos seleccionar primero la columna de latitud y después la de longitud, y convertir ambas coordenadas a radianes. De igual manera, si establecemos el valor de eps en kilómetros, debemos convertirlo a radianes utilizando el radio de la Tierra.

- Los puntos que DBSCAN identifica con el valor -1 son considerados como ruido. Esto no quiere decir que las coordenadas sean incorrectas, sino que esos puntos no quedaron agrupados en ningún clúster con los parámetros que utilizamos.

- Los resultados que obtenemos con DBSCAN dependen de los parámetros que utilizamos. Por ello, es recomendable modificar un parámetro a la vez para entender mejor cómo influye cada uno en los resultados.

- Al trabajar con una muestra aleatoria, tanto los clústeres como el porcentaje de ruido corresponden únicamente a esa muestra. Si modificamos su tamaño o seleccionamos otra muestra, los resultados pueden cambiar.

- El hecho de que un clúster concentre una mayor cantidad de reportes no significa necesariamente que tenga una mayor proporción de reportes con código de cierre F. Por esta razón, también es importante revisar cómo se distribuyen los códigos de cierre dentro de cada clúster.

