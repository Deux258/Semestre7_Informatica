
No necesariamente disminuyen los tiempos de respuesta, dado que se pueden crear puntos calientes en la distribución y no variar tanto el tiempo de respuesta, incluso usando Hash.
R: correcta

2. Correcto
3. Tiene la funcion de distribuir uniformemente y aleatoria, no aproximadamente uniformes. El hecho de que un peer sea vecino, no significa que tenga valores similares.
No necesariamente tiene valores semanticos similares. Su cercania depende del hash generado

Ventaja de escalar Cliente/servidor
Simplemente agrego "esclavos" y puedo escalar horizontalmente sin problema
Desventaja
Si cae el servidor/maestro, se caen todos dado que no pueden comunicar con el ente central

Ocuparia message queue para hacer una cola de acceso a la pagina La7am y agregando un temporizador del tiempo que tiene el usuario que ingreso para que luego de terminar su turno, ingrese el siguiente y consuma el "evento".


# Ayudantía: Preguntas Solemne
04/06/2025

1. Los peers estructurados se distribuyen correctamente al ser particionado con tablas de hash.
R: Correcto, hace justo lo que dice el enunciado.
Se distribuyen uniformemente sobre el espacio del overlay ==rompiento la semántica==.
Cambia el hash completamente, mas no su espacio.

2. "Cuando mapeamos pares y recursos en un modelo basado en DHT estos se ubican de manera aproximadamente uniforme en el espacio numérico de mapeo, este hecho permite que recursos con valores similares se puedan encontrar fácilmente en pares vecinos. Un ejemplo de esto es lo que ocurre en Chord y sus sucesores."
R: Incorrecto, valores similares se distribuyen lejanos en el overlay.
DHT rompen la semantica en la red, aunque sea muy similar no va a tener el mismo hash.

3. "El mayor desafío de los sistemas P2P no estructurados es lograr localizar recursos a un bajo costo en el número de mensajes diseminados en el overlay. Para ello estos sistemas utilizan técnicas de flooding controlado. Estas técnicas requieren de una parametrización previa para ajustarse al escenario y maximizar la probabilidad de éxito en la búsqueda de la aplicación."
R: Incorrecto, la parte de parametrización es fake

4. "Las arquitecturas de borde o edge architectures se basan en la idea de alojar servicios cercanos a los usuarios donde los datos se generan. Un ejemplo de este tipo de arquitecturas es Akamai. Akamai es un distribuidor de contenido o CDN que explota esta arquitectura para la entrega de contenidos. Este tipo de sistemas aplican técnicas de monitoreo y análisis de calidad de servicio (por medio de una métrica) para la elección del servidor de borde que brindará el servicio requerido."
R: Correcto

5. "NTP (Network Time Protocol) asegura la sincronización precisa de relojes entre sistemas distribuidos, incluso en redes de alta latencia o con variabilidad, gracias a su estructura jerárquica de servidores organizados en niveles (stratum), donde el Stratum 0 actúa como referencia más precisa. Utiliza algoritmos avanzados para calcular y ajustar desfases, corrigiendo latencias de red mediante estimaciones de ida y vuelta, y logra una precisión de milisegundos en redes públicas y de microsegundos
R: Correcto
Depende del entorno, se evita usar en servidores pero es god

6. "Los algoritmos de exclusión mutua distribuidos deben cumplir con dos premisas base, la seguridad y la viveza. El primer concepto apunta a dar garantía que todo proceso pueda ingresar a la región crítica y el segundo, a la disponibilidad de los procesos para su ingreso. Una premisa adicional es el orden, que apunta a respetar el orden en que los procesos manifiestan su interés por acceder a la región crítica."
R: Incorrecto. Viveza es fake: no son 2 premisas base, sino 3

7. "Si consideramos un modelo de elección basado en el algoritmo el bully con n procesos que participan en la elección y donde los procesos están identificados con los ids del [O...n- 1]. El proceso de elección se inicia, si solo si, el actual proceso electo falla y dicha falla es detectada por otro proceso participante. Bajo este modelo podrían todos los procesos identificar la caída al mismo tiempo, iniciando todos de manera concurrente la elección, esto último incrementa la comunicación pero no afecta en los tiempos de convergencia"
R: Correcto

8. "Todo algoritmo de elección comienza con un conjunto de procesos candidatos para ser electos y finaliza con un único proceso electo. Un algoritmo clásico de estos es el conocido Bully. Este algoritmo distribuido converge eligiendo a aquel proceso que posee el mayor pid. Una de las propiedades de este algoritmo radica en su capacidad de lograr la elección de un único proceso incluso ante la ocurrencia de particiones de red."
R: Cada partición tiene su propio coordinador, eventualmente tendria 2 procesos lideres sin tener en consideración la otra particion

9. ![[Pasted image 20250604193305.png]]

a. PID 40 le comunica a los que tienen mayor PID que el, luego, los que son mayores le responden al antiguo coordinador para determinar el nuevo coordinador, mantienen el envio al 70 para ver si responde en caso de que vuelva. Responden que estan disponibles 50, 60 a 40. PID queda fuera del proceso dado que hay PIDs mayores que el. 50 queda fuera, si 60 no obtiene respuesta de 70, se autoproclama como coordinador y envia un wsp a todos los PIDs dando aviso. Si vuelve 70 ya no es mas el coordinador

![[Pasted image 20250604194245.png]]

a. 
b. 
c. Confirmación

10. Considere que tenemos un servidor electo o leader y 4 servidores Zoo-Kee-Puh adicionales. Los servidores tienen un pid secuencial en el rango 1 al 5, donde el servidor leader es el 4. Muestre como se lleva a cabo la secuencia de interacciones siguiendo el modelo de resolución de escrituras presentado anteriormente. Considere que existe un evento inicial de escritura recibido en el proceso 2.(5 puntos).
R: el proceso 2 le envia el mensaje de escritura al lider (4) y el lider lo esparce a todos los demás (incluyendo a 2).





