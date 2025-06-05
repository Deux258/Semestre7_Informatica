
# Clase 4 (21/03/2025)
## Escalabilidad

Habilidad de un sistema para expandir las necesidades del negocio y cumplir con los requerimientos.

> Los sistemas son dinámicos
> Web 2.0 
> 	Web (primera instancia) los datos eran estáticos

##### Escalabilidad horizontal (scaling OUT)
Aumenta la cantidad de los recursos (más fácil)
##### Escalabilidad Vertical (scaling UP)
Aumenta las capacidades de mi recurso (ej dlc de ram para el pc)

###### Sin estado (Escala Horizontal)
depende de lo que haya pasado anteriormente, es decir, funciona como un simple intermediario (no hace proceso de calculo, simplemente redirige)

Con estado
	Lo que paso antes SI interfiere al proceso X
Define mucho si se puede escalar o no un sistema

### Técnicas para escalar

#### Particionar y distribuir
Dividir un componente y distribuirlo

#### Replicacion (comodin)
Si un sistema se cae, tengo una copia de respaldo
Paralelizar los accesos
#### Cache
Subconjunto de recursos de acceso fácil y rápido (Hay que parametrizarlo dependiendo del contexto)

**Repetición** Si hay repetición, me conviene usar cache (evito reprocesar un mismo proceso)

Si tengo una enorme cantidad de datos, no podre captar el patron de repeticion

Input: q5 q4 q3 q2 q1

| Q1  | Q2  |
| --- | --- |
| Q3  | Q4  |
¿Que hago con q5? se determina con sincronización dependiendo del contexto
Si todas las consultas se repiten lo mismo, no tiene sentido agregar cache

Si está bien configurado, el cache recibe 60-80% de HIT (procesos)


#### Particionamiento

##### Particionamiento Vertical


|     |     |
| --- | --- |
|     |     |
|     |     |
|     |     |

##### Particionamiento Horizontal

|     |     |
| --- | --- |
|     |     |
|     |     |
|     |     |
A

|     |     |
| --- | --- |
|     |     |

|     |     |
| --- | --- |
|     |     |

**Desventaja** Puede que haya una particion con mucha demanda y no se puede predecir al 100% cual será

###### Hash-Based
Índice con hash:   Hidalgo --(encripta)--> 'as912jfsa8'
Volver al origen es muyy caro

**Baja colisión** Para entradas diferentes, la salida siempre es diferente
Cuando hago la transformación, el resultado se distribuye uniformemente 

$$ 0  --|----|----|-----|-----|----|--    2^n  $$

Rompe la semántica de la entrada para poder dividir el proceso, la distribucion no es sesgado
###### Range
Datos particionados por un numero o rango de datos
Problema de desbalance

###### Geographical
Cuando los datosnecesitan ser distribuidos a traves de diferentes ubicaciones geográficas. Reduce latencia

###### Directory o Virtual Bucket
Funciona como un HashMap 
Donde esta la particion? se puede jugar con la reparticion de los datos

#### Replicación
La vara de plata para poder solucionar problemas (no siempre es efectivo)

**Ventajas**
- Permite tolerar fallos
- Accesos paralelos

**Desventajas**
- Costo de la réplica
- Mantenimiento estado
Costo depende de numero de replicas y relacion read/write del sistema


#### Cache


# Clase 5 (25/03/2025)

## Técnicas para Escalabilidad

### *Particionamiento*
Distribuimos las particiones alojadas en diferentes maquinas
_¿Para qué?_ 
- Balancear la carga
	- Hash / CH
	- Range
	- Geográfico
	- Directory
	A partir de esto se pueden generar _Puntos calientes_

Tambien se puede hacer Reshading, refactorizando para la escalabilidad 
Pero puede ser muy costoso en dinero y tiempo
### *Cache*

### *Réplica*
Copia exacta que me permite acceso paralelo
- Necesita técnicas de sincronización
- Permite tolerar fallos
- Balance de carga

####  **Desventaja**
- Costo de la réplica (costo común)
- Mantenimiento estado -> núm de réplicas y Read/Write del sistema 

Si el recurso *NO* tiene *estado*, puedo hacer simplemente copia.
Si el sistema *SI* tiene *estado*, tiene que haber comunicación entre réplicas. Costos de comunicación y sincronización.
Se puede liberar si se tienen modelos más simples.

 Escritura IU(x)>>
Rp  <- R(x)
R1 <- R(x)
R2 <- R(x)

La escritura sólo va al principal (Rp) y luego se coordina con el resto, unificando el estado.
#### **Problemas**
- Ubicación del servidor de la réplica
Trade-off performance
Elasticidad
- Dónde se coloca la réplica
- Actualización de la réplica
Estado
updates síncronos/asincrónicos


#### Para actualizar 
¿Quién inicia la actualización?

***Push**-based protocols:* Replica propaga updates

Envia datos al cliente tan pronto como están disponibles
- Es como tirar carbón ardiendo y lo tiro al Pull para que reciba mi actualización.
- Yo cuando le mando la actu, Pull puede que no tenga recursos para recibirlo

***Pull**-based protocols:* Replica pregunta si hay updates

Cliente solicita los datos cuando esta listo para recibirlos. Controla el ritmo del consumo de mejor forma.
 - Depende del intervalo de actualizaciones, cada cuanto T recibo actualizaciones
- Detiene el proceso de las maquinas para ver si hay actualización o no
- Ofrece mejor escalabilidad

#### Dónde se usan Réplicas

Sistemas de almacenamiento
- Amazon
- Oceanstore
- GFS (gogle Filesystem)
##### GFS Google FIle System
	 Maestro - Esclavo
Pregunta en que server se situa, para luego el cliente conectarse directamente al server X.
Se cae el maestro, cagué. Mientras tenga referencia al server, todo bien. Si no hay maestro, no hay escritura.

### Cache (Cash)
Espacio reducido que almacena información temporal y de acceso rápido.

Concepto explotado : ==localidad==
- Temporal
- Espacial
- Nivel usuario o servidor
- ¿Qué guardar?
	Impacto sobre el rendimiento

*Eficiente* Captando el patrón de repetición para sacarle el jugo al cache.
*Objetivo* Performance, lograr escalar. NO es para captar errores.

- En general son respuestas completas que daría el sistema/servicio

#### ¿Qué guardar?
Se necesitan mecanismos rapidos y eficientes en la seleccion de lo que mantendremos en cache

- **Políticas de remoción** 
Elegir a quién sacar con lógica muy simple (necesito que sea rápido)

- **Políticas de ingreso** 
Complementario. Evita que todo entre al cache.
*EJ)* Si tengo una consulta que parece que no se repite mucho, evito que entre en cache.
Análisis histórico.

- **Tiempo de vida (TTL)**
Debe existir una estrategia de control de obsolesencia %%Minimizar el efecto de recomputo tratando de apuntar a la instancia %%

#### Políticas de remoción
- LRU
- FIFO
- HARMINIC
- MRU
- SIZE
#### Políticas de ingreso
- Mecanismos de control de ingreso
- Tiempo de vida (TTL)
¿Cuánto tiempo mantengo una entrada en cache?


# Clase 6
28-03-25

## Réplicas (Medio resumen)

#### Tolerar fallos

#### Paralelizar accesos
Balance de carga
	Performance
	Throughtput
	
- Ubicación \$$
- Updates copias \$$
Con estado: \$$ 
Sin estado : <\$
- Recursos para réplica \$$

!!:Para bajar el riesgo de sobrecarga
**Push** Asociado a restricciones de tiempo
**Pull** Polling

## Cache / Caching

Objetivo: Mejorar el performance (throughput).
####  *Localidad* 
*Repetición*
	Temporal (Frecuencia)
Cada cúanto repito la pregunta
	Espacial (Relacionado)

%%Gráfica con curva decreciente donde el cache se ubica cuando hay mayor cantidad de datos%%

**QoS Cache:** Calidad de la entrada
	HIT: Si le achunto a la consulta, es un HIT.
	MISS: Si fallo, calculo en backend y es un MISS.
#### *Política Remoción*
¿Qué saco del cache para limpiarlo y que entre otra consulta?

q4? -> %%tabla%%

| q1  | q2  |
| --- | --- |
| q3  | q5  |
q5 si no cumple con los requisitos de patrón, entra q4 y así..
(FIFO, LRU)
#### *Política Ingreso* 
Histórico, Heurístico

**Objetivo:** Disminuir la competencia por el recurso

Si no tengo política de ingreso, esa potencial basura me obliga a reemplazar el cache que tengo "almacenado".
Si el cache es mas grande, menor competividad.

#### *TTL: Time To Live*

Definir tiempo en el cual la entrada va a estar en el cache y al terminar reemplazarlo por otra entrada. 

**Objetivo:** Mantener la frescura de entrada

## Web Search Engine

- Ley de competencias es clave
- Comportamiento de consultas altamente dinámico

¿Cómo funciona un buscador genérico?

##### *Consultas de navegación* (5% aprox)
Su único objetivo es navegar
Son muy superiores en término de frecuencia que el resto

**Mejora**
Una parte de la memoria ==Estática==

| Estático | Dinámico |
| -------- | -------- |
**Estático** Entradas no cambian
Se obtiene histórico a partir de top de búsquedas #q
Si no se repite, perdimos ese espacio.
Si se repite, ganamos eficiencia.
**Dinámico**
Basado en el comportamiento actual.


#### Preguntas

- ¿Por qué será mejor el mecanismo estático-dinámico?
Como hay consultas que se repiten mucho, podemos explotar este tipo de problemas para reducir competencia.

- ¿Qué conceptos explora?
Competencia y repetición a nivel histórico

- Servirá el mecanismo estático por si solo?
Ahora la mayoría de aplicaciones son dinámicos.
Si tengo una lista (por ej) que siempre será el mismo, conviene ser estático

- Si la distribución de consultas cambia, qué ocurre?
Funciona igual. Si aparece otra app popular, estarán en la parte dinámica.
Luego de un par de días se puede ajustar para evitar cambiar la distribución innecesariamente.

#### Preguntas solemne

**Gráfica**

- ¿Por qué al aumentar el tamaño del cache estático aumenta la tasa de Hit Rate?
Veo mayor cantidad de consultas al mismo tiempo
En este caso, aumenta porque puedo ver más consultas repetidas con mayor \% de HIT

- ¿Por qué el cache con LRU tiene peor rendimiento que el cache estático?
Lo que yo saco pierdo
No soy capaz de capturar el patrón de buena manera
El histórico se repite mucho

Se asume que el comportamiento histórico se vuelve a repetir, por eso funciona el estático

Hay mas entradas en el historico en memoria y por eso puedo responder con mayor eficacia.

Cuando hay más espacio, menos competencia

- Explicar 90% estatico 10% dinamico
Se da espacio para consultas históricas y se deja un poco para consultas dinámicas que son mínimas pero no 0

#### Preguntas solemne (desarrollo)

1. Hay muchas consultas únicas -> Siempre generan MISS
2. Mirar cuadro rojo (LRU - SDC / Sizes)
	Hay repetición del histórico.

*Nota:* Si se cambian los tamaños de cache es para normalizar por la cantidad de consultas que hay por región (EEUU puede tenet 50k y UK 100k)

3. Estático responde al histórico.

#### Otras preguntas

1. Falso
Quiero capturar el patrón de repetición.
Si hay mayor repetición de consultas, requiero mayor tamaño de cache (manejo de competencia).
2. Falso
Si todos tienen el mismo tiempo, pueden colapsar todos y generar carga al backend ó caer en cascada. Se trata de generar tiempos aleatorios para cada entrada o tener distribución.
3. Falso
Depende de las consultas nuevas
Depende de la estructura de la entrada
4. Verdadero
Recomputo y frescura
Que no sea un tiempo muy grande (para tener frescura) y ni muy pequeño para evitar recómputo.

# Clase 7
01/04/25


### Paper conferencia resumen

##### Adaptive time-to-live strategies for query result caching in web search engines.
Time to live TTL

##### Abstract

WSE (Web search engine)
Time to live / Cache
	Generan:
Obsolescencia (frescura) --> Tiempo Fijo --> Respuesta obsoleta
									Recómputo
						=TTL Adaptativo

##### Keywords
Search engines, result cache, feshness, time-to-live

##### Introduction

Challenge:
Maintaining the freshness of cached query results without incurring too much redundant computation overhead on the backend search system.

Main reason of freshness problem:
document additions are continuously modified due to new document additions.

Evaluating the queries on backend increases the computational cost.

Techniques for maintaining freshness
- Indexer
- Cache entries

Each entry is associated with a TTL value.
A too large value 
	 stale result 
Small value 
	diminish efficiency gains of having a result cache.

TTL depends on the context in witch it will be implemented

In the paper, they investigate adaptive TTL strategies
	separate TTL value to each query


# Clase 8
03/04/25

## Comunicación

Medio de comunicación es el paso de mensajes por medio de la red.

- Importante analizar cómo diferentes máquinas intercambian información.
- Comunicación por paso de mensajes no es trivial
- Más complejo que manejo de memoria compartida

- Se comunican por medio de una red *no fiable* como internet (todo va por internet)
	- Red saturada
	Usos para los cuales no fue planeada
- Desarrollo de sistemas distribuidos *extremadamente complejo* en estos escenarios

### La Red y Fundamentos de comunicación
Variados dispositivos de hardware 
- Cables, routers, switches, etc
- Medio físico para comunicación.

#### Performance

##### Latencia
Cuánto me demoro en transmitir 1 bit para otro lado
##### Tasa de Transferencia
Velocidad de transmision de datos (bit/seg)
##### Tiempo para transmitir un mensaje

##### Jitter
Variacion del tiempo que toma
Cuál es la desviación de las mediciones de tiempo (cómo se comporta)

#### Tipo de redes
##### LAN 
*Local area Network*
Alta velocidad. Distribuida en segmentos
##### MAN
*Metropolitan Area network*
Alta velocidad en regiones más grandes
Útil para requerimientos de sistemas distribuidos
##### WAN
*Wide area network*
Velocidad menor
Conecta diversas organizaciones físicamente distnates
Latencia depende de la ruta a seguir


**¿Hablamos el mismo lenguaje?**
Codificación de la información
OSI Model (Open System Interconnection Reference Model)

Define reglas para formatos, contenido y significado de los mensajes

#### Protocolos de Middleware
(De arriba para abajo)

Sistema distribuido
Middleware
Operating system Comms
Network

### **¿Cómo se comunican las entidades en un sistema?

#### *Interprocess comunication*
Comunicación de bajo nivel entre procesos distribuidos.
Utiliza primitivas message-passing - Acceso directo a API entregadas por protocolos de internet.

*EJ)* Socket, MPI (message passing interface (framework))
No va codificado de forma adecuada para relaciones más lejanas/complejas

#### *Indirect communication*
Métodos anteriores requieren que emisor y receptor estén presentes al mismo tiempo.

La gracia: Comunicación indirecta con una tercera parte que interviene.

- Funciona como un espacio donde "almacena" el mensaje como si fueran tareas (sistema de colas).
- Emisor y receptor no necesitan existir al mismo tiempo *time uncoupling*
- Cuando funciona el tercero (pizarra) puedo comunicarme con mas de uno a la vez

##### *Message Queues vs Pub/Sub*

Son ideales para lograr que un sistema escale horizontalmente.
- Comunicación es mas durable que la comunicación sincronía tradicional.

##### Message Queues 
Usado para delegar trabajos de un servicio, Sin orden

( x ) -> [][][][][] -> ( x )
		   -> ( y )
##### Pub/Sub
Todos los suscriptores deben consumir lo mismo, al menos 1 copia de lo publicado por el publicador. Mismo orden!
Más costoso

( ) -> ( x )   -> [][][] -> ( ) 
		  -> [][][] -> ( )

> No todo es para todo, considerar consumo de recursos

Si se cae el consumidor, otro consumidor consume el evento.
Una vez recuperado, consume el evento desde su cola de suscripción

**EJ)** RabbitMQ vs Kafka

##### Remote invocation

Paradigma común en sistemas distribuidos.
Intercambio de dos-sentidos (emisor-receptor) entre entidades distribuidas.

Emula a través de una llamada simple, logro mover a un lugar remoto.
>modelo cliente-servidor (C/S).

Cliente ----        ----- (respuesta del servidor)

Servidor       ----

%% DLC %%

- Request/Reply
- RPC
- RMI

- Request/Reply 
	- Patron impuesto a un servicio

- RPC: Remote Procedure Call
	- Modelo que hace request a un servicio alojado en otra maquina en la red sin tener que entender los detalles de esa red.
	- Propiedades
		- Request/Reply sincrono
		- Distribucion transparente
		- garantia de fallas 
			- Congestion de red
			- caida clientes, red o servidor
			- Error entregado a programador
	- Desventajas
		- Atado a C/S
		- Sincrono: Cliente se bloquea al esperar
		- No es posible full transparencia
		- NO es orientado a objetos
- 

##### Remote Procedure [RPC]
Permite el llamado a procedimientos que están alojados en otra máquina.
Llamada de alto nivel (fácil de entender)

**Fallas en RCP**
- congestión de red
- Fallas/caídas de clientes, red o servidor
- Error reply!

###### Stub Cliente/Servidor
Objetivo es generar una llamada a un procedimiento

##### OOM: Java RMI
Llamadas a procedimientos remotos orientado a objetos

*Stub* para el cliente
*Skeleton* para el servidor

##### RPC Asíncrono?
RPC es bloqueante o síncrono
Para poder escalar, se puede optimizar y hacer RPC asíncrono


# Clase 9
08/04/25

## Arquitecturas
Relación entre componentes, dando efecto al cómo yo respondo. 

Me conviene tener buena arquitectura para no depender de un supercomputador y obtener el mismo resultado.

### Tipos o Arquitecturas?

- *Tipo* Asociado a cuál es la finalidad del sistema
- *Arquitectura* organización de los componentes
- *3 tipos de Sistemas Distribuidos*
	1. Computación distribuida
	2. Sistemas de información
	3. Sistemas distribuidos embebidos

### Computación Distribuida

Orientada a la computación de alto rendimiento ==(HPC)==.

- *Clusters* Nodos homogéneos conectados 
en una red rápida
(Más rígido, sistema cerrado) 
	- Se los recursos que dispongo

- *Grid* nodos heterogéneos conectados a una red compartida 
bajo varias administraciones
(Mayor cantidad de recursos, no tan rígido) 
	- Se los recursos que dispongo

- *Peer-to-Peer* Nodos heterogéneos conectados a una red compartida
completamente abierta
(Bordes muy difusos en temas de comportamiento)
	- Sistemas abiertos, no sé los recursos que dispongo

#### Cluster

- Económicamente mejor opción
- Hardware homogéneo
- Cluster vs Supercomputador
- Usados para aplicaciones paralelas. Programa unico corriendo en paralelo en varias máquinas.
- Populares cuando los computadores se volvieron mas tochos

- Generalmente están en un mismo lugar físico

**EJ)** Master node --> N Compute node

#### Grid
Se organiza bajo una misma lógica para compartir recursos

- Heterogéneos (Distintos OS, HW, network, etc)
- Principalmente enfocados a investigación
- Conjunto de máquinas/red a escala mundial o metropolitano
- Arquitectura mucho más avanzada *Menor escalabilidad*
- Entidad semi-cerrado para los que colaboran (redundancia casi completa)
- Son un conjunto de clusters finalmente

##### Grid WLCG
entro global de computadores 
Combina cerca de 1.4 millones de nucleos 

#### Grid VS Cloud

- Cloud público y privado
- Cloud asociado a servicio
- Grid asociado a aplicación (adaptado a tareas específicas)

*Escalabilidad*
Cloud Todo compartido, escalable
Grid: cuesta más escalar

#### Peer-to-Peer
Evaluación de pares -> evaluacion entre la misma categoría. Basado en cooperación
Actúo como cliente y "proveedor". Ubuntu

- Comunicación directa
- Montar algo tiende a ser difícil
- Sistema abierto -> *Completamente descentralizado*

*Ventajas*
- Barato
- No hay rol principal
- Puede escalar linealmente: Mientras más usuarios, más recursos
- Comunicación directa (permitía anonimato)

**Desventajas**
- No es estable (pueden irse todos)
- Problemas de seguridad al ser abierto
- Problemas de latencia
- No hay autenticación


Hay 3 tipos:
###### Estructurada
###### No estructurada
###### Híbrida

Uno de los primeros que sacó provecho es Seti@home
Descomposición de la aplicación en micro-tareas
- Folding@home -> Investigación de enfermedades
- Sistemas de Información
- Sistemas Pervasivos
Saca provecho a los recursos que están cerca de los usuarios (Smartcities)
- Home Systems
Sacar provecho a los recursos de los "mini cpus" de mi casa. Ej: smart tv
- Health
- Smartcities

Cada vez los procesos se quieren acercar más a los que crean datos para hacer pre-procesamiento.

### Arquitecturas Distribuidas
Una buena organización es la clave para alcanzar las propiedades deseadas.

- Arquitectura del sistema
- Centralizadas
- Descentralizadas

#### Cliente/Servidor
Tengo un conjunto de clientes y un servidor
Cuando quieren algo, solicitan al servidor y este da una respuesta de vuelta.

Cliente --->              ----->
	  	|             |
Servidor   --------

- Tengo limite para escalar, *Escalamiento vertical*
- Cuello de botella
- Modelo seguro
- Recursos dedicados
- Mientras más comunicación tenga, mayor riesgo de inseguridad

Para poder escalar, se puede jugar con los recursos.

C/S Existen dependiendo donde se alojan los componentes físicamente.

- 2 tipos de máquina (2-tier)
- 3 de usuario: Interface, aplicación, database

User interface
Application 
- - - - -.
Application
Database

No me conviene que el usuario tenga tanto control sobre el backend/funcionalidad.

- Evitar mucha responsabilidad en el cliente --> *Thin Clients*
- Manejo del cliente es más difícil
	Más sensible a errores

###### Patrones C/S
- Master-slave
- Proxy
- Etc..

# Clase 10
10/04/25

## Peer-to-Peer P2P
Par a Par (iguales)

- Los sistemas que aplican esta igualdad --> ==Peer-To-Peer Puros==
Ninguno tiene mayor responsabilidad que otro

- Clientes/Servidores
	- Escalabilidad Lineal
	Mientras más usuarios, más recursos

- Colaboración
Dispongo recursos en pro del sistema. Normalmente no es tan así.
- Potencial de tener millones de nodos "infinitos"

**Desventajas**
- Poca estabilidad de los pares (Aprox 20% usuarios estables)
Si los tipos no están conectados, pierdo recursos

Entrada/Salida (Join/Leave) **Churm**

#### C/S vs P2P

![[Pasted image 20250410114716 1.png]]

### ¿Por qué P2P?

- Altamente escalables
- 100% descentralizado -> No hay cuello de botella
- *Sólo conocimiento local*
- Tolerancia a fallos (no hay un punto único de falla)
- *!!* Conexión y participantes no confiables (abierta y anónima)

### P2P y Overlay

**Overlay**
Red de computadores construida sobre otra red

De base tenemos una red que no podemos cambiar

Por encima de la red "física":

Peers que se sobrepone a la primera red

![[Pasted image 20250410115506 1.png]]

Si falla la conexión en la red física, el peer que está conectado directamente también.

#### Tipos de Overlay P2P

Estructurados
No Estructurados
Híbridos

##### Estructurado
Sé done buscar cierto recurso para tolerar todo tipo de acción.

- Estructura bien definida (construidas con proceso determinístico)
- Se usan *tablas de hash* distribuidas para su generación.
- Capaz de *escalar* a cientos de millones de participantes

**Desventajas**
- Inseguridad (configurable)
- Latencia alta
###### Tablas de Hash Distribuido (DHT)

Llave, valor a base de punteros. De aquí sé dónde conectarme y cómo.
Corazón de P2P Estructuradas
Al ser altamente dinamico, me conviene tener particionada la tabla con datos repetidos

| Llave | valor |
| ----- | ----- |
Hash f(x) 
- Uniformes: Baja probabilidad de colisión
- Unidireccional
- $2^n$ Hash


"ip"       "destino"
f(x) --> f'(x)      0 ---x--- $2^n$ hash

PERO de f'(x) --> f(x)  *\$\$\$*

Permite distribucion uniforme de los pares y tolerar particiones en este tipo de sistemas
Como rompo la semantica, los pares las direcciono con tablas de hash para que queden distribuidos uniformemente

Bueno para mapeos
	Malo para datos


![[Pasted image 20250410121653 1.png]]


Super eficiente para buscar
Encuentra específicamente 1, si necesito dos datos muy parecidos, tengo que buscar 2 veces

Hash clave
Tabla me permite reducir el numero de saltos que tengo que hacer para llegar a mi peer sin depender de otro peer. Completamente autónomo

Otro peer puede darme otra tabla para poder encontrar más facilmente mi peer

- *Búsquedas completas*
Yo sé donde están las cosas y llego al óptimo. Hay responsables entre espacios que me dan la información de dónde se ubica exactamente el peer

Son Búsquedas puntuales
Asociado a un recurso como tal. 

	Recurso - Identificador Relación 1-1

Mi estrategia para asociar recursos y pares

- asociarlos por cercanía y 

 0 | ----- x ---- ==x --o---== x ---- |  $2^{160}$

==x== responsable de lo que está entre el y el otro peer 

EJ) Quiero buscar capitulos de Game of thrones
Tengo que hacer varias busquedas para encontrar todos los capítulos. Obtengo un capitulo por busqueda a la vez

Yo le pregunto a $log_{10}(n)$ pares, he de ahi que es altamente escalable

Cuando se va un responsable, el más cercano se hace responsable de su espacio. Dinámicamente adaptable

Menos pares, más carga que tiene el peer referencial

*Posible pregunta Solemne*
Yo solo conozco unos cuantos peers y con eso es suficiente para conocer el resto de datos
IMPOSIBLE saber todos los peers que hay en el sistema completo



# Clase 11
15/04/25

## Repaso

### Correcto o Incorrecto?

1. Un buen sistema de cache no solo ayuda a mejorar el rendimiento aumentando su throughput, sino que tambien permite mejorar la tolerancia a fallos y su disponibilidad

Incorrecto: Capta el patron de consulta para reducir interacciones hacia el backlog. Sólo performance. Tolerar fallos es mera consecuencia indirecta

2. Generacion de replicas es la mejor tecnica para escalar
Incorrecto: Depende si el sistema tiene estados. Si tiene estados afecta negativamente por la coordinacion: más costoso en sincronización y mantenimiento. No cumple con esa condicion

3. Particionamiento horizontal de datos o sharding es una tecnica muy usada dado que es idoneo para escalar el sistema horizontalmente
Correcto. sharding es un sistema propicio para escalar horizontalmente.


> [!NOTE] NOTA
> Cuando particiono, no implica que todas las partes tengan los mismos costos.
Si yo se el comportamiento, puedo distribuir acorde a las exigencias.

4. Una forma de escalar cliente-servidor radica en alojar los componentes Podemos afirmar que una implementacion con clientes livianos es mucho mas escalable (en costo para el backend) que una implementacion de cliente fat.
Incorrecto: Los clientes livianos/thin requieren mucho mas backend que los clientes pesados/fat


- Con respecto al modelo de particionamiento horizontal

5. 
Incorrecto: Depende de los recursos que yo uso y el acceso a ellos
No me da garantia de que no se generen puntos calientes porque los datos son dinamicos, no los puedo controlar

6. 
Incorrecto: Distribucion de frecuencia es importante para tomar la decision de qué pongo en el cache. Debo entender qué es lo que se repite para meter en el cache

**Cache** evita recalculo
	
*¿Para qué extiendo el tamaño de cache?*
- Disminuyo competencia

*Si extiendo demasiado el cache*
- Las consultas nuevas siempre generan miss independiente del tamaño
- Tasa de Hit 100% no existe

Cache: *Competencia*
Si hay poco espacio, mayor competencia hay por el espacio en cache

7. 
Incorrecto: La tasa de cambio de estado depende de la consulta, en relacion a su frescura y naturaleza.
Es raro que todas las consultas tengan el mismo Time-To-Live.
El error me genera recomputo a veces de forma innecesaria.
Tiempo unico no me garantiza que el resto de consultas sea igual.

8. as
Incorrecto. Si es altamente dinamica, casi todas las consultas tienen el mismo patron de repeticion por lo que un cache no me resolveria nada.

9.  Comunicacion Indirecta
Emisor --> (espacio dupla/pizarra) -> Receptor
Correcto: la definicion se ajusta a lo que hace el modelo

10. sharling
Incorrecto: Los accesos pueden ser no balanceados/equitativos 
No necesariamente todos tendrian iguales recursos

11. Cache: crucial 
Incorrecto: Parametrizamos en torno al comportamiento del trafico de consultas.
Todo lo que se decide para el cache depende de las consultas y su trafico

12. Arquitecturas P2P
Tengo que ver a nivel de recursos
Si pongo un cache sobre la consulta para evitar la consulta, puede que mi peer quede obsoleto y el cache quede tambien obsoleto
- El peer puede asumir incorrectamente el destinatario, por lo que almacenaria en cache un dato inutil
- La tabla ruta no necesariamente es igual al que tiene mi vecino.

13. Overlays P2P
Incorrecto: Búsquedas eficientes ($log_n$.) No es eficiente si yo miro la latencia
Cada vez que yo salto en el anillo con tabla de hash, cuando voy saltando para encontrar un recurso, estos saltos "lejos" la latencia es bajita. Cada vez que me acerco en overlay, la latencia aumenta dado el aumento fisicamente. Si el salto en overlay es mayor, menor latencia.
Plt Incorrecto: No son Busquedas eficientes dado su latencia

### To Cache or not to Cache?

#### Preguntita 1

1. Cache tradicional:
- ¿Qué condicion debe tener el flujo de consultas que llegan al buscador para que un sistema de cache tenga sentido?
Que haya repeticion. Si no hay, cague

- ¿Qué tecnicas se pueden usar para controlar la calidad de las entradas en un cache como este?
TTL, Politicas de remosion, Politicas de ingreso

- ¿Sera relevante el tamaño de cache en el rendimiento del mismo?
Si. Tamaño que coloque impacta en el rendimiento. Si tengo tamaño pequeño, mayor competencia y contraproducente.

#### Preguntita 2

- Tiene sentido tener un sistema de cache?
Si, tiene que estar enfocado al ranking de enlaces. Entremedio no pq son datos dinamicos previos al ranking.

#### Preguntita 3

tuple space
Emisores(push) que colocan eventos y receptores(pull) que consumen la cola.
Me permite manejar y distribuir el trafico 

- Diferencia entre modelos de espacios de tuplas aplicados a colas distribuidas y punish-subscribe
Receptor consume un evento y el 2do consume el 2do evento.
Suscritores pueden consumir los mismos eventos al mismo tiempo.

- Limtantes en el uso de comunicaciones Remote Procedure Call (RPC)
Cliente servidor: cuello de botella, Asíncrono

### Search Engine Cache

1. ¿por que a medida que pasa el tiempo, el hit rate fluctua incluso cuando cache es infinito?
Consultas nuevas/ que aparecen una sola vez

2. pq no alcanza 100%?
Consultas nuevas

3. Pq a mayor tamaño de cache, mayor % de hit?
Disminuye la competencia

4. ¿Efecto negativo sobre el sistema por tener cache muy grande?
Costo y la frescura de la entrada
Muy probable que no actualice los datos y perder frescura


---

Parte 2
# Clase 12
08/05/25

### Core
Protocolo estructurado de 1ra generacion unidireccional
Define las opciones en el encaminamiento para la tabla de rutas en base a distancia determinada

#### ¿Como se busca?

- Objetivo: Encontrar el sucesor de la clave en cuestión
- Metrica: Cercanía entre identificadores


Distancia numerica
### Pastry
Hoy
Saltos: Largos de prefijos 

Pensado a gran escala

- Organizado en anillo
- TOpoloigia ruteo ==Plaxon-Tree== Árbol de prefijos 
El cómo mido la distancia
- Ruteo basado en prefijo
- Identificador de 160 bits generado con hash SHA-1
- Espacio entre [0 - 2^160]
- Ruteo: log(N) saltos al destino

*Diferencia*
**Core**   --> Distancia numérica
**Pastry** --> Largo de prefijos (Comparte x valor igual al que tengo)

Sigue siendo llave-valor

#### Tabla Ruta Pastry

1. Leafset
2. Routing Table
3. Neighbours set

---
*Mas complejo* pq 
camino bidireccional (Conlleva restricciones)
Me permite tener vecindarios
Soy capaz de tener mayor diversidad entre pares (más opciones)


**Core** Si hay un par que no se comportaba, se nivela por programador

*¿Cómo manejo las copias?*
Sucesor y antecesor
- **Más costoso** Necesito tener info en ambos sentidos (tengo la opcion de salir paralelamente)

> Ventaja de Core frente a Pastry En término de réplicas

- Número de saltos
Cuando busco en core el que responde siempre es el responsable. Son todos posterior
(A nivel de programador)

- Pastry
Cuando me voy acercando, alta probabilidad de pasar antes por la réplica antes que al responsable

---
##### 1. Leafset

- Es un conjunto de copias
- Balancea carga entre copias
- Como balanceo las copias, paralelizo el acceso al recurso

##### 2. Routing Table
Números vacios entre celdas
Empieza vacia la tabla y se va rellenando con pares que empiecen con el valor de la columna en i=0

i=0 (fila 1)
Tiene solamente entradas en esta fila que no comparte ningún prefijo
Las columnas representan el espacio numerico de mis pares

| i=0 | que no tenga ninguna coincidencia | 0-x  | 1-x  | 2-x  | 3-x  |
| --- | --------------------------------- | ---- | ---- | ---- | ---- |
| i=1 | 1 prefijo igual                   | 0-0x | 1-1x | 2-2x | 3-3x |

**Costoso** Rellenar la tabla desde 0
*La gracia que tiene* como tengo varias opciones puedo seguir funcionando
##### 3. Neighborhood set
Puedo categorizar las filas de Routing table a traves del vecino

Por ej tengo prefijo con 1
Soy capaz de elegir filtrando desde 

**Costoso** Tengo que revisar/mantener las entradas si sigue vivo o no
Por el mantenimiento costoso se baneó y ya no se usa 


==!!!!== Entender Pros y Contras de Chord y Pastry


- En cuanto a *Overlay* mantiene las mismas características de seguridad, problemas o LSH


## P2P NO estructurado

Depende netamente del comportamiento del usuario como par
Los recursos asociados a los pares (iniciadores/proveedores) y los que han consumido históricamente 

- Entro y dispongo con mis recursos a otros pares
- Cuando busquemos no hay forma determinística para encontrar el recurso
- Se encuentra por popularidad
- Conocimiento Local 

- Si el par no le hago copia a sus recursos y se desconecta, pierdo la disponibilidad del recurso
	Recursos de tipo *POPULARES*
	*Búsquedas ciegas* Yo no sé donde está

*Velocidad*: Depende a dónde yo me conecto
Si bien es un overlay se pueden dar cercanías entre los pares dado que conectas con cualquiera

- Dependo del vecindario y la popularidad del recurso
	No conozco la estructura de la red

*Para que funcione* ==Flooding Controlado==
- Núm de mensajes que genero 
**VS** 
- Resultados 

Tiene que haber un balance entre cantidad de msjes que genero 
Puedo congestionar la red con miles de consultas esperando respuestas. Para esto, hay técnicas de búsqueda

Como *no hay HASH*, pasamos de consultas puntuales a consultas complejas para poder hacer Match con la peli que estoy buscando

*Ingreso más simple* Escojo un par para conectarme
Tabla de Vecinos: Hay lista de pares conectados (por ej alta conectividad) 

**Desventaja** 
Se generan cuellos de botellas si todos se conectan al mismo par *Puntos HOT*
- Limitar número de conexiones

### ¿Cómo me conecto?
Participante es llamado Bootstrap node
- Lista pre-existentes
- Boostrap cache

**Desventaja**
- Incompletas
- Poco eficiente, ¡Muchos mensajes!
- comparacion de string, gran expresividad
- completamente descentralizada
- Poco efectivo de encontrar contenido NO popular

### Recursos
Se almacenan en peers aleatorios 

- Recursos pueden quedar lejos de los peers
- Max TTL = 7 en Gnutella 0.4
- Para busquedas mas profundas --> Más mensajes
- Caching: Problema de investigación

### Tipos de búsqueda

#### Flooding o inundación de mensajes

- Mensaje enviado a todos los participantes conocidos
	- Poco eficiente
- Núm de mensajes que genero **VS** Resultados 
- Problemas de escalabilidad
- Poco realista, red de millones de usuarios

#### Breath First Search (BFS)
Flooding controlado basado en TTL o routing hops

- Se hace "forward" de la consulta si no hay resultados
- El proceso se repite TTL veces, se manda a TODOS los vecinos
- Mensajes duplicados, gran overhead!

Si un par encuentra un archivo poco popular, el par inicial lo copia para acceder más fácilmente
**Costoso** Duplicación de recursos

#### Iterative Deeping
Hace BFS secuenciales

1. Comienza con búsquedas poco profundas
2. De no encontrar el recurso, itera y prueba mayor profundidad

Se puede optimizar
- Nodos en el camino de profundidad p1 almacena por un tiempo en cache la pregunta
- Si no encuentra, se reenvía a profundidad p2p1

#### Directed BFS
Yo propago a un subconjunto mi mensaje para consultar a mis vecinos

- NO exploto con consultas a todos mis vecinos
- Resultados con menos saltos
- Más mensajes recibidos
- Más vecinos
- Menos latencia, etc

#### Local Indexes
Referencias locales de Dónde están los recursos
Qué archivos tienen mis vecinos 

Problema 
- Mantener la lista de indices dado el dinamismo de peers y datos
- Poco usado
#### Random Walks
Selecciono un vecino al azar
Mucho mas recatado en mensajes, menos agresivo

- Menor probabilidad de encontrar recurso
Sólo si estoy en un vecindario con alta diversidad de recursos
- Aumenta latencia esperando respuestas

#### K-Random Walk
En vez de elegir un solo peer, elijo N pares paralelamente
- Disminuye Latencia

### Parametrizar

TTL
- Dificil setear
- Red dinamica no ayuda

Chequeo resultados
Normalmente cada 4 saltos


# Clase 13
20/05/25
## Super-Peers

Busquedas eficientes a traves de Super Pares.
Pares regulares conectados como si fuera un sistema NO estructurado. 
Mix entre parte estructurada (SP) y No estructurada (P).

Balance entre N de búsquedas, tasas de resultados a costo de escalabilidad del modelo como tal.

*Escalabilidad* Grupo reducido de Super-Peers, depende de ellos.
- Skype: No es un sistema abierto dado que tiene sv para login.

Necesito un mecanismo para detectar peers capaces de ser Super-Peers.

Funciona como cualquier P2P. Asigna a través del par vecino la búsqueda del recurso.
Dinámicamente puede ir cambiando los SP dependiendo de su estabilidad, recursos y/o ancho de banda que disponga el peer para convertirse en Super-peer.

Ya no puedo indexar de manera libre. Tengo que ser mas generalista para los conjuntos de pares.

- ¿Es Beneficioso conectarse al mismo SP?
	No siempre. Anonimato
	Estrategias para asignación dinámica a SP

- ¿Qué nodos son candidatos a SP?
	Se tiene que capturar el comportamiento de los peers para detectar candidatos. Costoso
	Buenas caracteristicas de ancho de banda, estabilidad, recursos.

- ¿Cómo los elegimos?
	Algoritmos de elección


- Funcionamiento entrada/salida sigue funcionando igual que con P2P normal
- Problemas:
	- Asignación SP -> Se crea asignaciones dinámicas
	- Cuello de botella
	- Puntos calientes
	- Eventualmente la escala es limitada. 
		Obligado a conectar un SP, rendimiento depende de la cantidad de SP que tenga.
		Si no tengo un mínimo de pares, no funciona


Básicamente un mix entre P2P estructurado y NO estructurado. Genero una red mas cerrada, menor publico que se incentive a ser SP.
NO es un requerimiento que sea un mix. Puede ser NO estructurado y tener SP.


## Edge Architectures
Arquitectura Edge

Se usa de manera masiva, busca explotar los recursos cercanos al usuario.

Asigna recursos al "borde" en los puntos calientes (físicamente), NO al core para acceder de manera rapida a la copia de borde, permite acceder más rápido al recurso y evita cuellos de botella.

- Gana internet, menor tráfico
- Gana el usuario, acceso más rápido
- Gana ISP, este paga por el acceso al core de internet por lo que menor tráfico, menor costo.

*CDN* Está basado en esta arquitectura de borde

Puede capturar mucho mejor los patrones en vez de tener la lógica en el CORE de forma centralizado.


**Cloud Continium**
Saca provecho de los recursos de Fog, Edge e Internet para Disminuir el tráfico.

- Akamai
El más usado. Provee distribucion de actualizaciones por ejemplo para Playstation con actualizacioens de software.

- Soluciones en los bordes (edge) de la red
- End Users se contectan a edge servers
- Permite optimizar la entrega de contenido
- Se aplican técnicas similares a todo sistema distribuido
	- Réplicas en otros edge servers

Capas:	
	Cloud: Internet
	Edge: LAN/WAN
	Device: usuario

#### Open Connect

- Modelo cooperativo
- Mejor acercamiento para usuarios
- Reduce costos para ambas partes ($1.2B)




# Clase 14
22/05/25

Repaso clase anterior

Super-Peers
Como cualquier sistema, se tienden a mezclar paradigmas para comprobar su funcionamiento.

## Edge Architectures
Arquitecturas híbridas muy usado actualmente.

> Estrategia que permite llevar los recursos a los bordes de internet. 
> Internet: Conjunto de proveedores

**Problema** Internet no fue diseñado para usarse tan masivamente. La cantidad de tráfico es enorme.

Define servicios en el borde de internet de forma que pueda tener la capacidad de proveer servicio de forma más directa sin necesidad de generar tráfico a través del ISP

*Beneficios*
- Gana internet, menor tráfico
- Gana el usuario, acceso más rápido
- Gana ISP, este paga por el acceso al core de internet por lo que menor tráfico, menor costo.

¿Cómo se gestiona? **CDN**
Estudia el contexto de internet/app y como proveedor elige lugares para generar los bordes.
	*EJ)* Akamai: Servidores para uso en general. 250 mil servidores
	Cloud Continium

Capas:
	Cloud: Internet
	Edge: ISP, LAN/WAN
	Fog / Device: usuario


#### Open Connect
Caso de Netflix

- Modelo Cooperativo
- Mejor acercamiento a usuarios
- Reduce costos para ambas partes
	$1.2 Billones en ahorros de conexión

#### Akamai
Si no funciona el server de forma "local", conecta al servidor Padre sin ningun problema.


# Clase 18
27/05/25
## Sincronización
Repaso clase pasada

Cada cierto tiempo estoy obligado a sincronizar
hay diferencia máxima de desfase dependiendo de la app y/o exigencias de la sincronización.
#### Relojes Físicos
- Deriva
	1. Corte Cuarzo
	2. Corriente 
		Mientras más estable, limita los desfases
		f -> ^tics reloj -> desfase

> Se puede sincronizar 2 relojes perfectamente?

NO, dado los métodos de sincronización
- Cliente/servidor (red)
	La precision donde sincronizo los clientes depende de *La red* (tiempos).
	Casi imposible dar con el mismo orden
	Lo que se hace es aproximar $\delta t = \delta t2$ 

Siempre se sincroniza con un aproximado, ==NO exactamente porque es imposible.==

NTP: Tiene STRATAS -> niveles jerarquicos en forma de árbol, donde cada nivel es un Strata

### Relojes Lógicos
Lamport (1978).
Contador de tiempo discreto que sigue ciertas reglas que permite mantener una vara unificada de cómo pasa el tiempo para poder ordenar los eventos.

No es necesario usar tiempo real.

*¿Por qué?* No es necesario ser tiempo absoluto y que todos los procesos estén sincronizados (Procesos que no interactúan).

Lo que necesitamos que concuerden el cómo ocurren los eventos: qué ocurre antes que

%%1. Forma local%%
Cuando tenemos comunicación con el otro proceso.
Lo que nos permite asociarlos radica en la comunicación que hay entre ambos.

- *A ~ B*: A ocurre antes que B
- Todos los procesos concuerdan que A ocurre y luego B

1. Si A y B son eventos en el mismo proceso, y A ~ B, entonces A ~ B es cierto.
2. Si A es un evento enviado por un proceso y B un evento recibido por otro, entonces A ~ B es cierto

P1 ------ A -------
		|
		  | $\delta t$
		   |
P2 --------- B ----

La relación "ocurre antes que ~ " es transitiva
- e || a: Lo único que sé que E ocurrió 

Estas marcas de tiempo son *baratas*. Implicitamente en la comunicación organiza los eventos.

- Afirmar algo acerca del clock no implica nada.
- Importante recordar que el tiempo de reloj C siempre debe avanzar y **nunca retroceder!**

E ~ F --> C(e) < C(f)

Para ajustar, se queda con el máximo de ambos procesos y sumo 1 evento al contador.
EJ) 
	P3 (60)   m3 --> P2(56)
	C(e) < C(f) No se cumple
	Usando el máx de ambos contadores, sumo 1
	P2(61) y continuo

- NO puedo recibir y enviar a la vez (E/R)
- Note que los ajustes se producen en la capa de *middleware*

EJ 2) Base replicada

Capacidad de Bidireccionalidad
Para resolver eventos de Precedencia:

### Reloj de Vector
CV(A)

Cada proceso $Pi$ tiene un arreglo VC_i[n] con n número de procesos involucrados
VC_i[j] es el numero de eventos que el proceso Pi sabe que se han llevado a cabo en Pj

A || E
Lo unico que puedo saber es que son eventos concurrentes
E || D
Concurrentes, ambos occuren antes que f, pero no sé en que momento

# Clase 19
29/05/25

Unificar el orden de los eventos, NO el tiempo. Cómo se ejecutan de manera secuencial.

Relojes físicos, ahora
## Relojes Lógicos

Sólo sincroniza con procesos que realmente es necesario, cuando hay interacción con otro proceso.

- NO se puede retrasar, ni físico ni lógico.

¿Qué acciones ocurren antes de?

P1 ---- A --- B ---->
	
	A ~ B  --> C(A) < C(B)

Cuando hay otros procesos:

			(evento de emisión)
P1 ---- A --- B ----> 
			|
P2 ---------------- C ---->
			(evento de recepción) Si o si tiene que tener un predecesor

	B ~ C --> C(B) < C(C)
	Por lo tanto: A ~ C

Ahora si A y B ocurren en procesos diferentes que No intercambian mensajes:
- A ~ B NO es cierto
- B ~ A tampoco lo es

**Concurrentes ||** No se puede saber el orden de cómo ocurrieron ||

P1 ---- A --- B ----> 
			|
P2 ---- E -------- C ---->

	E ~ C, pero desconozco si ocurrió A antes que E o al revés.
	A || E
	B || E

*Efecto de corrección:*
Para ajustar, se queda con el máximo de ambos procesos y sumo 1 evento al contador.

P1 ---- A (1) --- B (5) ---------> 
			|
P2 ---- E (1) -------- C (*6*) ---->
	
	Max{5,1} + 1

### Totally Ordered Multicast

mantiene cola tanto para P1 (a si mismo) como para enviar a P2 

Cuando recibo la respuesta, organizo mi nocion temporal acorde a la cola recibida de P2.
Una vez que se ordenan las interacciones, si bien hay marcas de tiempo distintas, sigue el mismo orden.
Los N eventos estarán en cada cola (P1 y P2) y se ordena secuencialmente.

