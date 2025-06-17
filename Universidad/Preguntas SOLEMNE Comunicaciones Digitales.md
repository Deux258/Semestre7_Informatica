PD: Al profe no le gusta Ninguna de las respuestas
Una buena parte de las respuestas se pueden descartar con lógica

1. La relacuion señal-ruido en una modulacion afecta a
	R: tasa de error de bit

2. Un repetidor regenerativo se coloca en el camino de la señal para 
	R: reconstruir los pulsos
	Se aprovecha de reconstruir para retransmitir la señal completamente reconstruida
	*NOTA:* El num de repetidores tiene que ser IMPAR (cuando es par con bit errado, en el 2do repetidor habrá la misma probabilidad)

3. El uso del rolloff se debe a que:
	R: Los pulsos sinc(x) no son realizables físicamente
factor de rolloff es usado en el filtro de coseno realzado, disminuye la interferencia intersimbolo

4. Una modulación 64QAM
	R: Agrupa 6 bits por símbolo
	\** Dividir 64 por 4 cuadrantes ($2^6$) 

5. A medida que se reduce el $E_b / N_0$, el BER
	R: Aumenta
	(Gráfico de distintas modulaciones, mayor mientras más a la derecha se encuentre: Mayor energia con respecto al ruido)
	Si disminuyo en el eje X, mayor es el BER 

6. La codificación Manchester permite:
	R: Elimina el valor de continua (frecuencia cero) de la señal
	Se le conoce también como fase dividida
	Manchester asegura que el área bajo la curva no sea 0 empleando valores positivos y negativos para representar los niveles de la señal (evita corriente continua)
	(frecuencia cero) voltaje discontinuo

7. Una modulación multinivel permite
	R: Reducir el ancho de banda ocupado
	Cada nivel representa una pareja de bits (2 bits, 4 palabras), mayor cantidad de palabras transmitidas ($l$ bits )

8. El umbral optimo para diferenciar entre dos niveles de una modulación binaria es
	R: La mitad de la distancia entre ambos niveles
	 Cuando yo cuantifico siempre hay un margen de error (NUNCA produce interferencia intersimbólico)
	 (por ej) nivel 1v, 2v si tengo 1.7v aproximo a 2v

9. La interferenicia intersímbolo es consecuencia de
	R: El efecto del *canal* sobre la señal transmitida
	EJ: Aire entre canales
	(Error de cuantización NUNCA produce interferencia intersimbólico)

10. Para la modulación PSK, al aumentar la cantidad de símbolos
	R: Se requiere mayor Señal a Ruido para lograr el mismo BER
	Análisis de cómo funcionan las modulaciones (gráfico) ** 
	EJ) BPSK: Modulación Binaria 2^1
	Los puntos donde se dibujen, lo que hacen es acercarse entre ellos cada vez que se aumenta la cantidad de bits
	--> Limita la capacidad de margen de error

11. La cantidad de elementos de una constelación depende de
    R: La cantidad de símbolos que se utilizan
	La constelación son los puntos dibujados en el plano cartesiano
	
12. AL aumentar la cantidad de bits por muestra, en PCM
    R: Aumenta el bit rate
	EL ancho de banda lo coloca el fabricante

13. ¿Cómo se define el ruido generado durante el proceso de cuantización?
	R: Diferencia entre la señal original y su versión cuantizada
	(Busca la palabra más cercana, EJ: si tengo 1.2v redondeo a 1v)

14. En las siguientes proposiciones, cual es Verdadera?
a. Un circuito con codificación NRZ necesita una fuente de voltaje que pueda variar
	unipolar [0,A] NO requiere de multiples voltajes
	polar [-A, A] ESTA si
	Con una fuente de 5v es más que suficiente (constante) para unipolar 
	el 1 digital esta a Av
	el 0 digital esta a -Av
	Plt: FAKE
*b.* El código RZ operea de forma más eficiente cuando hay una proporción equilibrada entre unos y ceros
	Return to Zero: si transmito 10 bits donde la mitad es 1 y la otra es 0, esta opera más eficiente por la sumatoria de las áreas bajo las curvas (no sé si esta correcta esta desc.)
	Plt: VERDADERO


**PROBLEMA 15**

El ancho de banda disponible en un canal es de 100KHz
Señal analogica de entrada tiene frecuencia máx de 15kHz
se usan 16 bits x muestra
Calcule el Bit rate ....

- fs tiene que ser el doble de la frecuencia máxima
$fs = 2f$
- *n* Num de bits por muestra = 16 (16 bits por palabra)
- Recordar R = $n / T$ con T como periodo (T = 1/fs)

- 16 / (1 / 30.000)

Elija la modulación más eficiente que pueda aplicarse del gráfico BER para 17db

- para 17db automáticamente descarto TODAS las modulaciones menores a 17 (gráfico Eb/N0)
- el más eficiente es el que está más abajo (16-PSK) $10^{-4.7}$ aprox

(se adjunta el gráfico en la solemne, tranka palanca)

**PROBLEMA 16**

un sistema de espectro expandido usa 16 chips por bit. Calcule la ganancia de procesamiento y el ancho de banda ocupado si la señal de entrada tiene R = 1000 bits/s

*n* = 16
tasa de bits *Rb* = 1000 bits/s
la tasa de chips *Rc* será 16 * 1000 = 16 mil chips/s

gnancia de procesamiento PG
PG (db) = $10 log _ {10} (16000 / 1000)$

10 log_{10} (Rc / Rb)

PG = 10 * 1.2041 = 12.041 dB

Ancho de Banda Ocupado
2 * Rc = 2* 16.000Hz = 32KHz