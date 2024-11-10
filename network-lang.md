# Network Descriptive Language
DSL para describir y programar redes de computadoras.

## Motivacion
Existe software de redes, como packet tracer, donde se utilizan escenarios para describir una red.
Se pueden realizar copias de estos para ser distribuidos. Las configuraciones (serie de comandos) de cada elemento de red debe ser extraida
y se deben "volver a insertar" al recrear el escenario.
A su vez, las configuraciones pueden ser un poco oscuras al tener mucha variedad de comandos.

Este lenguaje pretende hacer scripts reutilizables para armar archivos de configuracion de redes en distintos 
programas, realizando una abstraccion de como se configura un elemento en la red.
Obteniendo asi una manera mas "interactiva" y menos oscura a la hora de armar estos archivos, sin 
perder la rigurosidad que el output tal vez necesite.
El output de este script sera, en primera instancia, un archivo en formato JSON. 
Idealmente se podria extender a los formatos que requieran otros softwares.

## Descripcion 
El lenguaje poseera funciones primitivas para crear, y modificar *elementos de red*.

Algunas primitivas posibles:

	- create_switch
	- create_network
	- create_computer
	- create_autsys
	- connect

La gracia de estas primitivas sera poder asumir configuracion a la hora de armar
un elemento de la red. Esta idea de asumir configuracion se intentara llevar en todo el
lenguaje.

Debera poseer estructuras modificables y accesibles para:

    1. Describir configuraciones relativamente grandes.
    2. Evitar el pasaje de configuraciones grandes parametro por parametro.
    3. Editar configuraciones grandes.
    4. Utilizar propiedades particulares de una estructura.

Poseera funciones como para permitir crear sistemas relativamente complejos, las veces necesarias.
Debera poder guardar elementos de red para ser reutilizados y sus propiedades deberan ser accesibles.
Hilar tan fino en un resultado como se pueda llegar a tener en cuenta.
Poseera estructuras de control para evitar repetir codigo y 
poder plantear patrones particulares a la hora de describir una red.

Todos los elementos de red van a estar a la misma altura en la salida.
Los niveles de abstraccion generalmente son dados por el administrador de la red.

### Ejemplo
ausys_config = {
	egp = bgp,
	igp = ospf
}

autsys1 = create_autsys(ausys_config);

switch1_config = {
	enable_vlan = true,
};

switch1 = create_switch(switch1_config);
switch1_config.enable_vlan = false;
switch2 = create_switch(switch1_config);

network_config = {
	ip = "192.168.0.128/25",
};

net1 = create_lan(network_config);

network_config = 192.168.0.0/25;

net2 = create_lan(network_config);
amount_to_create = 4; 

while(i = 0; i < amount_to_create; i++) { 
	pc = create_computer();
	if(i % 2 == 0) { 
		connect(pc, switch1) 
	} else { 
		connect(pc, switch2) 
	}
};

connect(net1, net2)

#### Salida del programa
{
	"igp" : "ospf",
	"egp" : "bgp",
	"ip" : "192.168.0.0"
	"mask": "255.255.255.0"
	"router1" : {
		"eth0": {
			"ip" : "192.168.0.1"
			"mask": "255.255.255.128"
			"mac": "0x123456789ABC"
		 },
		"eth1": {
			"ip" : "192.168.0.129"
			"mask": "255.255.255.128"
			"mac": "0x123056789ABE"
		}
	},
	"switch1" : {
		"stp_enabled": false,
		"port1" : {
			"vlan_id":1,
		},
		"port2" : {
			"vlan_id":2,
		}
	},
	"switch2" : {
		"port1" : {
			"vlan_id":0
		},
		"port2" : {
			"vlan_id":0
		}
	},
	"computer0" : {
		"eth0" : {
		}
	},
	"computer1" : {
		"eth0" : {
			mac: ...
			ip: ...
			default_gateway: ...
			dhcp_server: ...
		}
	},
	"computer2": {
		"eth0" : {
			...
		}
	},
	"computer3": {
		"eth0" : {
		}
	}	
}
