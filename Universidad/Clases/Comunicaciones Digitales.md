# Clase 5  
(24/03/2025)
## Codificación de Hamming


### Codificación Sistemática

Con solo ver la salida de un codificador, podemos separar los datos y los bits redundantes/paridad. 
### Codificación NO Sistemática

Los bits redundantes/paridad se intercalan.

Algoritmo_
- Posiciones potencia de 2 
- Resto de posiciones son los datos a transmitir

**Ejemplo** (11,7) 
11 bits en total / 7 de datos
Datos = 0101001


Paridad *P* (Potencia de 2)
Datos *D* = 0101001 (EN D)
Se enumeran 11 posiciones con 4 bits (16),  _4 bits de paridad_

|                   | p1   | p2   | d1   | p3   | d2   | d3   | d4   | p4   | d5   | d6   | d7   |
| ----------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Posición          | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   |
| Posición (BI)     | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 | 1010 | 1011 |
| Palabra original  |      |      | 0    |      | 1    | 0    | 1    |      | 0    | 0    | 1    |
| p1                | 1    |      | 0    |      | 1    |      | 1    |      | 0    |      | 1    |
| p2                |      | 0    | 0    |      |      | 0    | 1    |      |      | 0    | 1    |
| p3                |      |      |      | 0    | 1    | 0    | 1    |      |      |      |      |
| p4                |      |      |      |      |      |      |      | 1    | 0    | 0    | 1    |
| Palabra + Paridad | 1    | 0    | 0    | 0    | 1    | 0    | 1    | 1    | 0    | 0    | 1    |
1. Insertamos la palabra en las posiciones d
2. "Bajamos" los bits de datos con LSB de posicion en 1 (xxx1) 
	SI HAY 1, BAJO
	Calculo paridad (hay 3 1s sin contar p1) Por eso _se colora 1 en p1_
3. Posicion en xx1x BAJA el bit de dato
	Ya se calculó p1, se baja hasta p2
	Hay 2 bits de paridad _0 en p2_
4. Posicion en x1xx
Hay 2 bits de paridad _0 en p3_
5. Posicion en 1xxx
Hay 1 bit de paridad, _1 en p4_
6. Deje caer todo _10001011001_

___

El código Hamming binario lineal en categoria de _codigos de bloques lineales_ 
_Puede corregir errores de 1 solo bit_

Por cada entero P >= 3 (num de bits de paridad), existe un codigo de Hamming (n,k)

**n** Tamaño total de bloque/palabra
**k** Num simbolos de info que el codigicador puede aceptar a la vez

##### Características de código de Hamming generico (n,k)

![[Pasted image 20250324150952.png]]

**EJ)** Codificador Palabra 1011

n =7
k = 4
n-k = 3

| D1  | D2  | D3  | D4  | P1  | P2  | P3  |
| --- | --- | --- | --- | --- | --- | --- |


![[Pasted image 20250324151110.png]]

Le chantai circuito
![[Pasted image 20250324151356.png]]

Matriz generadora [K x N]

*K = 4* filas
*N = 7* columnas

[G] = [I_k : P]
[I_k] Matriz identidad [K x K]
[P] Matriz [K x P] que contiene coeficientes de paridad (4 filas, 3 Paridad)

![[Pasted image 20250324151825.png]]

Si no interviene en P paridad, le coloco 0
Si interviene en P el dato D, le coloco 1

1ra fila: D1 
2da fila: D2
3ra fila: D3
4ta fila: D4

![[Pasted image 20250324153047.png]]

Para combinaciones intermedias, sumo entre filas para dar con el resultado

**EJ)** 1101 -> Sumo F1, F2, F4
Paridad lo mismo, da 010

# Clase 6: Matrices Codificación
(27/03/25)

Retomando la clase anterior:
![[Pasted image 20250327144501.png]]


![[Pasted image 20250327144522.png]]

Si la suma vertical es impar, el lado derecho de la matriz = 1
Si es par = 0

EJ) 
0100   011
0010   101
0001   110
=0111 / 000
### Matriz de Corrección de error (H)
$$ [H] = [P^T : I_P] $$
![[Pasted image 20250327145012.png]]

Se escribe en "Horizontal" cuando P1 esta conectado con Dx

#### Probabilidad de Error

$P_e$ =  Probabilidad de error de 1 bit
$R'$ = Cantidad de errores
$n$  = Longitud de bloque
Buscamos la probabilidad de tener más de $R'$ Errores en un bloque de $n$ dígitos

$$ P (e > R' errores) = 1 - P(e < R' errores) $$
Hasta sumar $n$ cantidad probables de errores
Se calcula individualmente

Un bloque que representa una palabra de codigo se divide en _elementos_ desde _1_ a _n_
Cada _elemento_ corresponde a un dígito en una palabra de _n_ dígitos, se le asigna la probabilidad que le corresponde

Caso general: $j$ errores en $n$ dígitos nos da (binomial)

![[Pasted image 20250327145811.png]]

Podemos reescribir la sumatoria anterior en:
![[Pasted image 20250327145858.png]]

Si hacemos _n_ muy grande, la cantidad de errores en un bloque tiene a $P_en$
%%Bruh,%%

**EJ)** La probabilidad de un error simple es 0.01, entonces la probabilidad de recibirlo correctamente es 0.99.
Calcular la probabilidad de 0 a 2 errores que ocurran en una palabra de 10 dígitos.

Probabilidad de no tener errores en el bloque
$$ (0.99)^{10} = 0.904382 $$
![[Pasted image 20250327150137.png]]

Siendo todas las repeticiones de bits independientes.

- Probabilidad con 1 error.

$$ P (1error) = (0.01)^1 * (0.99)^9 * combinatoria^{10}C = 0.091352$$

combinatoria (n k) = n! / k! (n - k)!
(10 1) = 10! / 1! (10 - 1)!

10 posibles combinaciones donde colocar 1

- Probabilidad con 2 errores.
$$ P (1error) = (0.01)^2 * (0.99)^8 * combinatoria^{10}C = 0.00415$$
45 posibles combinaciones donde colocar 45

> Mientras más errores haya, más raro es. Tiende a 0

### Códigos Lineales de Grupo

Toda palabra se puede expresar como combinación lineal. 
Es decir, a partir de palabras existentes.

Contienen la palabra con todos ceros
Si se toman dos palabras Ci y Cj entonces: _(OR exclusivo)_

![[Pasted image 20250327151223.png]]

#### Ejemplo 
alfabeto de 4 miembros, a,b,c,d
- Se codifican en palabras de 5 dígitos (n=5)  
- El código es (5,2)  
- Si sumamos las palabras de c y b, obtenemos d

Generados por polinomio 
Capaz de mover bits de izquierda a derecha.


# Clase 7

### Performance o Eficiencia de un código

Se mide considerando todas las distancias de Hamming entre pares de código.
- La que cuenta es la distancia mínima.

En el ejemplo

Supongamos que una palabra *Ci* es cofundida por *Cj*

- La probabilidad depende de la distancia entre ambas: *Dij*
- La distancia equivale a una palabra *Ck*, suma módulo-2 de *Ci* y *Cj*
- La probabilidad

Básicamente ver cuántos bits cambiaron desde la palabra original a la palabra transmitida buscando el vecino más cercano que haya cambiado

**EJ)**

Ci = 11010
Cj = 10010

Si sumo las palabras por paridad:

Ck = 01000

Si la distancia minima entre 1 (unos) en Ci es igual al peso de Ck

### Capacidad de corrección de errores

t maxima posibilidad de corregir todos los patrones de error de t o menos errores.

Distancia de Hamming:
$$ T = int (\frac{Dmin -1}{2}) $$
Donde: $$Dmin - 1 = e + t$$
*int* es parte entera, *e* es la cantidad de errores detectables (incluyendo los t corregibles)
y *t* <= *c*
*R* = Eficiencia

**EJ)** 
n = 63
K = 57
n-k = 6 bits de paridad
R = k/n = $57/6 = 0,9$   
	R = 0,9 (dividr la cantidad de bits de datos / bits totales)

**EJ 2)**
n = 63
k = 45
n-k = 18
R = k/n =  45/63 = 0,71

> [!NOTE] Propiedad y DCH
> 
> Longitud de bloque n = $2^n - 1$
> m = 3,4,5..
> Distancia mínima d $\geq$ 2t + 1   
>  Núm de Bits de información k $k$ >= n - mt

### Proceso de decodificación

> Ver matriz H 

Corregible 1 bit
Detectable 2 bits

La técnica da 1 sola posición 
No puedo corregir 2 bits con el unico bit de corrección, pero puedo detectar que hubo un error.


### Decodificación por síndrome

El método de buscar el vecino más cercano/parecido.

#### ¿Cómo se codifica? técnica visual

- Dato 1001
Palabra x matriz
[1001] (MATRIZ G) = [1001001]

C 1x4 x G 4x7
igual a 1x7

fila x columna (OR exclusivo) + fila x columna (OR exclusivo) + ...

- Aparecen el dato y sus bits de paridad correspondientes

- llegó la palabra [1001001]
- Para verificar si llegó bien (errores):

Matriz H * Palabra 
H3x7 * P7x1 

fila x columna (OR exclusivo) + fila x columna (OR exclusivo) + ...

= [000] Palabra SIN error!

**Caso contrario**

[1001001] Original
[1011001] Error

Matriz H * E = [101]

Al no dar todos 0, palabra CON error!

- Luego, ese valor 101 lo busco en las columnas de la matriz H

*Columna azul (3)* posición N°3 donde hubo error y cambio el bit care palo que está mal

##### En resumen

Un código agrega redundancia para detectar y7o corregir errores en una secuencia de bits

La distancia de Hamming mide el grado de similaridad entre dos secuencias de bit del mismo largo

La matriz de corrección...

# Clase 10 
14/04/25:
## Clase de ejercicios

- 20 preguntas de alternativas
- 2-3 de desarrollo

### Preguntas modelo

Revisar pdfs de las clases

1. En un sistema digital:
El ruido NO se acumula de repetidor a repetidor en sistemas de larga distancia (Por qué no aprovecho de mejorar la onda con repetidor?)

2. El organismo internacional que regula las telecomunicaciones es:
La International Telecommunications Union

3. La cantidad de errores detectables es:
Mayor o igual a los corregibles

4. El peso de una palabra de código binaria esta definida por la cantidad de
Unos que tiene

5. La propagación por Linea de Vista se produce cuando la frecuencia es:
Mayor a 30 MHz

6. Para lograr una velocidad de informacion con una tasa de error que se aproxime a cero
Debe cumplirse con la formula de Capacidad de Shannon
- capacidad del canal directamente proporcional a capacidad de datos a procesar

7. En una señal con frecuencia mayor a 30 MHz
El horizonte es un obstáculo
(si quiero llegar más lejos, más tengo que elevar la antena para transmitir a larga distancia)

8. La relación de eficiencia R de un código está dado por
$k/n$ 
k: cantidad bits n: tamaño palabra
- (n-k) = Bits de paridad

8.  La medida de la info depende solo de la
Probabilidad $P_j$

9. La relaión señal a ruido en la fórmula de capacidad de Shannon debe escribirse como:
Una expresión lineal (Watt/Watt)
- Medida Adimensional

10. La técnica ARQ consiste en
La retransmisión de un paquete con error
- Es flojo, no corrige

11. Un código lineal de grupo
Contiene la palabra nula 
Si K = 4 -> $2^4$ = tamaño de palabra (32)

12. La longitud de onda de una señal es 
Inversamente proporcional a la frecuencia de la señal
$\lambda = \frac{C}{f(Hz)}$  

13. La decodificación por síndrome se basa en la desigualdad en donde la pro de $t$ errores es:
Mucho mayor que la probabilidad de t+1 errores


### Pregunta de Desarrollo: Radioenlace

Dos escenarios: para 433MHz y 710MHz probar casos, NO que se comunican en esas frecuencias

Datos:
Distancia = 90Km
h1 = 350m
h2 = 120m
f1 = 433MHz
f2 = 710MHz
S1 = -90dBm
S2 = -105dBm
P1 = 0.25W
P2 = 0.25W
G = 14dB

1. Pasar G a W
2. La pregunta esta orientada a la distancia, despejo la distancia

*Sensibilidad* capacidad del equipo (potencia) para procesar una señal entendiendo el mensaje que le está llegando
- La mínima potencia es la S para captar y procesar la señal

2 verificaciones: 
- Línea de vista 
para la distancia que establece, curvatura de la tierra
- Potencia de equipos
que sea suficiente para dar alcance segun sea requerido

Este caso, potencia de equipos.

R: ambos equipos funcionan joya dado que dan dobra la distancia

Ahora, se debe cumplir que se veam por sobre el horizonte 

d <= $\sqrt{2rh_1}$ + $\sqrt{2rh_2}$

d <= 77122,6m + 45158,4m = 122,3km
122,3km > 90km

R: Cumple joya

> NO tiene nada que ver con las frecuencias

Si no alcanza, pongo una antena al medio o aumento la altura de las antenas

### Pregunta de Desarrollo: Codificación

d = 3
d -1 bits = 2 detectables
1 corregible

si H x matriz nula = (000) joya: toda matriz tiene palabra nula

Sindrome = H transpuesta 

EJ:

H =
101
111
011

Matriz H:

| 1   | 0   | 1   | 1   | 1   | 0   | 0   |
| --- | --- | --- | --- | --- | --- | --- |
| 1   | 1   | 1   | 0   | 0   | 1   | 0   |
| 0   | 1   | 1   | 1   | 0   | 0   | 1   |
Paso a S (cambiaso):

| 0     | 0     | 0     |                                   |
| ----- | ----- | ----- | --------------------------------- |
| 1     | 1     | 0     | Elemento 1 de la palabra original |
| 0     | 1     | 1     | Elemento 2 " " " "                |
| 1     | 1     | 1     | Elemento 3 " " " "                |
| 1     | 0     | 1     |                                   |
| 1     | 0     | 0     | ..                                |
| **0** | **1** | **0** | Cambio el bit 6                   |
| 0     | 0     | 1     |                                   |


Si tengo la palabra 001111 y tengo como respuesta (**010**) al multiplicar matriz H x palabra (matriz),
busco la fila **010** 

0001111
a
00011**0**1

yera.


# Clase 13
12/05/25

## Modulación por Codificación de pulsos PCM

- Cuantización
- Muestreo
- Error
- Codificación


> PCM Posee 3 operaciones básicas:

Señal de entrada con tiempo y amplitud 
continua
|
###### 1. Muestreo *PAM SAMPLER*
Señal con tiempo discreto 
Amplitud continua  
**Pulsos PAM**
|
###### 2. Cuantificación *Quantizer*
Señal con tiempo y amplitud continua
**Pulsos PCM**
|
###### 3. Codificación *Encoder*
|
Señal digital

>          \* Hasta página 18 materia pasada\*


S/N Num de veces de señal respecto al ruido

S/N mas grande, mejor partido

Si cada palabra tiene 10 bits
tamaño palabra = 3 bits
10\*3 = 30 bits

90 bits

> [!NOTE] Cuantización
> Cantidad de bits requeridos 
> Asignar palabra al punto muestreado de forma Arbitraria


*EJ)* Pasos Codificación de Pulso

Muestreo
1. Señal analógica
2. Señal PAM
3. Señal PAM Cuantificada

Cuantización
- Gráfica con valores arbitrarios en mapa cartesiano
+7 = 110
+5 = 111

Codificación
- Se traduce los valores redondeados de muestreo a cuantización
Palabra =111|110|110|110...

Error
> ¿Cómo se minimiza el error?

- Agregando más bits de paridad N
- Incremento R
A mayor N, el error (ruido) se hace mas pequeño respecto la señal
- Se escoje el inmediato superior para S/N


La tabla divina se obtiene a partir de:

#### Efectos del ruido

Fórmula Original:
($\frac{S}{N}$) = $3M^2  / 1+4(M^2-1)P_e$

Simplificado:
($\frac{S}{N}$) peaksalida = 3M^2

 ($\frac{S}{N}$) salida = M^2 

Por cada nuevo bit de cuantización que se use, la relación señal a ruido mejora en 6dB

- Para N se aproxima AL MAYOR siempre
- La señal debe abarcar todos los codigos del cuantizador
- Para señales más debiles, el valor de SNR dbe recalcularse
	
	Por culpa de un punto desperdicio tamaño de palabras
	EJ: 95% de palabras esta entre 1 y 5, 2% SOLO en -3

4 tipos distintos de ruido para Salida **Cuantizador**

1. Sobrecarga (saturación)
2. Aleatorio
Ruido blanco montado en la señal
3. Granular
Amplitud cercana al tamaño de un paso del cuantizador
4. Busqueda
Oscilacion cuando no hay entrada (no transmito nada) o cuando hay constante
	
	Se produce un tono de frecuencia $(1/2)f_s$


**EJERCICIO** PCM

máx 1% margen de error/distorsion
Frecuencia max 22kHz

a) Velocidad pcm tiene a la salida?

- 1% de error implica 1 nivel entre 100: 100 bits enviados 1 falló
La potencia de 2 más cercana es 128 (2^7) para alcanzar 100
N = 100
M = 128

- Se usa 128 niveles codificando en 7 bits

- La señal se muestra al *doble* de la frecuencia max
22kHz ($f_s$ muestreo) = 44KHz

- Por cada muestra hay 7 bits
44000 muestras/seg * 7bits/muestra 
= 308.000 bits/s

b) Relacion señal a ruido max y promedio?

- Ancho de banda mínimo para reconstruir un pulso cuadrado en banda base es:
2 veces la velocidad de bits:
616KHz (14B según tabla)

- Según la tabla/formula
Para 7 bits: 46.9dB y 42.1dB



Resumen:
Codificacion PCM resultado de 3 etapas
	Muestreo Cuantificacion Codificacion

Ruido de cuantizacion se suma al ruido en la señal original (antes de transmitir)
