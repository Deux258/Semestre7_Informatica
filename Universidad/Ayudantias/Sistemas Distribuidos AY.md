
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


# Ayudantía
15/05/2025

