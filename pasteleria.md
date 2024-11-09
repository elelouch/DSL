# DSL Pasteleria

## Motivacion
Se distinguio el siguiente proceso durante un pedido.

- El *cliente* se contacta para definir un *pedido*. 
- Se requieren encontrar las *recetas* para el producto.
- Las recetas tienen una lista definida de *ingredientes* ademas de un *procedimiento*.
- En caso de que falten ingredientes, se "iran a comprar" segun la receta.
- Si se usan varias recetas, hay que ir anotando que comprar de cada una.
- Al obtener todo se comienza la preparacion de las recetas basandose en la *narrativa*
del *producto* descrito por cliente.

## Propuesta
Se define un DSL utilizado en el ambito de la pasteleria que facilite el acceso
a una lista de clientes, recetas, ingredientes.
Podra realizar operaciones CRUD (Create, Read, Update, Delete)

Este DSL deberia dar una lista de ingredientes dado un grupo de recetas.

Cada entidad estaria representada por una tabla, y cada tabla sera un archivo. 
Las entidades estan limitadas para hacer mas sencillo el lenguaje.

## Sintaxis y ejemplos

<COMANDO> <TABLA> COLUMNA CONDICION [WITH] [UPDATE]

GET CLIENT WHERE CAKE IS DONE
UPDATE CLIENT WHERE NAME CONTAINS "" WITH
DELETE INGREDIENTE WHERE NAME IS "huevo"
