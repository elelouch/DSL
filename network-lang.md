# Networks Descriptive Language
Lenguaje utilizado para la programacion de redes de computadoras.

## Motivacion
Existe software de redes, como packet tracer, donde se utilizan escenarios para describir una red.
Se pueden realizar copias de estos para ser distribuidos. La configuracion (serie de comandos) de cada elemento de red debe ser extraida
se debe "volver a insertar" al recrear el escenario.
A su vez, la configuracion puede ser un poco oscura al tener mucha variedad de comandos.

Este lenguaje pretende hacer scripts reutilizables para armar archivos de configuracion de redes en distintos 
programas, realizando una abstraccion muy grande de como se configura un elemento en la red.
Obteniendo asi una manera mas "interactiva" y menos oscura a la hora de armar estos archivos, sin 
perder la rigurosidad que el output tal vez necesite.
El output de este script sera, en primera instancia, un archivo en formato JSON. 
Idealmente se podria extender a los formatos que requieran otros softwares.

## Descripcion (ideal)
El lenguaje poseera primitivas para crear, y modificar elementos en la red.
Debera poseer estructuras modificables para:
    1. Describir configuraciones relativamente grandes.
    2. Evitar el pasaje de configuraciones grandes parametro por parametro.
    3. Editar configuraciones grandes.

### Ejemplos
network = create_network()
