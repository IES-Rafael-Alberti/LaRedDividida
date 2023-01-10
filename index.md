# La red dividida

![Untitled](La%20red%20dividida/Untitled.png)

# Configurar router CISCO

Lo primero que haremos será configurar las bocas del router CISCO.

Para ello deberemos de realizar los siguientes comandos

| Comando | Descripción |
| --- | --- |
| enable | Entra en “Modo avanzado” |
| Configure terminal | Entra en la configuración del terminal |
| Interface *Nombre de la interfaz* | Entra en el modo de configuración de la interfaz de red seleccionada |
| Ip address *IP* *Máscara* | Configuración de IP estática |
| No shutdown | Encendido de la interfaz |

Estos comandos sirven para configurar una de las bocas del router con IP estática para empezar a trabajar.

Una vez tenemos las IP levantadas del equipo, podemos configurar el DHCP en los rangos que hemos establecido.

# Configurar switch para VLAN

Para configurar las VLAN deberemos de acceder a los switches que hemos instalado en nuestra simulación y ejecutar los siguientes comandos:

Primero nombraremos todas las VLAN que utilizaremos. En este caso, configuramos 3 con los siguientes comandos desde el modo de configuración de terminal.

| Comando | Descripcion |
| --- | --- |
| Vlan *Numero* | Accede a la configuración de la VLAN |
| Name *Nombre* | Configura el nombre de la VLAN |

Una vez hemos establecido esto le daremos a la interfaz que usaremos para administrar la IP y puerta de enlace que tengamos configurada.

Accedemos a la interfaz de la VLAN que utilizaremos para gestión, es decir, la que se encuentra en el mismo rango que el router con la puerta de enlace y configuramos la IP en la configuración de la interfaz. La puerta de enlace se configurará a nivel de configuración global.

Configuramos las bocas del switch que vayamos a usar y asignamos la VLAN con la que estará trabajando, en este caso configuramos el switch que tiene conectados los servidores por lo que le daremos acceso a todos en la VLAN que vamos a utilizar.

Para finalizar la configuración del switch, configuraremos las conexiones de este con el router y el otro switch en ”Modo Trunk” para que la información de las VLAN pueda pasar por estas conexiones.

# Router on a-stick

Configuraremos el router on a-stick aplicándole interfaces virtuales las cuales tendrán la IP de la VLAN que queramos gestionar usando este router como el enlace troncal de las VLAN.

| Comando | Descripción |
| --- | --- |
| Description *Descripción* | Texto para identificar más fácilmente la conexión de la interfaz virtual |
| Encapsulation dot1Q *Número VLAN* | Comando que se utiliza para que esta interfaz virtual recoja la información proveniente de la VLAN designada |

Para configurar un “Port Security”, tendremos que realizar lo siguiente:

Nos dirigiremos a la configuración del switch y accederemos a su terminal.

Una vez dentro, escribiremos los siguientes comandos:

Hay 3 tipos de “Modo Violación de Seguridad”:

**Shutdown:** El puerto pasa inmediatamente al estado “Error-Disabled”, apaga el LED del puerto y envía un mensaje de “SYSLog”. Incrementa el contador de violaciones. Cuando un puerto seguro se encuentra en el estado “Error-Disabled”, un administrador debe volver a habilitarlo introduciendo los comandos:

- Shutdown
- No Shutdown

**Restrict:** El puerto deja caer paquetes con direcciones de origen desconocido hasta que se elimine un número suficiente de direcciones MAC seguras para que caigan por debajo del valor máximo o aumenten el valor máximo. Este modo hace que el contador de violaciones de seguridad incremente y genere un mensaje de “SYSLog”.

**Protect:** Este es el menos seguro de los modos de violación de seguridad. El puerto deja caer paquetes con direcciones de origen MAC desconocidas hasta que se elimine un número suficiente de direcciones MAC seguras para que caigan por debajo del valor máximo o aumenten el valor máximo. No se envía ningún mensaje de “SYSLog”.

Vamos a probar a configurar varias bocas a la vez, con los rangos de interfaces:

De esta manera seleccionamos las 24 bocas del switch y entramos en el modo config-if-range.

En nuestro switch 1 vamos a seleccionar un método de protección más agresivo (shutdown) ya que da acceso directo a los servidores. Mediante el siguiente comando:

Vemos que en todas las interfaces se ha aplicado la medida restrictiva.

Haremos lo mismo con el switch 5 que es el que sale del ordenador a la red, pero con el modo “Protect”.

Por último, configuraremos el “Port Security” en el switch. Para ello, tendremos que escribir los siguientes comandos:

- Enable
- Configure Terminal
- Interface FastEthernet(NºBocaFísica)
- Switchport Port-Security
- Switchport Port-Security Violation (Protect/Shutdown/Restrict)
- Switchport Port-Security MAC-Address Sticky: Sirve para que el activo se enganche a la primera MAC que encuentre.
- Switchport Port-Security Maximum (NºEquiposEnLaRed): Sirve para indicar cuántas MAC diferentes se pueden conectar a la boca física del dispositivo.
- Exit

# Segmentación final

Una vez hemos visto como se aplican todas las configuraciones pasaremos a la división de la red de forma que esta quede segmentada de manera correcta.

Se creara la VLAN 10  “Guerreros Z” en esta se incluirán los servidores, será la VLAN mas limitada debido a que en ella estableceremos los equipos que contienen los servicios que nuestra empresa necesita para funcionar.

Se creara la VLAN 20 “Namekianos” a la cual se le asignaran las bocas en las que se conectan los equipos de los usuarios de la empresa.

Se creara la VLAN 30 “Saiyans” a la cual se le asignaran las bocas en las que se conectan los equipos inalambricos de los usuarios de la empresa (esta no ha sido replicada en packet tracer debido a las limitaciones de los AP que traen).

Por ultimo tendremos la VLAN 100 “dragonball” la cual es la VLAN que utilizaran los administradores para poder realizar modificaciones en la configuración de red.