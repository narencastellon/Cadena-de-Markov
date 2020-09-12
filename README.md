# Cadena-de-Markov

#instalar packages
#install.packages("markovchain")
library(markovchain)


# 1. creamos nuestra matriz de transición

#El argumento "nrows" de la funcion matrix es para declarar el numero de filas
#que deseamos que nuestra matriz P posea, y el argumento "byrows" es para que 
#almacene los elementos de la matriz almacenados en c(), fila por fila.

P = matrix(c(0.08,0.184,0.368, 0.368, 0.632,0.368,0,0,0.264,0.368,
             0.368,0,0.08,0.184,0.368,0.368),nrow = 4,byrow = TRUE)
P


# 2. Crear la matriz de transición creamos el objeto "markovchain" de la siguiente forma:

mc = new("markovchain",transitionMatrix=P,states=c("a","b","c","d"),name="Cadena Markov") 
mc
# Resumen de la cadena

summary(mc)

#Dibujamos la cadena
plot (mc, main="Cadena de Markov")



#Otras funciones importantes que nos ayuda clasificar el tipo de cadena
#son las siguientes:

# absorbingStates(): Identifica los estados Absorbentes

# transientStates(): Identifica los estados Transitorios

# recurrentClasses(): Identifica las clases recurrentes

# Veamos si nuestra matriz definida es absorbetes

absorbingStates(mc)

transientStates(mc)

recurrentClasses(mc)

#Análisis probaistico

#Para conocer la probabilidad de transición en 1 paso entre un estado y 
#otro basta con utilizar la función transitionProbability()

#La probabilidad de transicion en un paso del estado "a" al estado "c" es:

transitionProbability(object = mc,t0 = "a",t1 = "c")
mc
#Recuerde que dicha probabilidad es un elemento de la matriz de transición P,
#por lo tanto, la probabilidad de transicion del estado "a" al estado "c" 
#es simplemente P23

mc[2,3]

#Es posible calcular la matriz de transición en n pasos, simplemente calculamos
#la n-ésima potencia de la matriz de transición P, como ejemplo calcularemos 
#la matriz de transición en n = 5 pasos.

n=1000
mc^n

#Tambien se pueden conocer la distribución de la cadena en n pasos adelante
#(P(Xn)) multiplicando la distribucion inicial de X0 por la matriz de transición 
#en n pasos (Pn), calcule la distribución de la cadena en el tiempo n = 6, si 
#la ditribución inicial de la cadena es "(0.4, 0.2, 0.3,0.1)".

X0 = c(0.4,0.2,0.3,0.1) # La distribucion de X en t = 0
n = 6
Xn = X0*(mc^n) # El resultado es un vector
Xn


#Puesto que Xn es una función de densidad, la suma de las probabilidades
#en todos los estados debe ser 1.

sum(Xn)

#Finalmente encontrar la distribución estacionaria de la cadena se obtiene
#mediante la función "steadyStates" de la siguiente forma:

DistEst = steadyStates(mc)
DistEst
