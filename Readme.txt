El proyecto actual se centra en la clasificación de la funcion de una proteina según su secuencia de amino acidos, 
posterior a esto se prueba la misma tarea pero implementando PH, inidice de hidrofobicidad y cambios quimicos alpha 
la base de datos fue tomada de kaggle 
en cuanto a las características de los aminoacidos, se diseño una base de datos partiendo de la ya existente conocida
como aaindex1

inicialmente se desarrollo un pretratamiento de los datos, se eliminaron todas aquellas filas que tivieran los espacios
de interes vacios que para el caso del proyecto son clasificación y secuencia. 
posterior a esto se seleccionareon solo aquellos datos que tivieran como clase "proteina" y ademas se seleccionaron las 10
clases con mayor cantidad de datos. 
luego se limito la base de datos para que todas las clases tuvieran aproximadamente la misma cantidad de datos 
en este caso 6978 (que era la cantidad de datos de la clase "virus" y la que menos datos tenia dentro de las 10 
clases con mas datos)
se realizo un proceso de on hot encoding para la respuesta (clases) obteniendo una matriz 
ses tokenizaron las letras de los aminoacidos en las secuencias.
luego de esto se seleccionaba un subcojunto de la secuencia, puesto que tomar la secuencia mas larga llevava a los modelos 
a tiempos de entrenamiento excesivamente largos, y obteniendo precisiones muy malas, razón por la cual se redujeron las
scuencias a 250 datos.

se construyeron una gran cantidad de modelos, puesto que es un problema que se desconocia, inicialmente los modelos 
tenian una gran cantidad de horas de entrenamiento, lo cual esta relacionado con la cantidad de neuronas y la cantidad 
de datos que se introduzca. En el notebook se observan 3 modelos, ya que debido a la no convergencia de la perdida, se modifico
multiples veces la taza y el tipo de optimizador con el que se compilaba. Hasta donde se pudo observar, el optimizador 
adam con una tasa de aprendizaje de 0.05 fue el mas efectivo para este caso. aun asi se probaron los optimizadores SGD
y el mismo Adam con tasas de aprendizaje de (0.1,0.01,0.05 y 0.001(por defecto el del adam))

por otra parte se probo con los 3 tipos de redes neuronales recurrentes vista (Simples,LSTM,GRU) observando que el mejor
comportamiento la tenia la GRU y RNN razón por la cual se dejo de trabajar con la LSTM, aunque no se probo para varias variaciones
en la cantidad de neuronas, luego de esto se procedio a construir arquitecturas con 2 capas,esperando mejorar la respuesta,
estas se encuentran regularizadas mediante la técnica de droop out.

en la entrada de estas redes se implemento el embeding y enmascaramiento puesto que se realizo padding, el tamaño de 
embedging fue de 8 por revisión en la literatura.

finalmente se procedio a realizar un tratamiento de datos con respecto a las características de los amino acidos, 
para esto se descargo la base de datos aaindex y se seleccionaron 3 comportamientos bioquimicos

se construyo la base por aprate puesto que aaindex1 no contiene con claridad estos parametros asi que se buscaron uno 
por uno y se organizaron en una base de datos aparte

posterior a esto se procedio a realizar un codigo para asignar a cada amino acido de cada una de las series estas características
y se contruyo una base de datos para poder realizar el entrenamiento de las arquitecturas.

de igual forma se contruyeron varias arquitecturas y se evaluo la precisión de los modelos. 

Como se observa en el notebook parece ser que a traves de las características el entrenamiento es mucho mejor 
probablementa la secuencia de aminoacidos no sea lo suficientemente clara para permitir separa, mientras que las otras variables 
en conjunto si permiten diferneciar entre funciones con mucha facilidad. 