# Ayudantía 1

##### Largo de una antena
$$h = \frac{\lambda}{10}$$
##### Longitud de onda
$c = 3 * 10^8$
$$\lambda = \frac{c}{f}$$
##### Distancia que cubre una Antena
h = Altura de antena
$$d = \sqrt{2*r*h}$$
2 antenas a distancia d

##### Recepción de una señal

$$ Pr = \frac{Pt * Gt * Gr * \lambda^2}{(4\pi * d)^2} $$
Pt Potencia de transmision
Gt Ganancia de Transmision
Gr Ganancia del receptor
$\lambda^2$ Longitud de onda 
d Distancia que cubre la antena

##### Convertir dB a Watts

###### dB
$$ Pw = 10^{xdB/10} $$
###### dBm
$$ Pw = 10^{xdBm - 30/10} $$
###### dBi
$$ Pw = 10^{xdi/10} $$

#### Ejercicios

1. Convertir :

a. 12dB = $10^{12/10}$ = 15,85 veces 
b. 56dB = $10^{56/10}$ = 398,1 veces
c. 15dBm = $10^{15-30/10}$ = 0,0316 W
d. 20dBm = $10^{20-30/10}$ = 0,1 W
e. 3dBi = $10^{3/10}$ = 1,99 veces
f. 12dBi = $10^{12/10}$ = 15,85 veces

2. Ejercicio de desarrollo

 _a. Determine:_ Máxima distancia a la que pueden comunicarse entre ellos (antenas)

Sensibilidad del sist. de comunicación S = -95dBm 
=> Pr = -95 dBm
Potencia de transmisión Pt = 40dBm
Ganancia de antenas transmisoras y receptoras, es decir:
Gt = 12dBi
Gr = 12dBi
Frecuencia de transmisión f = 700MHz

> Aplicar fórmula de Recepción de una señal

_Distancia D_ = $\sqrt{Pt * Gt * Gr * \lambda^2 / Pr}/4\pi$
Nota: $4\pi$ fuera de la raíz

Tal que:

*fotito desarrollo*

*Calcular variables de distancia para luego calcular D*

_b._ La misma lesera pero con otros valores c:

$f$ = 1200MHz
Gt = 12dBi
Gr = 12dBi

_Fotito (mismo desarrollo, cambiar los datos)_


# Ayudantía 3
08/04/25

## Conceptos básicos

Fórmula para corregir los bits al momento de transmitir una palabra errónea
$$ C = R xo E $$
Al momento de mult matrices 
Matriz G: I | P
Matriz H: P | I

Ejercicio

Longitud de onda disminuye cuando la frecuencia aumenta

La propagacion terrestre



