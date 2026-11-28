# Conclusiones conjuntas: Aprendizaje supervisado y análisis geográfico

## Modelo supervisado recomendado

El modelo que se recomendaría con la evaluación semanal podría ser árbol o el random forest ya que ambos fueron los que, en promedio, mejores en las implementaciones que se hicieron. Con una cobertura alrededor del 20% y con las métricas entre los splits siendo relativamente estables. Además, ambos fueron mejores que la línea base, generando mejores resultados que el modelo más sencillo.

Las complicaciones que se enfrentaron para aplicar las técnicas de aprendizaje supervizado fueron en primer lugar que la etiqueta es minoritaria y que tenemos relativamente poca información con la que se puede pronosticar en el momento de la predicción. Esto se tradujo en métricas no tan buenas a pesar de la ingeniería de variables y la implementación de modelos más complejos. 


## Análisis no supervizado

El principal hallazgo geográfico es que, con el clúster número 2, las alcaldías que presentaron un mayor porcentaje de reportes falsos fueron Tláhuac e Iztapalapa, en particular ese bloque mostró un número de casos más balanceado entre clústers. Con el clúster de ruido sin un número tan alto de casos.

DBSCAN ayuda a tener una mejor perspectiva espacial que no es tan fácil de apreciar en la construcción del modelo supervisado. Al agrupa reportes cercanos según su ubicación. Así es más fácil ver si los reportes se concentran en ciertas zonas y después esto podría ayudar en la generación de variables que mejoren el poder predictivo de los modelos. Ayudó a identificar como se distribuyen porcentualmente las falsas alarmas, ya que si analizamos el número absoluto puedes tener un número muy grande, pero no significa que tenga una mayor tasa de falsas alarmas. 


## Limitaciones

En importante reconocer que las métricas semanales se calcularon ordenando retrospectivamente los reportes de cada semana y seleccionando los 100 scores más altos. Este ejercicio supone que se conoce el conjunto de reportes de la semana. No demuestra que el mismo resultado pueda obtenerse al decidir en tiempo real, cuando los reportes llegan de manera continua y todavía no se conoce el resto de la semana.

El principal reto es cómo evaluar en el tiempo real. Nosotros estamos fijando un K de eventos que se van a atender. Pero esos eventos los estamos eligiendo tras haber ya transcurrido toda la semana. Pero la decisión se tiene que tomar en el tiempo real. Como calculamos al inicio del notebook se pueden presentar más de 40 casos de falsas alarmas al día. Lo que implica que en los primeros días pudiésemos "acabarnos" el número de casos que se pueden atender. 

## Siguiente paso antes de una implementación operativa

Consideramos que se podría hacer un tipo de test en pseudo tiempo real. Es decir, entrenamos lo datos del modelo y le vamos dando los "nuevos datos" de forma semejante a como realmente ocurrieron
Durante la prueba, el modelo generaría scores con la información disponible al momento de cada reporte, y así se podría evaluar cómo se puede distribuir la capacidad de revisión ya sea por turno, por hora, por personal, por el número de cámaras. 

También se puede implementar un sistema de fila de casos pendientes en función de alguno de los criterios materiales que tenemos. Después, cuando se compare contra los códigos de cierre, evaluaríamos la eficiencia y la cobertura, así como los tiempos de espera y los casos que quedaron pendientes. Esto permitiría comprobar si el modelo y la regla de priorización funcionan con el flujo operativo real antes de utilizarlos para apoyar decisiones. También se podría realizar alguna clase de prueba piloto en tiempo real para ver si el modelo puede funcionar mejor que bajo el esquema actual.

