# Ayudantía 3
09/04/25

Repaso, Proyecto y UML

## UML Y Modelamiento

Diagramas que sirven para representar/diseñar sistemas.

- Comunicar ideas
- Documentar el sistema
### Diagrama de Casos de uso

**Propósito**
Describe la funcionalidad desde la vista del usuario.

Admin --  Acciones -- Usuario
Acciones son requisitos de mi app (ej: gestionar, actualizar, eliminar, etc..)

Si yo quiero agregar un producto al carrito --> Incluye iniciar sesión

> Dibujo cualquiera
### Diagrama de Clases
Lit base de datos
Sirve para mostrar cómo se relacionan los datos
Fundamental en diseño orientado a objetos 

### Diagrama de Secuencia

Como van a interactuar al final dependiendo del orden

Usuario          Objeto:Clase
	--->	|
		|
		|  -->    |
|            			     |	
|			           <--|

| Mensaje     | Flecha       | Nota             |
| ----------- | ------------ | ---------------- |
| Sincrónico  | ------->     | Espero como cola |
| Asincrónico | -------->>   | Independiente    |
| Respuesta   | - - - - - -> |                  |

Reconocer mi actror principal y cuales van a ser mis objetos

### Diagrama de Actividad

Secuencia de distintas actividades (mapa de nodos)

o ---> |=| ---> X ---> |=| ---> o
             |
			|=| ---> o

|=| Se divide en dos: si / no


### Diagrama de Estados

Modela el comportamiento de un objeto a lo largo de su ciclo
Para objetos de comportamiento complejo. Los cuadros son Estados

> Dibujo ovarios


## Repaso

### Roadmap

No tengo que especificar, tiene que ser algo general
No tengo fechas especificas, si no periodos (meses)
Grandes cosas que tengo que lograr a lo largo del tiempo

#### Versiones

Puntos en el tiempo donde se entrega una version "inicial" a los usuarios/entorno.
- Hitos de entrega de valor
- Cada versión tiene que tener sentido

### User Storty Map

Descripciones cortas de una funcionalidad específica
- Contexto
- Como
- Quiero
- Para
Tiene que haber criterios para decir que funciona

Epicas  -> Historias de usuario -> Subtareas

Debe ser:
- Verificable
- Supuestos
- Testeable

### Descomponer en SubTask

Dividir una historia de usuario en tareas más pequeñas
Hasta 3 subtareas

De mayor a menor priorización

#### Dependencias y Rutas críticas

Tengo que hacer las cosas críticas primero. Prioridad para poder seguir con lo demás.

