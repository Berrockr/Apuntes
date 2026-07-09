# Infraestructura de Seguridad de Redes de Datos

## Topologías de Red

### Representaciones de Red

Los diagramas de red o diagramas topológicos utilizan símbolos para representar los dispositivos dentro de la red. Contamos con la topología física (ilustran la ubicación física de los dispositivos intermedios y la instalación de cables) y lógica (ilustran dispositivos, puertos y el esquema de direccionamiento de la red).

Una LAN es una infraestructura de red que abarca un área geográfica pequeña, administrada por una organización o individuo y proporciona ancho de banda de alta velocidad a los dispositivos finales e intermedios.

Una WAN interconecta LAN en áreas geográficas amplias. Por lo general su administración está a cargo de varios ISP y proporcionan enlaces de menor velocidad entre las LAN.

El diseño LAN jerárquico separa la topología de red en grupos modulares o capas:

- **Acceso.** Ofrece a los terminales y usuarios acceso directo a la red (switches).
- **Distribución.** Agrega capas de acceso y brinda conectividad a los servicios.
- **Núcleo.** Provee conectividad entre las capas de distribución para grandes entornos LAN.

En algunas redes empresariales, las capas de núcleo y distribución se combinan en una, reduciendo costes y complejidad.

### Arquitecturas de Seguridad Comunes

El diseño de firewall tiene por objetivo permitir o denegar el tráfico según el origen, destino y tipo de tráfico.

- **Público y privado.** Diseño de firewall que confía en la red privada (interna) pero no en la pública (externa).
- **Zona Desmilitarizada (DMZ).** Diseño de firewall donde hay una interfaz conectada a la red privada, una a la pública y una a la DMZ, que permite selectivamente el tráfico.
- **Firewall de Políticas Basado en Zonas (ZPF).** Emplean el concepto de zonas (grupos de al menos una interfaz con funciones o características similares) para brindar mayor flexibilidad.

## Dispositivos de Seguridad

### Firewalls

Sistema o grupo de sistemas que impone una política de control de acceso entre redes. Originalmente trabaja a nivel de capa 4. Resiste a ataques de red y es el único punto de tránsito entre redes internas y externas.

#### Ventajas del firewall

- Previenen la exposición de hosts, recursos y aplicaciones confidenciales.
- Sanean el flujo de protocolos, previniendo así el aprovechamiento de las fallas de protocolos.
- Bloquean los datos maliciosos de servidores y clientes.
- Reducen la complejidad de la administración.

#### Limitaciones del firewall

- Si está mal configurado, podría volverse en punto único de falla.
- Los datos de muchas aplicaciones no se pueden transmitir con seguridad mediante firewalls.
- Puede reducirse la velocidad de la red.
- Los usuarios pueden buscar formas de eludirlo, usando incluso tunelizado de tráfico.

#### Firewall para filtrado de paquetes (sin estado)

Son parte de un firewall de router, que autoriza o rechaza el tráfico a partir de la información de las capas 3 y 4. Sin estado quiere decir que cada solicitud la trata de forma independiente sin recordar interacciones previas. Utilizan una búsqueda en la tabla de políticas que filtran el tráfico según criterios específicos.

#### Firewall con estado

Proporcionan un filtrado de paquetes con estado, utilizando información de conexión que se mantiene en una tabla de estados. Actúa a nivel de capa 3, 4 y 5.

#### Firewall de gateway de aplicación (firewall de proxy)

Filtra la información de las capas 3, 4, 5 y 7. La mayor parte del control y filtrado se realiza en el software. También filtra y restringe aplicaciones.

#### Firewall de siguiente generación (NGFW)

Van más allá, integrando IPS, control y reconocimiento de aplicaciones para ver y bloquear aplicaciones riesgosas, rutas de actualización y técnicas para afrontar amenazas en constante evolución.

### Dispositivo de Detección y Prevención de Intrusiones (IDS, IPS)

IDS detecta una intrusión, IPS los previene. La diferencia está en que el IPS lo envía a cuarentena y aprende del ataque. La arquitectura de red integra estas soluciones en los puntos de entrada y salida.

El IDS no genera impacto en la red, pero sus acciones de respuesta no pueden detener paquetes activadores, son vulnerables a las técnicas de evasión y requieren la afinación correcta para las acciones de respuesta. Un IPS, en cambio, detiene paquetes activadores y puede usar técnicas de normalización de transmisiones, pero podría generar latencia.

Tenemos los IPS basados en host (HIPS) y en red. HIPS es un software instalado en el host para controlar y analizar actividades sospechosas. Los basados en red usan dispositivos especializados.

## Servicios de Seguridad

### Control de Tráfico con ACL

Una lista de control de acceso (ACL, Access Control List) es una serie de comandos que controla si un dispositivo reenvía o descarta paquetes según la información que se encuentra en el encabezado de este.

- Limitan el tráfico de red para aumentar su rendimiento.
- Proporcionan control del flujo de tráfico.
- Proporcionan un nivel básico de seguridad para el acceso a la red.
- Filtran el tráfico según su tipo.
- Filtran a los hosts para permitir o denegar acceso a los servicios de red.

#### ACL estándar

Permiten o deniegan el tráfico únicamente con las direcciones IPv4 de origen.

#### ACL extendida

Filtran los paquetes IPv4 basados en atributos que incluyen el tipo de protocolo, direcciones IPv4 y puertos de origen y destino e información optativo de tipo de protocolo.

Ambas se pueden crear con un número para identificarlas. Un ejemplo para permitir una sola IP con una ACL estándar:

```html
Router(config)#access-list 1 permit host 192.168.10.1
Router(config)#access-list 1 deny any
Router(config)#interface fastethernet 0/0
Router(config-if)#ip access-group 1 in
```

### SNMP

Protocolo simple de administración de red (Simple Network Management Protocol) es un protocolo de la capa de aplicación que proporciona un formato de mensaje para la comunicación entre administradores y agentes. Permite administrar dispositivos, rendimiento de red, encontrar y resolver problemas y planificar el crecimiento de la red.

El sistema SNMP consiste en dos elementos, el administrador (ejecuta software de administración SNMP) y el agente (nodos monitoreados y administrados).

### NetFlow

Tecnología de Cisco IOS que proporciona estadísticas sobre los paquetes que atraviesan un router o switch multicapa de Cisco.

Proporciona datos para habilitar la seguridad y monitoreo de red, planificación de red, análisis de tráfico, contabilidad IP, además de que puede monitorear la conexión de aplicaciones mediante el seguimiento de conteo de bytes y paquetes de flujo.

Se habilita NetFlow en el router y un colector NetFlow analiza este tráfico.

### Replicación de Puertos

Característica que permite al switch hacer copias del tráfico que pasa y luego enviarlo a un puerto donde se encuentra un supervisor de redes conectado.

### Servidores Syslog

El protocolo Syslog permite que los dispositivos envíen sus mensajes de sistema a servidores Syslog mediante la red. Provee la capacidad de recopilar información de registro, escoger el tipo de información de registro que se captura y de especificar el destino de los mensajes Syslog capturados.

### NTP

Protocolo de tiempo de red, las redes utilizan un sistema jerárquico de fuentes de tiempo, donde cada nivel se denomina estrato.

- **Estrato 0.** Una red NTP obtiene la hora de fuentes horarias autorizadas.
- **Estrato 1.** Dispositivos directamente conectados a las fuentes de tiempo autorizadas.
- **Estrato 2 y más bajos.** Sincronizan su horario con paquetes NTP desde servidores del estrato superior inmediato.

### Servidores AAA

- **Autenticación.** Los usuarios y administradores prueban que son quienes dicen ser. La autenticación AAA es una forma centralizada de controlar el acceso a la red.
- **Autorización.** Una vez autenticado, se determinan sus privilegios.
- **Contabilidad.** Registra lo que hace el usuario, incluidos los elementos a los que accede, la cantidad de tiempo que accede y los cambios.

### VPN

Red privada que se crea un una red pública. Utiliza conexiones virtuales, cuyo enrutamiento se realiza desde la organización hacia el sitio remoto. Es un entorno de comunicaciones en el que el acceso se controla de forma estricta y se encripta el tráfico.

# Los Atacantes y sus Herramientas

## ¿Quién Está Atacando Nuestra Red?

### Amenaza, Vulnerabilidad y Riesgo

- Amenaza es un peligro potencial de un activo.
- Vulnerabilidad es cualquier cosa que puede ser aprovechada por una amenaza.
- Superficie de ataque es la suma total de vulnerabilidades presentes en un sistema las cuales son accesibles para un atacante.
- Ataque (exploit) es un mecanismo utilizado para aprovechar una vulnerabilidad y poner en riesgo un activo.
- Riesgo es la probabilidad de que una amenaza específica aproveche una vulnerabilidad.

Gestión de riesgos es el proceso de equilibrar los costos operacionales de proporcionar medidas de protección con los beneficios que brinda la protección de los activos.

- **Aceptación de riesgos.** No se toman acciones respecto a un riesgo cuando el costo de las opciones de gestión sobrepasa el costo del riesgo mismo.
- **Evasión de riesgos.** Evitar toda exposición al riesgo eliminando la actividad, resultando así en la pérdida de beneficios de la actividad.
- **Reducción de riesgos.** Reduce la exposición al riesgo mediante estrategias de mitigación.
- **Transferencia de riesgos.** Se transfieren a un tercero que esté dispuesto a responder por ellos.

Una contramedida es una acción tomada para proteger los activos mediante la mitigación de una amenaza o la reducción del riesgo.

Impacto es el daño potencial causado por la amenaza que sufre la organización.

### Hacker vs Atacante

Un hacker es un programador capaz de desarrollar nuevos programas y cambios de código en los programas para hacerlos más eficientes. También puede ser un profesional en redes que utiliza habilidades de programación sofisticadas para asegurar que las redes no sean vulnerables a los ataques. Por último, puede ser una persona que utiliza programas para entorpecer o dañar datos en servidores.

#### Hackers de sombrero blanco

Son hackers éticos que utilizan sus habilidades de programación con fines buenos, éticos y legales.

#### Hackers de sombrero gris

Personas que comenten delitos y realizan acciones probablemente inmorales, pero no para beneficio personal ni para causar daños.

#### Hackers de sombrero negro

Delincuentes que violan la seguridad de una computadora y una red para beneficio personal.

### Ciberdelincuentes

Son atacantes cuya motivación es hacer dinero. Operan bajo “economía subterránea”, comprando y vendiendo información personal y propiedad intelectual.

### Tareas de Ciberseguridad

La ciberseguridad es una responsabilidad compartida que todos los usuarios deben practicar para que internet y las redes sean más seguras.

## Categorías de Ataques

#### Ataques de intercepción pasiva (eavesdropping)

Ocurre cuando un atacante captura y escucha el tráfico de red. También se le conoce como sniffing o snooping.

#### Ataques de modificación de datos

Se producen cuando un atacante ha capturado el tráfico de la empresa y ha modificado los datos sin el consentimiento ni conocimiento del emisor o receptor.

#### Ataque de suplantación de dirección IP

Ocurre cuando un atacante crea un paquete IP que parece provenir de una dirección válida dentro de la intranet corporativa.

#### Ataques basados en contraseñas

Se producen cuando un atacante obtiene las credenciales de una cuenta de usuario válido.

#### Ataques de denegación de servicios (DoS)

Puede bloquear el tráfico, lo que resulta en una pérdida de acceso a recursos de red. Impide el uso normal de una computadora o red.

#### Ataque Man-in-the-middle (MiTM)

Este tipo de ataques se produce cuando los atacantes se posicionan entre la fuente de origen y destino.

#### Ataque de contraseña comprometida

Ocurre cuando un atacante obtiene una contraseña secreta.

#### Ataque de sniffer

Un sniffer es una aplicación o dispositivo que puede leer, monitorear y capturar intercambios de datos en la red y leer paquetes de red. Si los paquetes no están encriptados, un analizador de protocolos permite ver por completo los datos que lo componen.

# Amenazas y Ataques Comunes

## Malware

Tipo de código o software diseñado para dañar, interrumpir, robar o efectuar otras acciones malintencionadas o ilegítimas en datos, usuarios o redes. Es un medio para colocar una payload (carga dañina).

### Virus

Tipo de malware que se propaga al introducir una copia de sí mismo dentro de otro programa. Al ser ejecutado, se propaga de una computadora a otra, infectando toda la red.

### Troyanos

Un virus troyano es un software que parece legítimo, pero contiene código malicioso que ataca los privilegios del usuario que lo ejecuta.

### Gusanos

Similares a los virus pues pueden replicarse, aunque pueden ejecutarse sin la necesidad de un programa que los aloje. Pueden ralentizar las redes mientras se propagan.

### Ransomware

Malware que deniega el acceso al sistema informático o sus datos, usualmente encriptándolos.

## Ataques de Red Comunes: Reconocimiento, Acceso e Ingeniería Social

### Ataques de Reconocimiento

Es la recopilación de información. Los atacantes la usan para realizar la detección no autorizada y el mapeo de sistemas, servicios y vulnerabilidades. Preceden a los ataques de acceso o los de DoS.

### Ataques de Acceso

Explotan vulnerabilidades conocidas de los servicios de autenticación, FTP y servicios web para obtener acceso a las cuentas web, bases de datos e información confidencial.

#### Ataques de contraseña

El atacante intenta descubrir contraseñas críticas del sistema utilizando métodos y herramientas de descifrado de contraseñas.

#### Ataques de suplantación de identidad

El dispositivo del atacante intenta hacerse pasar por otro dispositivo mediante la falsificación de datos. Puede ser suplantación de IP, MAC y DHCP.

#### Ingeniería social

Es un tipo de ataque de acceso que intenta manipular a las personas para que realicen acciones o divulguen información confidencial.

## Ataques de Red: Denegación de Servicios, Desbordamientos del Búfer y Evasión

### Ataques DoS y DDoS

Crea un tipo de interrupción en los servicios de red para usuarios, dispositivos o aplicaciones. Pueden darse por una cantidad inmensa de tráfico o por paquetes formateados maliciosamente.

### Ataques de Desbordamiento de Búfer

Se hace un ataque DoS mediante desbordamiento de búfer con el fin de encontrar una falla relacionada con la memoria y aprovecharla.

### Métodos de Evasión

- **Encriptación y tunelizado.** Utilizan túneles para ocultar, encriptar o codificar archivos de malware, impidiendo que muchas técnicas de detección de seguridad detecten e identifiquen el malware. Tunelización también puede significar ocultar datos robados dentro de túneles.
- **Agotamiento de recursos.** Mantiene al usuario demasiado ocupado como para responder adecuadamente con las técnicas de detección de seguridad.
- **Fragmentación de tráfico.** Divide una payload maliciosa en paquetes más pequeños para eludir la detección de seguridad de red.
- **Interpretación errónea a nivel de protocolos.** Se produce cuando las defensas de una red no manejan adecuadamente las características de una PDU. Esto puede hacer que el firewall ignore los paquetes que debería comprobar.
- **Sustitución de tráfico.** El atacante intenta engañar al IPS enmascarando los datos en el payload mediante una codificación en formato diferente.
- **Inserción de tráfico.** Similar a la sustitución de tráfico, pero el atacante inserta bytes extra en el payload, haciendo que el IPS no lo detecte.
- **Pivoting.** Esta técnica supone que el atacante ya tiene acceso y busca ampliar todavía más su acceso a la red atacada.
- **Rootkits.** Herramienta de ataque que se integra a bajo nivel en el sistema operativo. Presenta una versión depurada de la información y elimina cualquier dato incriminatorio.
- **Proxys.** Se puede distribuir a un proxy destino que aparente ser legítimo, o distribuirlo entre varios proxys.

# Comprender la Defensa

## Defensa en Profundidad

### Activos, Vulnerabilidades y Amenazas

- **Activos.** Cualquier elemento de valor para una organización que debe ser protegido, incluyendo servidores, dispositivos de infraestructura, terminales y datos.
- **Vulnerabilidades.** Debilidad en un sistema o en su diseño que podría ser aprovechada por un actor de amenaza.
- **Amenazas.** Cualquier peligro potencial para un activo.

### Identificación de los Activos

Los activos son la colección de todos los dispositivos e información que posee o administra una organización. Constituyen la superficie de ataque a la que podrían apuntar los agentes de amenaza.

La administración de activos consiste en inventariarlos y luego implementar políticas y procedimientos para protegerlos.

### Clasificación de Activos

La clasificación de activos asigna los recursos de una organización en grupos según características comunes. 

- **Paso 1.** Determinar la categoría adecuada de identificación de activos: información, software, físicos o servicios.
- **Paso 2.** Establecer la rendición de cuentas de los activos mediante la identificación del propietario de cada activo de información y pieza de software.
- **Paso 3.** Determine los criterios para la clasificación: confidencialidad, valor, tiempo, derechos de acceso, destrucción.
- **Paso 4.** Implementar un esquema de clasificación

### Etapas del Ciclo de Vida de los Activos

- **Adquisición.** La organización compra los activos según las necesidades. El activo se agrega al inventario de la organización.
- **Implementación.** El activo se ensambla e inspecciona para detectar defectos u otros problemas. Se le instalan etiquetas o códigos con fines de seguimiento. El activo se mueve del inventario a en uso.
- **Utilización.** Etapa más larga del ciclo. Su rendimiento se verifica continuamente. Incluye las actualizaciones, correcciones con parches, compra de nuevas licencias y auditorías de cumplimiento.
- **Mantenimiento.** Ayuda a extender la vida productiva de un activo. El personal puede modificar o actualizar el activo.
- **Eliminación.** Al final de su vida productiva, el activo debe eliminarse y todos los datos de este.

### Identificación de Vulnerabilidades

La identificación de amenazas le brinda a una organización una lista de probables amenazas en un entorno determinado.

La identificación de vulnerabilidades en una red requiere comprender las aplicaciones importantes que se utilizan, así como las diferentes vulnerabilidades de esa aplicación y del hardware.

### Identificación de Amenazas

Las organizaciones deben emplear un enfoque de defensa profunda para identificar amenazas y proteger activos vulnerables. Utiliza múltiples capas de seguridad en el perímetro de la red, dentro de la red y en los dispositivos finales de la red.

### Estrategias de Defensa en Profundidad

- **Capas.** Se deben establecer múltiples capas de protección, creando una barrera de múltiples defensas que trabajen juntas para prevenir los ataques.
- **Limitar.** Una organización debe restringir el acceso al mínimo requerido para que el usuario realice su trabajo.
- **Diversidad.** Las capas deben ser diferentes para que, si se penetra una capa, la misma técnica no funcione con las demás capas.
- **Oscuridad.** Oscurecer la información puede proteger los datos. Una organización no debe revelar ninguna información que los ciberdelincuentes puedan utilizar para identificar qué SO está ejecutando un servidor o la marca de equipo o software que utiliza.
- **Simplicidad.** La complejidad no garantiza seguridad. Si los empleados no entienden cómo configurar correctamente una solución, los ciberdelincuentes pueden poner en peligro esos sistemas.

## Gestión de Operaciones de Ciberseguridad

### Gestión de la Configuración

La gestión de la configuración se refiere a identificar, controlar y auditar la implementación y cualquier cambio realizado en la línea de base establecida de un sistema.

La configuración de referencia incluye los ajustes que se configuran para un sistema que proporciona la base para todos los sistemas similares.

Los recursos de configuración documentados pueden incluir mapas de red, diagramas de cableado, especificaciones de configuración de aplicaciones, esquemas de IP, entre otros.

La configuración de archivos de registro, auditoría, cambio de nombres de cuenta y contraseña por defecto e implementación de políticas de cuenta y control de acceso a nivel de archivos refuerza el sistema operativo.

### Archivos de Registro

Un registro documenta todos los eventos a medida que ocurren. Las organizaciones deben considerar un proceso de gestión de registros.

La administración de los datos de registro de seguridad informática debe determinar los procedimientos para generación, transmisión, almacenamiento, análisis y eliminación de archivos de registro.

Los registros del sistema operativo registran eventos vinculados a acciones que tengan que ver con este. Los registros de seguridad de las aplicaciones detectan actividades maliciosas, útiles para auditoría y proporcionan documentación que evidencia el cumplimiento de leyes y requisitos normativos.

## Políticas, Regulaciones y Estándares de Seguridad

### Políticas de la Empresa

Las políticas empresariales son las pautas que desarrollan las organizaciones para regir sus acciones y las de sus empleados. 

- **Políticas de la compañía.** Establece normas de conducta y responsabilidades de empleados y empleadores. Protege los derechos de los trabajadores y los intereses empresariales de los empleadores.
- **Políticas de empleado.** Creados y mantenidos por recursos humanos para identificar salarios, pagos, beneficios, horarios, vacaciones y más. A menudo se entrega a empleados nuevos para leer y firmar.
- **Políticas de seguridad.** Identifican un conjunto de objetivos de seguridad para una empresa, definen las reglas de comportamiento de usuarios y administradores, y especifican los requisitos del sistema.

### Política de Seguridad

Las políticas de seguridad se usan para informar a los usuarios, personal y gerentes los requisitos de una organización para proteger la tecnología y los activos de información. También especifica los mecanismos necesarios para cumplir con los requisitos de seguridad y proporciona un patrón de referencia para adquirir, configurar y auditar el cumplimiento normativo de sistemas informáticos y redes.

# Vulnerabilidades, Amenazas y Ataques de Red

## Detalles de la PDU de IP

### IPv4 e IPv6

IP fue diseñado como un protocolo sin conexión (no establece sesión, no hay una coordinación entre origen y destino) de capa 3. Proporciona las funciones necesarias para enviar un paquete de un host de origen a uno de destino mediante un sistema de redes interconectado sin validar si la dirección IP de origen coincide con la que figura en el paquete.

Los atacantes pueden enviar paquetes con direcciones IP falsas y alterar otros campos del encabezado IP.

### Encabezado del Paquete IPv4

Estructura de 20-60 bytes que contiene información sobre el paquete ubicada al inicio de cada paquete IPv4.

!image.png

#### Versión (Version)

Contiene un valor binario de 4 bits establecido en “0100” que lo identifica como un paquete de IPv4.

#### Longitud del Encabezado de Internet (Internet Header Length)

Campo de 4 bits que contiene la longitud del encabezado IP como incrementos de 32 bits. El mínimo valor es 5, porque 20 bytes es el mínimo tamaño para un encabezado IP. El máximo valor es 15, dando como resultado 60 bytes.

#### Differentiated Services (DiffServ, DS)

Antiguamente conocido como el campo de Tipo de Servicio (Type of Service, ToP). Campo de 8 bits usado para calidad de servicio (QoS), determinando prioridades de los paquetes.

En 1998 se dividió en dos: 6 bits para Differentiated Services Code Point (DSCP) y 2 bits para Explicit Congestion Notification (ECN).

DSCP se utiliza para determinar la prioridad que tiene un paquete, mientras que ECN habilita las notificaciones de congestión, permitiendo bajar la velocidad de envío de los emisores antes de llegar a la pérdida de paquetes. El receptor puede establecer el ECN para esto.

#### Longitud Total (Total Length)

Campo de 16 bits (2 bytes) que indica el tamaño total del paquete IP (cabecera y datos) en bytes. El tamaño mínimo es 20 bytes (si no se tienen datos), y el máximo es 65 535 bytes.

#### Identificación (Identification)

Si el paquete IP es fragmentado, cada fragmento del paquete tendrá el mismo valor de 16 bits del campo de identificación. La generación de este valor depende del sistema operativo.

#### IP Flags

Son 3 bits usados para la fragmentación de la siguiente forma:

- El primer bit está reservado y siempre debe estar establecido en 0.
- El segundo bit, llamado el bit DF (Don’t Fragment) indica si el paquete debe o no ser fragmentado. Este bit también es usado para el Path MTU Discovery (PMTUD). Si el paquete es muy grande para el enlace y el bit DF está establecido, el router suelta el paquete y envía un mensaje ICMP que diga “Fragmentation Needed” hacia el servidor.
- El tercer bit, llamado el bit MF (More Fragments) está establecido en todos los fragmentos de paquete menos en el último.

#### Desplazamiento de Fragmentos (Fragment Offset)

Campo de 13 bits que indica la posición del fragmento (antiguo nombre para los paquetes IP) en el paquete IP fragmentado original. Está medido en unidades de 8 bytes. El máximo valor es 8191 (indica que está en el byte 65 528), dejando 7 bytes para un fragmento payload final de hasta 7 bytes.

#### Time to Live (TTL)

Cada vez que un paquete IP pasa por un router, este campo de 8 bits se disminuye en 1. Cuando alcanza el valor 0, el router descarta el paquete y manda un mensaje ICMP time-exceeded al emisor. Evita que los paquetes se queden en un bucle. A veces se usa como mecanismo de seguridad, estableciéndolo en 1 para que el paquete no pueda salir de la red.

#### Protocolo (Protocol)

Campo de 8 bits que dice qué protocolo de capa superior está encapsulado en el paquete. Indica el tipo de payload (carga útil), lo que permite que la capa de red transmita de forma adecuada los datos al protocolo de capa superior apropiado. Algunos valores comunes son: ICMP (1), TCP (6), UDP (17).

#### Suma de Verificación del Encabezado (Header Checksum)

Campo de 16 bits que almacena la checksum (suma de verificación). Corresponde a un valor que es calculado basándose en el contenido del encabezado IP del paquete, utilizado para determinar si ocurrieron errores durante la transmisión.

El receptor puede utilizarlo para detectar errores en la cabecera. No valida los datos del payload. Al pasar por un router, como el TTL disminuye su valor, el router debe recalcular la header checksum. Así se asegura la integridad de la cabecera durante todo el viaje.

#### Dirección IPv4 de Origen

Valor de 32 bits que representa la dirección IPv4 de origen del paquete, que siempre es una dirección unicast.

#### Dirección IPv4 de Destino

Valor de 32 bits que representa la dirección IPv4 de destino del paquete.

#### Opciones (Options)

Es un campo que varía de longitud entre 0 y un múltiplo de 32 bits. Si los valores de opción no llegan a un múltiplo de 32 bits, se agregan ceros para convertir el contenido de ese espacio en un múltiplo de 32 bits.

Usualmente son descartados por los routers, ya sea por razones de seguridad (algunas opciones pueden revelar la topología de red), o de rendimiento (los paquetes IP con opciones deben ser procesados por el CPU del router).

### Encabezado del Paquete IPv6

Existen ocho campos en el encabezado IPv6. La cabecera tiene un tamaño de 40 bytes.

!image.png

#### Versión

Valor de 4 bits establecido en “0110”, que lo identifica como paquete IPv6.

#### Clase de Tráfico (Traffic Class)

Campo de 8 bits equivalente al campo Differentiated Services (DS) de IPv4. Contiene valor DSCP de 6 bits y ECN de 2 bits, utilizado para controlar la congestión del tráfico.

#### Etiqueta de Flujo (Flow Label)

Campo de 20 bits que sugiere que todos los paquetes con la misma etiqueta de flujo sean manejados de la misma manera por los routers. Se puede utilizar para indicar a los routers y switches que deben mantener la misma ruta para el flujo de ciertos paquetes, a fin de evitar que estos se reordenen.

#### Longitud de la Carga Útil (Payload Length)

Este campo de 16 bits indica solo la longitud de la porción de datos o de la payload (carga útil) del paquete IPv6 en octetos (bytes). Incluye cabeceras de extensión, pero no la IPv6.

#### Próximo Encabezado (Next Header)

Este campo de 8 bits equivale al campo de Protocolo de IPv4. Es un valor que indica el tipo de payload de datos que contiene el paquete.

#### Límite de Saltos (Hop Limit)

Campo de 8 bits que sustituye al campo TTL de IPv4. Cada router que reenvía el paquete reduce este valor en 1. Cuando llega a cero, se descarta el paquete y se envía un mensaje ICMPv6 de “Tiempo excedido” al host de origen, indicando que el paquete no llegó al destino por exceder el límite de saltos.

#### Dirección IPv6 de Origen

Campo de 128 bits que identifica la dirección IPv6 de origen del host emisor.

#### Dirección IPv6 de Destino

Campo de 128 bits que identifica la dirección IPv6 del host receptor.

## Vulnerabilidades de IP

- **Ataques de ICMP.** Los atacantes utilizan paquetes de echo (pings) del Internet Control Message Protocol (ICMP) para detectar subredes y hosts en una red protegida, y luego llevar a cabo ataques DoS de saturación y modificar las tablas de enrutamiento de los hosts.
- **Ataques DoS.** Atacante intenta impedir que usuario legítimo tenga acceso a información o servicios.
- **Ataques DDoS.** Similar a DoS, pero llevado a cabo simultánea y coordinadamente desde varias máquinas de origen.
- **Ataques de Suplantación de Dirección.** Se suplanta la dirección IP de origen en un intento de llevar a cabo blind spoofing o non-blind spoofing.
- **Ataques Man-in-The-Middle (MiTM).** Los atacantes se ubican entre dos nodos para monitorear, capturar y controlar la comunicación en forma transparente. Pueden hacer monitoreo pasivo (eavesdropping), inspeccionar paquetes o alterarlos y reenviarlos.
- **Secuestros de sesiones.** Los atacantes obtienen acceso a la red física y usan ataques de tipo MiTM para secuestrar la sesión.

### Ataques de ICMP

El protocolo ICMP fue diseñado para llevar mensajes de diagnóstico e informar condiciones de error cuando no están disponibles las rutas, hosts y puertos. Son generados por los dispositivos cuando ocurre un error en la red.

El comando ping es un mensaje ICMP de tipo “solicitud echo”, utilizado para verificar la conectividad con algún destino.

Los atacantes utilizan ICMP para reconocimiento y análisis, también para para DoS y DDoS. ICMPv4 e ICMPv6 son susceptibles a ataques similares.

Las redes deben tener filtros estrictos en la ACL de ICMP en el perímetro de la red para evitar sondeos ICMP desde internet.

- **”Echo Request” y “Echo Reply”.** Se utiliza para la verificación de conectividad y ataques DoS.
- **”Unreachable”.** Utilizado para realizar ataques de reconocimiento y escaneo de red.
- **”Mask Reply”.** Se utiliza para mapear una red IP interna.
- **”Redirects”.** Se utiliza para lograr que un host de destino reenvíe todo el tráfico a través de un dispositivo comprometido y crear un ataque de MiTM.
- **”Router Discovery”.** Se utiliza para inyectar entradas de rutas falsas en la tabla de routing de un host objetivo.

### Ataques de Amplificación y Reflexión

Usados para crear ataques DoS. Una técnica de amplificación muy usada es “Smurf Attack”.

- **Amplificación.** El atacante reenvía mensajes ICMP echo request a muchos hosts con dirección de IP de origen de la víctima.
- **Reflexión.** Todos los objetivos responden a la dirección IP suplantada, sobrecargándola.

Los atacantes también utilizan ataques de agotamiento de recursos. Actualmente se están utilizando ataques de amplificación y reflexión basados en DNS y NTP.

### Ataques de Suplantación de Dirección

Se producen cuando un atacante crea paquetes con información suplantada de la dirección IP de origen, ya sea para ocultar su identidad o para hacerse pasar por otro usuario legítimo, logrando así acceder a datos a los que, de otro modo, no podría acceder, o para eludir configuraciones de seguridad.

Los ataques de suplantación (spoofing) IP suelen formar parte de otro ataque denominado smurf. Los ataques de spoofing pueden ser de tipo blind (a ciegas) o non-blind (con visibilidad):

- **Non-blind spoofing.** El atacante puede observar el tráfico que está siendo enviado entre el host y el objetivo. El atacante utiliza esta suplantación para inspeccionar el paquete de respuesta del objetivo. Esta técnica determina el estado de un firewall y la predicción del número de secuencia. También puede secuestrar una sesión autorizada.
- **Blind spoofing.** El atacante no puede ver el tráfico que se envía entre el host y el objetivo. Este tipo de ataque se utiliza en ataques DoS.

Los ataques de suplantación de dirección MAC se utilizan cuando los atacantes tienen acceso a la red interna. Los atacantes alteran su dirección MAC de su host para que coincida con la MAC objetivo. Luego, el atacante envía una trama con su dirección MAC recién configurada.

Cuando el switch recibe la trama, examina la MAC origen y sobrescribe la entrada actual en la tabla MAC, asignando la dirección MAC al puerto nuevo, y reenvía las tramas destinadas al objetivo hacia el atacante.

Otros ejemplos de suplantación son la suplantación de aplicaciones o servicios.

## Vulnerabilidades de TCP y UDP

### Encabezado de Segmento TCP

TCP es un protocolo de transporte (capa 4) confiable, que establece conexión antes de enviar datos y todo lo que envía recibe un acknowledge. Su encabezado mide al menos 20 bytes.

!image.png

#### Source Port

 Campo de 16 bits que especifica el número de puerto del emisor.

#### Destination Port

Campo de 16 bits que especifica el número de puerto del receptor.

#### Sequence Number

Campo de 32 bits que indica cuánta información se está enviando durante la sesión TCP. Cuando se establece una nueva conexión TCP (3 way handshake), la secuencia inicial es un valor aleatorio de 32 bits e indicará el número el número del byte que se transmite a partir de este valor. El receptor usará ese número de secuencia y envía de respuesta un acknowledgment. Analizadores de protocolos como Wireshark usualmente trabajan con un relative sequence number de 0 porque es más fácil de leer.

#### Acknowledgment Number

Campo de 32 bits utilizado por el receptor para pedir el siguiente segmento TCP. Su valor será el valor del sequence number incrementado en 1.

#### Data Offset (DO)

Campo de 4 bits, conocido también como el header length, indica la longitud de la cabeza de la cabecera, permitiendo saber dónde comienza el payload del segmento.

Utiliza como unidad de medida palabras (32 bits). Su mínimo valor es 5 (0101), pues el mínimo valor de una cabecera TCP es 20 bytes. Su máximo valor es 15 (60 bytes).

#### Reserved (RSV)

Es un campo de 3 bits que se encuentra reservado y no es usado. Generalmente estos tres bits están establecidos en cero.

#### Flags

También llamados bits de control, es un campo de 9 bits utilizado para establecer conexiones, enviar datos y finalizar conexiones.

- **URG (Urgent pointer).** Campo indicador urgente significativo. Cuando está establecido, los datos deben ser tratados como prioritarios sobre otros datos.
- **ACK (Acknowledgment).** Usado para reconocimiento.
- **PSH (Push function).** Este le dice a la aplicación que los datos deben ser transmitidos inmediatamente y no que no queremos esperar a completar el segmento TCP completo.
- **RST (Reset).** Esto resetea la conexión. Cuando se recibe, se debe terminar la conexión inmediatamente. Solo se usa cuando hay errores irrecuperables y no es una forma normal de finalizar la conexión TCP.
- **SYN (Synchronize).** Usado como primer paso del three way handshake. Es usado para establecer conexiones y para establecer el sequence number inicial.
- **FIN (Finish).** Este bit de finalización se utiliza para finalizar la conexión TCP. TCP es full duplex, por lo que los dos nodos tendrán que utilizar el bit FIN para finalizar normalmente la conexión.

#### Window

Campo de 16 bits que especifica la cantidad de datos en bytes que el receptor puede recibir. Se usa para poder regular el flujo de tráfico. Permite tráfico eficiente al permitir a los emisores transmitir múltiples segmentos antes de hacer una petición de ACK.

Para redes modernas, se usa el Window Scale option, del campo de opciones, que permite escalar hasta un giga byte. El factor de escalado es negociado durante el 3-way handshake.

#### Checksum

Campo de 16 bits utilizado para la suma de verificación, comprobando que el TCP header esté bien o no.

#### Urgent Pointer

Campo de 16 bits usado cuando el bit URG está activo. Indica el byte específico donde los datos urgentes terminan en el segmento actual. No es usado en la actualidad dado que URG está obsoleto.

#### Options

Campo opcional de hasta 40 bytes.

### Servicios TCP

#### Entrega Confiable

TCP integra acuses de recibo para garantizar la entrega en vez de confiar en protocolos de capa superior para detectar y resolver errores. Si no se recibe un acuse de recibo en un tiempo prudente, el emisor retransmite los datos. Requerir acuses de recibo puede retrasar las comunicaciones.

Algunos protocolos de la capa de aplicación que hacen uso de la confiablidad de TCP son HTTP, SSL/TLS, FTP y transferencia de zona DNS.

#### Control de Flujo

El TCP implementa control de flujo para abordar el problema de los retrasos. En lugar de confirmar la recepción de un solo segmento, se confirma la recepción de múltiples segmentos con un solo acuse de recibo.

#### Comunicación con Estado

La comunicación de TCP con estado entre dos partes ocurre durante el three-way handshake (intercambio de señales de tres vías). Antes de que los datos sean transferidos utilizando TCP, se debe habilitar con el three-way handshake. Si ambas partes lo aceptan, se pueden enviar y recibir datos utilizando TCP.

#### Intercambio Three-way Handshake de TCP

- El cliente de origen solicita una sesión de comunicación de cliente a servidor con el servidor. Se manda un mensaje SYN y se envía el sequence number.
- El servidor hace acuse de recibo (ACK) de la sesión de comunicación cliente-servidor y solicita una sesión de comunicación de servidor-cliente. Se recibe el SYN y se envía un SYN + ACK, donde se confirma el ACK al aumentar el valor del sequence number recibido en uno y manda su propio sequence number.
- El cliente de origen hace acuse de recibo de la sesión de comunicación de servidor-cliente.

!image.png

### Ataques de TCP

#### Ataques de Saturación de TCP SYN

En estos se ataca la comunicación de three-way handshake de TCP. El atacante satura al objetivo enviando constantemente paquetes de solicitud TCP SYN con una dirección IP de origen falsificada de forma aleatoria.

El objetivo responde con un paquete TCP SYN-ACK a la dirección IP falsa y espera el paquete TCP ACK que nunca llega, dejando demasiadas conexiones TCP medio abiertas.

Cuando el usuario legítimo envía un paquete TCP SYN, el host objetivo deniega el servicio.

#### Ataque de Restablecimiento de TCP

Un ataque de este tipo puede utilizarse para terminar las comunicaciones de TCP entre dos hosts enviando un paquete falso de TCP RST a uno o ambos terminales.

La finalización normal de TCP utiliza el proceso de intercambio de cuatro vías (TCP 4-way handshake):

- Cuando el cliente no tiene más datos para enviar, se manda un segmento con el flag FIN establecido.
- El servidor envía un ACK del FIN para terminar la sesión cliente-servidor.
- El servidor envía, un FIN al cliente para terminar la sesión servidor-cliente.
- El cliente responde con un ACK para dar acuse de recibo desde el servidor.

#### Secuestro de Sesión TCP

En este tipo de ataques, se toma el control de un host autenticado mientras se comunica con el objetivo.

El atacante tiene que suplantar la dirección IP de un host, predecir el siguiente número de secuencia y enviar un ACK al otro host.

Si se tiene éxito, el atacante puede enviar, pero no recibir datos desde el dispositivo objetivo ya que el servidor enviará la respuesta a la víctima original (blind hijacking).

### Encabezado y Funciones del Segmento UDP

UDP es comúnmente utilizado por los protocolos DNS, DHCP, TFTP (Trivial File Transfer Protocol), NFS (Network File System) y SNMP (Simple Network Management Protocol). También lo utilizan aplicaciones en tiempo real como transmisión multimedia o VoIP. UDP es un protocolo sin conexión de la capa de transporte.

UDP es llamado poco confiable, no implementa encriptación por defecto. Las aplicaciones que lo utilizan proporcionan confiabilidad. Su cabecera es de 8 bytes.

!image.png

Consta de cuatro campos principales de 16 bits. El checksum solo indica si ocurrió algún error en el transporte, pero no lo corrige.

### Ataques de UDP

La falta de encriptación de UDP permite que cualquiera que vea el tráfico pueda modificarlo y enviarlo a su destino.

#### Ataques de Saturación UDP

Consumen los recursos de la red. Se logran usando herramientas como UDP Unicorn o Low Orbit Ion Cannon que envían avalanchas de paquetes UDP, a menudo desde un host suplantado, a un servidor en la subred.

El programa analiza todos los puertos conocidos intentado encontrar puertos cerrados, haciendo que el servidor de una respuesta de “puerto ICMP inaccesible”.

Debido a la gran cantidad de puertos cerrados, se genera mucho tráfico en el segmento, utilizando gran parte del ancho de banda. Similar a un DoS.

## Servicios IP

### Vulnerabilidades de ARP

Los hosts transmiten una solicitud de ARP hacia otros hosts del segmento de red para determinar la dirección MAC de un host con una dirección IP específica. El host con IP coincidente con la de la solicitud ARP envía una respuesta llamada ARP gratuita.

Un atacante puede envenenar la caché ARP de los dispositivos de red local. El objetivo es asociar su MAC con la IP de la puerta de enlace predeterminada en la caché ARP de los hosts del segmento LAN.

El envenenamiento de caché ARP se puede utilizar para lanzar varios ataques man-in-the-middle.

### Ataques DNS

#### Ataques de Servidores DNS Abiertos

Una resolución DNS (DNS resolver) abierta es un servidor DNS abierto públicamente, como Google DNS (8.8.8.8), el cual responde a las preguntas del cliente fuera de su dominio administrativo. Los open resolvers responden a consultas de cualquier persona de internet.

- **Ataque de envenenamiento de caché DNS.** Los atacantes envían registros de recursos (RR) falsificados a un servidor DNS para redirigir a los usuarios de sitios legítimos a sitios maliciosos.
- **Ataques de amplificación y reflexión de DNS.** Los atacantes envían miles de mensajes DNS a las open resolver usando la IP de la víctima como remitente. Los servidores DNS envían las respuestas (mucho más grandes) hacia la víctima, saturando su ancho de banda.
- **Ataques de utilización de recursos DNS.** Este ataque DoS consume todos los recursos disponibles para afectar negativamente a las operaciones del DNS open resolver.

#### Ataques DNS Sigilosos

- **Fast Flux.** Los atacantes utilizan esta técnica para ocultar los sitios de phishing y malware que les pertenecen mediante un cambio rápido de dirección IP asociadas al DNS. Cuando un sistema de seguridad intenta bloquear dicha IP, la dominio está apuntando a otra totalmente diferente.
- **Double IP Flux.** Los atacantes cambian las IP del servidor final y de los servidores de nombres (Authoritative Name Servers). Esto significa que cambian la IP que resuelve el dominio (igual que Fast Flux) y cambian las IP de los servidores DNS que dicen dónde está el dominio.
- **Algoritmo de Generación de Dominio.** Los atacantes utilizan esta técnica para generar aleatoriamente nombres de dominio que puedan utilizarse como puntos de encuentro de sus servidores de mando y control (command-and-control server).

#### Ataques de DNS Domain Shadowing

Ocurre cuando un atacante compromete las credenciales de la cuenta de un registrador de dominios de una víctima legítima y crea múltiples subdominios para utilizarlos durante los ataques.

Los subdominios creados generalmente apuntan a servidores maliciosos sin alertar al propietario real del dominio principal.

#### DNS Tunneling

El tráfico DNS está permitido sin restricciones por los firewalls. Los atacantes registran un dominio y configuran un servidor DNS, luego se infecta un dispositivo de la red víctima con malware para realizar la tunelización. El malware toma los datos que desea enviar y los codifica dentro de una consulta DNS, como un subdominio.

Para evitar esto, se debe utilizar un filtro que inspeccione el tráfico DNS.

### Ataques de DHCP

Los servidores DHCP proporcionan de manera dinámica información de configuración IP a los clientes. Para lograrlo, se lleva a cabo un 4-way handshake, conocido como DORA (Discover, Offer, Request, Acknowledge).

Se empieza con un DHCP Discover en broadcast. El servidor DHCP responde con una DHCP Offer en unicast, que incluye información de direccionamiento que el cliente puede usar. El cliente luego transmite una DHCP Request en broadcast para decirle al servidor que acepta la oferta y el servidor responde mediante unicast con un acuse de recibo aceptando la solicitud.

#### Ataque de Suplantación DHCP (DHCP spoofing)

Un ataque de suplantación DHCP se produce cuando un servidor DHCP malicioso se conecta a la red y proporciona parámetros falsos de configuración IP a los clientes legítimos.

- **Default Gateway incorrecta.** El atacante proporciona una gateway incorrecta o la dirección IP de su propio host para crear un ataque de MiTM.
- **Servidor DNS incorrecto.** El atacante proporciona una dirección del servidor DNS incorrecta que dirige al usuario a un sitio web malicioso.
- **Dirección IP incorrecta.** El atacante proporciona una IP no válida, IP de puerta de enlace no válida o ambas. Luego, el atacante crea un DoS en el cliente DHCP.

# Amenazas de Seguridad en Capa de Enlace de Datos

## Amenazas de Seguridad en Capa 2

### Vulnerabilidades en Capa 2

!image.png

Las soluciones comunes (VPN, firewall, dispositivos IPS) protegen las las capas 3 hasta la siete. Si la capa 2 se ve comprometida, todas las demás capas también se verán afectadas.

La seguridad es tan sólida como el enlace más débil del sistema, y la capa 2 es considerada el enlace más débil.

### Categoría de Ataque en el Switch

- **Ataques de tabla MAC.** Incluye ataques de inundación de direcciones MAC.
- **Ataques de VLAN.** Incluye ataques de VLAN hopping y VLAN double-tagging. También incluye ataques entre dispositivos en una VLAN común.
- **Ataques de DHCP.** Incluye ataques de DHCP starvation y DHCP spoofing.
- **Ataques ARP.** Incluye ataques de suplantación de ARP y los ataques de envenenamiento de ARP.
- **Ataques de suplantación de direcciones.** Incluye ataques de suplantación de direcciones MAC e IP.
- **Ataques STP.** Incluye ataques de manipulación al Spanning Tree Protocol (STP).

### Técnicas de Mitigación de Ataques de Switch

- **Seguridad de puertos.** Previene muchos tipos de ataque, incluidos los ataques de inundación de direcciones MAC y los ataques de hambre DHCP.
- **Detección DHCP.** Previene ataques de suplantación de identidad y de agotamiento de DHCP.
- **Inspección ARP Dinámica (DAI).** Previene suplantación de ARP y los ataques de envenenamiento de ARP.
- **Protección de IP de origen (IPSG).** Impide los ataques de suplantación de direcciones MAC e IP.

Estas técnicas de mitigación no serán efectivas si los protocolos de administración no están asegurados. Se recomiendan las siguientes estrategias:

- Utilice siempre variantes seguras de protocolos de administración como SSH, Secure Copy Protocol (SCP), Secure FTP (SFTP) y SSL/TLS.
- Considere usar la red de administración fuera de banda para administrar dispositivos.
- Use una VLAN de administración dedicada que solo aloje el tráfico de administración.
- Use ACL para filtrar el acceso no deseado.

## Ataque de Tablas de Direcciones MAC

Para tomar decisiones de reenvío, un Switch LAN de capa 2 crea una tabla basada en las direcciones MAC de origen en las tramas recibidas. Esto se llama una tabla de direcciones MAC y se almacenan en memoria, permitiendo el intercambio de frames más eficientemente.

### Fundamentos del Switch: Switch Learning y Forwarding

Un switch ethernet de capa 2 utiliza direcciones MAC de capa 2 para tomar decisiones de reenvío. Desconoce por completo los datos (protocolo) que se transporta en la porción de datos de la trama.

El switch construye dinámicamente la tabla de direcciones MAC, examinando la dirección MAC de origen de las tramas recibidas en un puerto, luego reenvía las tramas buscando una coincidencia entre la dirección MAC destino y una entrada en la tabla de direcciones MAC.

Cada trama que ingresa a un switch se analiza en busca de información nueva, examinando la dirección MAC de origen y el puerto por donde ingresó. Si la MAC origen no existe, se agrega a la tabla junto con el puerto entrante. Si la MAC origen existe, actualiza el temporizador de actualización para esa entrada en la tabla. Por defecto, se mantienen las entradas en la tabla por 5 minutos.

Si la dirección MAC destino es una dirección unicast, el switch buscará alguna coincidencia entre esta y una entrada en su tabla de direcciones MAC. Si la dirección MAC destino no se encuentra en la tabla, el switch la reenviará por todos los puertos excepto por el de entrada (unicast desconocido).

Cuando el conmutador recibe una trama y tiene la MAC destino en su tabla de direcciones MAC, filtra la trama y la reenvía a través de un único puerto.

### Inundación de la Tabla de Direcciones MAC

Todas las tablas MAC tienen un tamaño fijo, por lo que un switch puede quedarse sin espacio para guardar direcciones MAC.

Los ataques de inundación de direcciones MAC aprovechan esta limitación al bombardear el switch con direcciones MAC de origen falsas hasta llenar la tabla. Cuando esto ocurre, el switch trata al frame como una unidifusión desconocida y comienza a inundar todo el tráfico entrante por todos los puertos en la misma VLAN, sin hacer referencia a la tabla MAC.

Esta condición ahora permite a un atacante capturar todas las tramas enviadas desde un host a otro en la LAN local o VLAN local. Dado que el tráfico solo se inunda dentro de la VLAN o LAN local, el atacante solo podrá capturar el tráfico dentro de esta, a la cual deberá estar conectado.

!image.png

### Mitigación de Ataques de Tabla de Direcciones MAC

Herramientas como macof permiten inundar un switch con hasta 8000 frames falsas por segundo, afectando tanto al switch local como a los switches de capa 2 conectados. Cuando la tabla de direcciones de un switch capa 2 está llena, empieza a desbordar todos los puertos, incluidos los conectados a otros switches.

Para mitigar los ataques de desbordamiento de la tabla de direcciones MAC, se implementa port security, que permite al puerto aprender sólo un número específico de fuentes de direcciones MAC.

## Ataques a la LAN

### Ataques de Salto de VLAN (VLAN hopping)

Este tipo de ataques permiten que una VLAN sea capaz de ver el tráfico de otra VLAN sin cruzar primero un router. En un ataque de salto de VLAN básico, el atacante configura un host para que actúe como switch, para aprovechar la función de Dynamic Trunking Protocol (DTP) habilitada de forma predeterminada en la mayoría de puertos de switch.

El atacante configura el host para falsificar la señalización 802.1Q y la señalización del DTP propietario de Cisco al enlace troncal con el switch de conexión. Si es exitoso, el switch establece un enlace troncal con el host, permitiendo al atacante acceder a todas las VLAN en el switch. El atacante puede enviar y recibir tráfico en cualquier VLAN, saltando efectivamente entre las VLAN.

!image.png

### Ataques de Doble Etiquetado de VLAN

Un atacante podría insertar una etiqueta 802.1Q oculta dentro de una trama que ya contiene una, permitiendo a esta trama dirigirse a una VLAN que la etiqueta 802.1Q original no especificaba.

- **Paso 1.** El atacante envía una trama 802.1Q de doble etiqueta al switch, donde el encabezado externo tiene la etiqueta VLAN del atacante, que es la misma que la VLAN nativa del puerto de enlace troncal.
- **Paso 2.** El frame llega al primer switch, que mira la etiqueta 802.1Q de 4 bytes, notando que está destinado a la VLAN nativa, entonces lo reenvía a todos los puertos VLAN nativos después de quitar la etiqueta VLAN. El frame no es re etiquetado porque es parte de la VLAN nativa. En este punto, la etiqueta VLAN interna todavía está intacta y no ha sido inspeccionada por el primer switch.
- **Paso 3.** La trama llega al segundo switch, que no tiene conocimiento de que era para la VLAN nativa. El emisor no etiqueta el tráfico de la VLAN nativa. El segundo switch solo mira la etiqueta interna 802.1Q que insertó el atacante y ve que el frame está destinado a la VLAN de destino. El segundo switch envía el paquete al puerto víctima o lo satura, dependiendo de si existe una entrada en la tabla de MAC para el host víctima.

Un ataque de doble etiqueta a una VLAN es unidireccional y funciona únicamente cuando el atacante está conectado a un puerto que reside en la misma VLAN que la VLAN nativa del puerto troncal. La idea es que el doble etiquetado permite al atacante enviar datos a hosts o servidores en una VLAN que, de otro modo, se bloquearía por algún tipo de configuración de control de acceso. Presumiblemente también se permitirá el tráfico de retorno, dándole al atacante la capacidad de comunicarse con los dispositivos en la VLAN normalmente bloqueada.

### Mitigación de Ataques de VLAN

Se pueden evitar los ataques de salto de VLAN y doble etiquetado mediante la implementación de las siguientes pautas de seguridad troncal:

- Deshabilitar troncal en todos los puertos de acceso.
- Deshabilitar troncal automático en enlaces troncales para poder habilitarlos de manera manual.
- Asegurarse de que la VLAN nativa solo se utilice para los enlaces troncales.

### Mensajes DHCP

Los servidores DHCP proporcionan dinámicamente la información de configuración IP a los clientes (dirección IP máscara de subred, default gateway, servidor DNS y más).

!image.png

Los dos tipos de ataques DHCP son starvation y spoofing. Ambos pueden ser mitigados implementando DHCP snooping.

- **Ataque DHCP Starvation.** El objetivo es crear un DoS para conectar clientes. Requieren una herramienta de ataque como Gobbler, que tiene la capacidad de ver todo el alcance de las direcciones IP alquilables e intenta alquilarlas todas, creando mensajes DHCP con dirección MAC falsa.
- **Ataque DHCP Spoofing.** Ocurre cuando un servidor DHCP falso se conecta a la red y proporciona parámetros de configuración de IP falsos a clientes legítimos.

Un servidor no autorizado puede proporcionar una variedad de información engañosa como:

- **Default gateway incorrecta.** Proporciona una gateway no válida o la dirección IP de su host para crear un ataque de MiTM. Esto puede pasar totalmente inadvertido, ya que el intruso intercepta el flujo de datos por la red.
- **Servidor DNS incorrecto.** El servidor fraudulento dirige a los usuarios a sitios web maliciosos.
- **Dirección IP incorrecta.** El servidor fraudulento proporciona una IP no válida que crea un ataque DoS en el cliente DHCP.

### Envenenamiento ARP

Los hosts transmiten solicitudes ARP para determinar la dirección MAC de un host con una dirección IP de destino. Todos los hosts de la subred reciben y procesan la solicitud ARP. El host con IP coincidente manda una respuesta ARP. Un cliente puede enviar una respuesta ARP no solicitada, llamada ARP gratuito.

Otros hosts en la subred almacenan la dirección MAC e IP  contenidas en el ARP gratuito en sus tablas ARP. Un atacante puede enviar un mensaje ARP gratuito que contiene una dirección MAC falsificada a un switch, actualizando su tabla MAC en consecuencia.

En un ataque típico, el atacante envía respuestas ARP gratuitas a otros hosts en la subred con la dirección MAC del atacante y la IP de la puerta de enlace predeterminada, configurando ataques MiTM.

IPv6 utiliza el protocolo de descubrimiento de vecinos (NDP) ICMPv6 para la resolución de direcciones de capa 2.

La falsificación de ARP y la intoxicación de ARP se mitigan mediante la implementación de la inspección dinámica de ARP (DAI).

### Ataques de Suplantación de Dirección IP

Ocurre cuando un atacante secuestra una dirección IP válida de otro dispositivo en la subred o usa una dirección IP aleatoria. Cambian su dirección MAC para que coincida con otra dirección MAC conocida de un host de destino. El switch sobrescribe la entrada actual de la tabla MAC y asigna la dirección MAC al nuevo puerto, reenviando las tramas a este. Cuando el host destino envía tráfico, el switch corrige el error, realineando la dirección MAC al puerto original.

Para evitar que el switch devuelva la asignación a su estado correcto, el atacante puede crear un script que constantemente enviará tramas al switch para que mantenga la información falsificada. Dado que no hay mecanismo de seguridad en la capa 2 que permita al switch verificar la fuente de las direcciones MAC, es vulnerable a estos ataques. Se pueden mitigar mediante la implementación de IP Source Guard (IPSG).

### Ataques STP

Los atacantes de red manipulan el Spanning Tree Protocol (STP) para realizar un ataque falsificando el puente raíz y cambiando la topología de una red. Los atacantes pueden capturar todo el tráfico para el dominio del switch inmediato.

Para realizar un ataque de manipulación de STP, el host atacante transmite unidades de datos de protocolo de puente STP (BPDU, Bridge Protocol Data Units), que contienen cambios de configuración y topología que forzarán los recálculos de árbol de expansión.

Las BPDU enviadas por el host atacante anuncian una prioridad de puente inferior en un intento de ser elegidas como root bridge. Este ataque STP es mitigado implementado BPDU guard en todos los puertos de acceso.

### Reconocimiento CDP

Cisco Discovery Protocol (CDP) es un protocolo de detección de enlaces de capa 2 propietario que está habilitado en todos los dispositivos Cisco de manera predeterminada. Los administradores de red también usan CDP para configurar dispositivos de red y solucionar problemas. Puede ver los vecinos en un dispositivo con el comando de modo configuración global `show cdp neighbors`.

La información CDP, que incluye la dirección IP del dispositivo, versión de software de IOS, plataforma, funcionalidades y la VLAN nativa, se envía a través de puertos habilitados para CDP en transmisiones periódicas, sin cifrar ni autenticar. El dispositivo que los recibe, actualiza su base de datos de CDP.

Para mitigar la explotación de CDP, se debe limitar su uso en los dispositivos o puertos, como en los puertos de borde que se conectan a dispositivos no confiables.

Para deshabilitar globalmente CDP en un dispositivo, utilice el comando `no cdp run`. Para habilitarlo, use `cdp run`.

Para deshabilitar CDP en un puerto, use el comando de configuración de interfaz `no cdp enable`. Para habilitarlo, use `cdp enable`.

Link Layer Discovery Protocol (LLDP), similar a CDP, pero no propietario, también es vulnerable a ataques de reconocimiento. Configure `no lldp run` para deshabilitarlo globalmente. Configure `no lldp transmit` y `no lldp receive` para deshabilitarlo en una interfaz.

# Mitigar Ataques LAN

## Implementar Seguridad de Puertos (Port Security)

### Asegure los Puertos no Utilizados

Se deben proteger todos los puertos (interfaces) del switch antes de implementar el dispositivo para la producción. Un método simple para hacerlo es inhabilitar los puertos del switch que no se utilizan. Se navega a cada puerto y se emite el comando de apagado `shutdown`. Si se debe habilitar el puerto más tarde, lo habilitamos con `no shutdown`. Para configurar un rango de puertos, use el comando:

```bash
Switch(config)#**interface range** *type module/first-number - last-number*
```

### Mitigar los Ataques de la Tabla de Direcciones MAC

El método más eficaz para evitar ataques de saturación de la tabla de direcciones MAC es habilitar el port security. Este limita la cantidad de direcciones MAC válidas permitidas en el puerto.

La seguridad de puertos permite a un administrador configurar manualmente las direcciones MAC para un puerto o permitir que el switch aprenda dinámicamente un número limitado de direcciones MAC. Cuando un puerto configurado con esta recibe una trama, la dirección MAC de origen se compara con la lista de direcciones MAC de origen seguras que se configuraron manualmente o se aprendieron dinámicamente en el puerto.

Al limitar el número de MAC permitidas en un puerto, port security se puede utilizar para controlar el acceso no autorizado a la red.

### Activar Port Security

Port security se habilita con el comando `switchport port-security` de la interfaz del puerto. Este comando solo se puede configurar en puertos de en modo acceso o trunks configurados manualmente.

Los puertos capa 2 del switch están definidos como dynamic auto (troncal encendido) de forma predeterminada, por lo tanto, debemos configurar primero el comando `switchport mode access` de la interfaz.

```bash
S1(config)#interface f0/1
S1(config-if)#switchport mode access
S1(config-if)#switchport port-security
```

Usar el comando **`show port-security interface** *interface*` para mostrar la configuración de seguridad del puerto actual para *interface*.

Las configuraciones por defecto son que el port security está habilitado, el número máximo de direcciones MAC permitidas es 1, el modo de violación está establecido en shutdown y si un dispositivo está conectado al puerto, el switch automáticamente agregará la MAC de este como una MAC segura.

Si un puerto activo está configurado con el comando `switchport port-security` y hay más de un dispositivo conectado a ese puerto, este entrará al estado `err-disable` (error-desactivado).

Estas funciones específicas se pueden configurar. Para más información, use la ayuda contextual con `switchport port-security` en el modo configuración de interfaz.

#### Limitar y Aprender Direcciones MAC

Para modificar el número máximo de direcciones MAC permitidas en un puerto (predeterminado es 1), utilice el comando:

```bash
Switch(config-if)#**switchport port-security maximun** *value*
```

El número máximo de direcciones MAC seguras que se puede configurar depende del switch y del IOS.

El switch se puede configurar para aprender direcciones MAC en un puerto seguro de tres maneras:

- **Configuración manual.** Se configura una dirección MAC estática para cada MAC segura en el puerto mediante:

```bash
Switch(config-if)#**switchport port-security mac-address** *MAC-address*
```

- **Aprendizaje dinámico.** Por defecto, al activar el port security, la fuente MAC actual para el dispositivo se guarda como segura, pero no se agrega la running-config. Si el switch se reinicia, el puerto debe reaprender la MAC del dispositivo.
- **Aprendizaje dinámico — Sticky.** Se configura el switch para aprender dinámicamente la MAC y adherirla a la running-config mediante:

```bash
Switch(config-if)#switchport port-security mac-address sticky
```

Al guardar la configuración en ejecución, la MAC aprendida se quedará en la NVRAM.

Use los comandos `show port-security interface` y `show port-security address` para verificar la configuración.

#### Vencimiento de Port Security

El vencimiento del port security puede usarse para poner el tiempo de vencimiento de las direcciones seguras estáticas y dinámicas en un puerto, permitiendo removerlas sin tener que hacerlo manualmente.

- **Absoluta.** Las direcciones seguras en el puerto se eliminan después del tiempo de caducidad especificado (en minutos).
- **Inactiva.** Las direcciones seguras en el puerto se eliminan si están inactivas durante un tiempo específico.

El vencimiento de direcciones seguras configuradas estáticamente puede ser habilitado o deshabilitado por puerto. Use el comando `switchport port-security aging` para este habilitar o deshabilitar el vencimiento estático para el puerto seguro, o para establecer el tiempo o el tipo de vencimiento.

```bash
Switch(config-if)#switchport port-security aging {**static** | **time** *time* | **type** {**absolute** | **inactivity**}}
```

- **`static`**. Habilita el vencimiento para direcciones seguras configuradas estáticamente en este puerto.
- **`time** *time*`. Especifica el tiempo de caducidad para este puerto. El rango es de 0 a 1440 minutos. Si el tiempo es 0, la caducidad está desactivada para este puerto.
- **`type absolute`**. Establece el tiempo de caducidad absoluto. Todas las direcciones seguras de este puerto caducan exactamente después del tiempo (en minutos) especificado y se eliminan de la lista de direcciones seguras.
- **`type inactivity`**. Establece el tipo de caducidad por inactividad. Las direcciones seguras de este puerto caducan solo si no hay tráfico de datos desde la dirección de origen segura durante el período de tiempo especificado.

#### Modos de Violación de Port Security

Si la dirección MAC de un dispositivo conectado a un puerto difiere de la lista de direcciones seguras, se produce una violación del puerto y el puerto entra en estado `err-disable`.

```bash
Switch(config-if)#**switchport port-security violation** {**shutdown** | **restrict** | **protect**}
```

- **`shutdown`** (predeterminado). El puerto pasa al estado `err-disable` de inmediato, apaga el LED del puerto, aumenta el contador de violaciones y envía un mensaje al syslog. Cuando un puerto seguro se encuentra en estado de `err-disable`, para volver a habilitarlo se ingresan los comandos `shutdown` y `no shutdown`.
- **`restrict`** (restricción). El puerto descarta paquetes con direcciones de origen desconocidas hasta que se eliminen un número suficiente de direcciones MAC seguras para caer por debajo del valor máximo de MAC seguras, o aumentar el valor máximo de MAC seguras. Este modo hace que el contador de infracción de seguridad se incremente y genere un mensaje de syslog.
- **`protect`** (protección). Es el modo menos seguro de los modos de violación de seguridad. Descarta paquetes con MAC de origen desconocidas hasta que se eliminen un número suficiente de MAC seguras o se incremente el número máximo de MAC seguras para estar debajo del valor máximo. No se envía ningún mensaje syslog.

#### Puertos en Estado `err-disable`

Cuando un puerto está apagado y puesto en modo err-disable, no se envía ni se recibe tráfico a través de este y en la consola se muestran una serie de mensajes relacionados con la seguridad del puerto.

El protocolo del puerto y el estado del enlace se cambian a inactivo y el LED del puerto se apaga.

#### Verificar Port Security

Para mostrar la configuración de seguridad del puerto para el switch, usar `show port-security`. Para ver detalles de una interfaz específica, usar `show port-security interface`.

Para verificar las direcciones MAC configuradas como sticking, use el comando `show running-config`.

Para mostrar todas las direcciones MAC seguras que son configuradas manualmente o aprendidas dinámicamente en todas las interfaces del switch, usar `show port-security address`.

## Mitigación de los Ataques de VLAN

### Revisión de Ataques de VLAN

Un salto de VLAN puede iniciar de una de tres maneras:

- La suplantación de mensajes DTP del host atacante hace que el switch entre en modo de enlace troncal. Desde aquí, el atacante puede enviar tráfico etiquetado con la VLAN de destino y el switch luego entrega los paquetes al destino.
- Introduciendo un switch dudoso y habilitando enlaces troncales el atacante pude acceder a todas las VLAN del switch víctima desde el switch dudoso.
- Otro tipo de ataque de salto a VLAN es el ataque doble etiqueta o doble encapsulado. Este ataque toma ventaja de la forma en la que opera el hardware en la mayoría de los switches.

### Pasos para Mitigar los Ataques de Salto de VLAN

- **Paso 1.** Deshabilitar las negociaciones DTP (enlace automático) en los puertos que no son enlaces mediante el comando `switchport mode access` en el modo de configuración de interfaz.
- **Paso 2.** Deshabilitar los puertos no utilizados y colocarlos en una VLAN no utilizada.
- **Paso 3.** Habilitar manualmente el enlace troncal en un puerto de enlace troncal utilizando el comando `switchport mode trunk`.
- **Paso 4.** Deshabilitar las negociaciones de DTP en los puertos de enlace mediante el comando `switchport nonegotiate`.
- **Paso 5.** Configurar la VLAN nativa en una VLAN diferente a la 1 mediante el comando **`switchport trunk native vlan** *vlan_number*`.

```bash
S1(config)#interface range fa0/1 - 16
S1(config-if-range)#switchport mode access
S1(config-if-range)#exit
S1(config)#
S1(config)#interface range fa0/17 - 20
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 1000
S1(config-if-range)#exit
S1(config)#
S1(config)#interface range fa0/21 - 24
S1(config-if-range)#switchport mode trunk
S1(config-if-range)#switchport nonegotiate
S1(config-if-range)#switchport trunk native vlan 999
S1(config-if-range)#end
S1#
```

## Mitigación de Ataques de DHCP

### Revisión de Ataque de DHCP

Los ataques de DHCP starvation pueden ser mitigados de forma efectiva usando port security, porque herramientas como Gobbler usan una MAC de origen única para cada solicitud DHCP. Mitigar ataques DHCP spoofing requieren de más protección.

Gobbler podría configurarse para usar la MAC de la interfaz real como dirección Ethernet de origen, pero especifique una diferente en el payload del DHCP, haciendo a la port security ineficaz, puesto que la MAC de origen sería legítima.

Los ataques DHCP spoofing se pueden mitigar mediante detección DHCP en puertos confiables, que filtra y limita el tráfico DHCP en puertos no confiables.

- Los dispositivos bajo control administrativo (switches, routers, servidores) son fuentes confiables.
- Las interfaces confiables (enlaces troncales, puertos del servidor) deben configurarse explícitamente como confiables.
- Los dispositivos fuera de la red y todos los puertos de acceso generalmente se tratan como fuentes no confiables.

Se crea una tabla DHCP que incluye la MAC origen de un dispositivo en un puerto no confiable y la IP asignada por el servidor DHCP a dicho dispositivo. Ambas direcciones están unidas. Por lo tanto, esta tabla se denomina tabla de enlace DHCP snooping.

### Pasos para Implementar DHCP Snooping

- **Paso 1.** Habilite DHCP snooping usando el comando `ip dhcp snooping` en el modo global.
- **Paso 2.** En los puertos de confianza, use el comando **`ip dhcp snooping limit rate** *packets-per-second*`.
- **Paso 4.** Habilite la inspección DHCP por VLAN, o por un rango de VLAN, utilizando el comando **`ip dhcp snooping** *vlan*`.

```bash
S1(config)#ip dhcp snooping
S1(config)#interface f0/1
S1(config-if)#ip dhcp snooping trust
S1(config-if)#exit
S1(config)#interface range f0/5 - 24
S1(config-if-range)#ip dhcp snooping limit rate 6
S1(config-if-range)#exit
S1(config)#ip dhcp snooping vlan 5,10,50-52
S1(config)#end
S1#
```

!image.png

Use el comando `show ip dhcp snooping` para verificar la configuración de inspección DHCP.

Use el comando `show ip dhcp snooping binding` para ver los clientes que han recibido información de DHCP.

DHCP snooping también requiere Dynamic ARP Inspection (DAI).

## Mitigación de Ataques de ARP

### Dynamic ARP Inspection

DAI requiere de DHCP snooping y ayuda a prevenir ataques ARP así:

- No retransmitiendo respuestas ARP inválidas o gratuitas a otros puertos en la misma VLAN.
- Interceptando todas las solicitudes y respuestas ARP en puertos no confiables.
- Verificando cada paquete interceptado para una IP-to-MAC Binding válida.
- Descartando y registrando respuestas ARP no válidas para evitar el envenenamiento de ARP.
- Err-disabling deshabilita la interfaz si se excede el número DAI configurado de paquetes ARP.

Para mitigar las probabilidades de ARP spoofing y ARP poisoning, siga las pautas de implementación DAI:

- Habilite DHCP snooping en el modo global.
- Habilite DHCP snooping en las VLAN seleccionadas.
- Habilite DAI en las VLAN seleccionadas.
- Configure las interfaces de confianza para DHCP snooping y DAI (no confiable es la configuración predeterminada para todas las interfaces).

Se recomienda establecer todos los puertos de acceso del switch como no confiables y todos los puertos de enlace ascendentes conectados a otros switches como confiables.

DAI requiere que funcione la tabla de enlace de DHCP snooping.

```bash
S1(config)#ip dhcp snooping
S1(config)#ip dhcp snooping vlan 10
S1(config)#ip arp inspection vlan 10
S1(config)#interface fa0/24
S1(config-if)#ip dhcp snooping trust
S1(config-if)#ip arp inspection trust
```

!image.png

DAI se puede configurar para revisar si hay direcciones MAC e IP de destino u origen:

- **MAC destino.** Comprueba y compara la MAC destino en el encabezado Ethernet con la MAC destino en el cuerpo ARP.
- **MAC de origen.** Comprueba y compara la MAC de origen en el encabezado Ethernet con la MAC del remitente en el cuerpo ARP.
- **Dirección IP.** Comprueba el cuerpo ARP para direcciones IP no válidas e inesperadas, incluidas las direcciones 0.0.0.0, 255.255.255.255 y todas las direcciones de multidifusión IP.

Para lograr este descarte de paquetes ARP, se usa el comando de configuración global siguiente:

```bash
Switch(config)#**ip arp inspection validate** {[**src-mac**] [**dst-mac**] [**ip**]}
```

Cuando se ingresan múltiples comandos de validación de inspección, sobrescriben al anterior. Para incluir más de un método de validación, ingréselos en la misma línea y verifíquelos en la siguiente salida con `do show run | include validate`.

## Mitigación de Address Spoofing Attacks

Estos ataques suplantan su dirección MAC para hacerla coincidir con la del host objetivo, haciendo que el switch, al recibir la trama, sobrescriba su entrada actual para dicha MAC asignándole el nuevo puerto y posteriormente reenviando tramas a este de forma inadvertida.

!image.png

Para protegerse contra las suplantaciones de MAC e IP, configure la función de seguridad IP Source Guard (IPSG). IPSG funciona de forma similar a DAI, pero analiza todos los paquetes, no solo los ARP.

Al igual que DAI, IPSG también requiere que DHCP snooping esté habilitado. Para cada puerto no confiable, existen dos niveles posibles de IPSG:

- **Filtro de dirección IP de origen.** El tráfico IP se filtra según su dirección IP de origen y solo permite tráfico IP con una dirección IP origen que coincida con la entrada de enlace de origen IP establecida. Cuando se crea o elimina un nuevo enlace de entrada de origen IP en el puerto, la ACL de VLAN por puerto (PVACL, Port VLAN Access Control Lists) se ajusta automáticamente para reflejar el cambio de enlace de origen IP.
- **Filtro de dirección IP y MAC de origen.** El tráfico IP se filtra según sus direcciones IP y MAC de origen. Solo permite el tráfico IP con direcciones IP y MAC de origen que coincidan con la entrada de enlace de origen IP.

La función IP Source Guard se habilita en puertos no confiables mediante el comando `ip verify source`.

```bash
S1(config)#interface range fastethernet 0/1 - 2
S1(config-if-range)#ip verify source
```

!image.png

Use el comando del modo EXEC privilegiado `show ip verify source` para verificar la configuración IPSG.

## Mitigar Ataques STP

### PortFast y Protección de BPDU

Los atacantes pueden manipular el Spanning  Tree Protocol (STP) para realizar un ataque falsificando el puente raíz y cambiando la topología de una red. Para mitigarlos, use PortFast y la protección de Bridge Protocol Data Units (BPDU):

#### PortFast

PortFast lleva inmediatamente un puerto al estado de reenvío desde un estado de bloqueo, sin pasar por los estados de escucha y aprendizaje. Aplica a todos los puertos de acceso de usuario final.

#### Protección de BPDU

El error de protección de BPDU deshabilita inmediatamente un puerto que recibe una BPDU. Al igual que PortFast, la protección BPDU solo debe configurarse en interfaces conectadas a dispositivos finales.

### Configurar PortFast

PortFast omite los estados de escucha y aprendizaje de STP para minimizar el tiempo que los puertos deben esperar a que STP converja. Solo se debe habilitar en los puertos de acceso. Si se habilita en enlaces entre conmutadores, puede crear un bucle de árbol de expansión.

- **En una interfaz.** Utilice el comando `spanning-tree portfast`.
- **Globalmente.** Utilice el comando `spanning-tree portfast default` para habilitar PortFast en todos los puertos de acceso.

Para verificar si PortFast está habilitado globalmente, puede usar:

- Comando `show running-config | begin span`
- Comando `show spanning-tree summary`

Para verificar si PortFast tiene habilitada una interfaz, use el comando **`show running-config interface** *type/number*`.

Otro comando para verificación es **`show spanning-tree interface** *type/number* **detail**`.

### Configurar BPDU Guard

Un puerto de acceso podría recibir un BPDU inesperado accidentalmente o porque un usuario conectó un switch no autorizado al puerto de acceso. Si se recibe una BPDU en un puerto de acceso habilitado para BPDU Guard, el puerto se pone en estado `err-disable`. Esto significa que debe apagarse y habilitarse manualmente o recuperarse automáticamente mediante el comando `errdisable recovery cause psecure-violaton` en el modo de configuración global.

Para BPDU Guard:

- **En una interfaz.** Use el comando `spanning-tree bpduguard enable`.
- **Globalmente.** Use el comando de configuración global `spanning-tree portfast bpduguard default` para habilitar BPDU Guard en todos los puertos de acceso.

# Seguridad de Dispositivos Intermediarios

## Proteger el Enrutador Perimetral

### Proteja la Infraestructura de Red

Garantizar la seguridad de la infraestructura (routers, switches, servidores, terminales y otros) es fundamental para la seguridad general de la red. Los routers son el objetivo principal de muchos ataques, ya que dirigen el tráfico desde y hacia la red.

El router de borde (edge router) es el último router entre la red interna y la red no confiable, como internet). Todo el tráfico de internet de una organización pasa por un router de borde.

### Enfoques de Seguridad Para Routers de Borde

- **Enrutador único.** Un único router conecta la red LAN interna con internet. Todas las políticas de seguridad se configuran en este dispositivo.

!image.png

- **Defensa en profundidad.** Múltiples capas de seguridad antes de que el tráfico ingrese a la LAN protegida. Existen tres capas principales de defensa: router perimetral, firewall y router interno conectado a la LAN protegida.

!image.png

- **DMZ.** Se puede utilizar para servidores que deben ser accesibles desde internet u otra red externa. Se puede configurar entre dos routers: uno interno, conectado a la red protegida, y otro externo, conectado a la no protegida.

!image.png

### Tres Áreas de Seguridad del Router

- **Físico.** Coloque el enrutador y los dispositivos físicos conectados a él en una habitación segura con cerradura, a la que solo tenga acceso personal autorizado. Instale un sistema de alimentación ininterrumpida (UPS, Uninterruptible Power Supply) o generador diésel de respaldo.
- **Sistema operativo.** Configure el router con la máxima cantidad de memoria posible. La disponibilidad de memoria puede ayudar a mitigar los ataques DoS. Utilice la versión más estable y reciente del SO que cumpla con las especificaciones del router o dispositivo de red. Mantenga una copia segura de las imágenes del sistema operativo del router y los archivos de configuración como copias de seguridad.
- **Refuerzo de la seguridad del router.** Asegúrese de que solo el personal autorizado tenga acceso y su nivel de acceso esté controlado. Deshabilite los puertos e interfaces no utilizados, así como los servicios innecesarios (un router cuenta con servicios habilitados por defecto).

### Acceso Administrativo Seguro

El acceso administrativo no autorizado podría causar una modificación en los parámetros de enrutamiento, que se deshabiliten las funciones de enrutamiento o dejar expuestos y accesibles otros sistemas dentro de la red.

Se debe restringir el acceso a dispositivos, iniciar sesión y crear cuenta para todo el acceso, autenticar el acceso, autorizar acciones, presentar notificaciones legales y garantizar la confidencialidad de los datos.

### Acceso Seguro Local y Remoto

Se puede acceder a un router con fines administrativos de forma local o remota:

- **Acceso local.** El administrador debe tener acceso físico al enrutador y usar un cable de consola para conectarse al puerto de consola. Se usa normalmente para la configuración inicial del dispositivo.
- **Acceso remoto.** Consiste en permitir conexiones Telnet, SSH, HTTP, HTTPS o SNMP al router desde una computadora. La computadora puede estar en una red local o remota.

## Configurar el Acceso Administrativo Seguro

### Configurar Contraseñas

Para proteger el modo EXEC de usuario, ingresa al modo de configuración de la línea de consola mediante el comando de configuración global `line console 0`. Especifique la contraseña del modo EXEC del usuario mediante el comando **`password** *password*`. Habilite el acceso al EXEC del usuario mediante el comando `login`.

```bash
S1#configure terminal
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
S1#
```

Para proteger el acceso al modo EXEC privilegiado, usar el comando de configuración global **`enable secret** *password*`.

```bash
S1#configure terminal
S1(config)#enable secret class
S1(config)#exit
S1#
```

Para proteger las líneas VTY (Virtual Teletype), que permiten el acceso remoto a dispositivos Cisco mediante SSH o Telnet para administración, usar el comando de configuración global **`line vty** *0 15*`. Especifique la contraseña de VTY usando el comando **`password** *password*`. Habilite el acceso VTY mediante el comando `login`.

```bash
S1#configure terminal
S1(config)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
S1#
```

### Cifrar Contraseñas

Para garantizar que las contraseñas permanezcan seguras en un router y switch Cisco, podemos hacer cifrado de todas las contraseñas en texto plano, establecer una longitud mínima aceptable para la contraseña, deshabilitar el acceso al modo EXEC privilegiado inactivo después de un período de tiempo determinado.

Para configurar todas las contraseñas en texto plano, utilice el comando de configuración global `service password-encryption`.

```bash
S1#configure terminal
S1(config)#service password-encryption
S1(config)#
```

Use el comando `show running-config` del modo EXEC privilegiado para verificar que las contraseñas estén cifradas.

### Seguridad Adicional de la Contraseña

El comando `service password-encryption` impide que personas no autorizadas vean las contraseñas en texto plano en el archivo de configuración. Para garantizar que las contraseñas tengan una longitud mínima especificada, utilice el comando del modo de configuración global **`security passwords min-length** *length*`.

Se puede usar software de descifrado de contraseñas para realizar un ataque de fuerza bruta en un dispositivo de red, para prevenirlo, usar el comando del modo de configuración global **`login block-for** *seconds* **attempts** *tries* **within** *seconds*`.

Para controlar el tiempo de inactividad de un usuario, se puede usar el comando del modo configuración de líneas (console, VTY, AUX) **`exec-timeout** *minutes* ****[*seconds*]`. Si se establece a ambos valores como 0, el tiempo será indefinido.

El comando del modo de configuración de líneas virtuales VTY **`transport input** *mode*` funciona como un filtro de seguridad para los protocolos para conexión remota al equipo. Para el modo:

- `ssh`. Permite solo conexiones SSH (práctica recomendada).
- `telnet`. Permite solo conexiones Telnet (inseguro).
- `all`. Permite tanto SSH como Telnet.
- `none`. Deshabilita el acceso remoto entrante.

```bash
R1(config)#service password-encryption
R1(config)#security passwords min-length 8
R1(config)#login block-for 120 attempts 3 within 60
R1(config)#line vty 0 4
R1(config-line)#password cisco123
R1(config-line)#exec-timeout 5 30
R1(config-line)#transport input ssh
R1(config-line)#login
```

Para verificar que todas las configuraciones son correctas, usar el comando del modo EXEC privilegiado `show running-config | section line vty`.

### Algoritmos de Contraseñas Secretas

Los hashes MD5 ya no se consideran seguros porque los atacantes pueden reconstruir certificados válidos. Esto les permite suplantar la identidad de cualquier sitio web. El comando `enable secret` usa un hash MD5 por defecto. Ahora se recomienda configurar todas las contraseñas secretas con algoritmos de tipo 8 y 9. Los tipo 8  y 9 se introdujeron en Cisco IOS 15.3(3)M y utilizan cifrado SHA.

Para introducir explícitamente el algoritmo a utilizar para encriptar la contraseña, poner el comando

```bash
R1(config)#**enable** **algorithm-type** { **md5** | **scrypt** | **sha256** | **secret** } *password*
```

Para aplicar esto en la protección de contraseñas de cuentas de usuario individuales creadas en el equipo, usamos el comando

```bash
R1(config)#**username** *name* **algorithm-type** { **md5** | **scrypt** | **sha256** | **secret** } *password*
```

- `md5` **Message-Digest Algorithm 5.** Estándar antiguo todavía disponible para compatibilidad con dispositivos antiguos.
- `scrypt` **Tipo 9.** Es el más fuerte, diseñado para requerir mucha memoria y tiempo de procesamiento para calcularse.
- `sha256` **Tipo 8.** Utiliza un estándar criptográfico moderno (SHA-256 junto con la técnica PBKDF2).

## Configurar la Seguridad Mejorada para Inicios de Sesión Virtuales

### Mejorar el Proceso de Inicio de Sesión

El bloqueo de inicio de sesión consiste en habilitar un perfil de detección que permita configurar un dispositivo de red para que reacciones ante repetidos intentos de fallos de inicio de sesión, rechazando así las solicitudes de conexión posteriores.

Las listas de control de acceso (ACL) se pueden utilizar para permitir conexiones legítimas desde direcciones de administradores de sistemas conocidos. Utilice el comando de modo de configuración global `banner` para especificar los mensajes adecuados. Estos protegen la organización desde una perspectiva legal.

```bash
R1(config)#**banner** { **motd** | **exec** | **login** } *delimiter message delimiter*
```

- `motd` **(Message of the Day).** Es el primer mensaje que ve cualquier persona que establezca una conexión con el router o switch, incluso antes de que se le pida un usuario o contraseña.
- `login`. Se muestra inmediatamente después del MOTD y justo antes de que aparezca el prompt pidiendo el Username o Password.
- `exec`. Solo se muestra cuando después de que el usuario ha iniciado sesión con éxito y ha entrado al modo EXEC de usuario (cuando aparece el prompt R1>). Generalmente se usa para dejar notas operativas útiles.

### Configurar las Funciones de Mejora del Inicio de Sesión

El comando `login block-for` protege contra ataques DoS deshabilitando los inicios de sesión tras un número determinado de intentos fallidos.

```bash
R1(config)#**login block-for** *seconds* **attempts** *tries* **within** *seconds*
```

El comando `login quiet-mode` asigna una ACL que identifica a los hosts permitidos, ignorando el bloqueo por intento de ataque solo si la conexión proviene de las IP en la ACL.

```bash
R1(config)#**login quiet-mode access-class** { *acl-name* | *acl-number* }
```

El comando `login delay` especifica el número de segundos que el usuario debe esperar entre intentos de inicio de sesión fallidos.

```bash
R1(config)#**login delay** *seconds*
```

Los comandos `login on-success` y `login on-failure` registran los intentos de inicio de sesión exitosos y fallidos.

```bash
R1(config)#**login on-success log** [**every** *login*]
R1(config)#**login on-failure log** [**every** *login*]
```

### Habilitar Mejoras de Inicio de Sesión

Para ayudar a un dispositivo Cisco IOS a proporcionar detección de DoS, utilice el comando `login block-for`, que debe emitirse antes que cualquier comando login. Este comando supervisa la actividad del dispositivo login y opera en dos modos:

- **Modo normal.** También llamado modo de vigilancia, donde el router lleva la cuenta del número de intentos de inicio de sesión fallidos dentro de un período de tiempo determinado.
- **Modo silencioso.** También llamado período de silencio. Si el número de inicios de sesión supera el umbral configurado, se deniegan todos los intentos de inicio de sesión mediante Telnet, SSH y HTTP durante el tiempo especificado en el comando `login block-for`.

```bash
R1(config)#ip access-list standard PERMIT-ADMIN
R1(config-std-nacl)#remark Permit only Administrative hosts
R1(config-std-nacl)#permit 192.168.10.10
R1(config-std-nacl)#permit 192.168.11.10
R1(config-std-nacl)#exit
R1(config)#login quiet-mode access-class PERMIT-ADMIN
```

### Registrar Intentos Fallidos

Estos comandos permiten detectar ataques de contraseñas registrando los intentos de inicio de sesión fallidos o exitosos. Los comandos `login on-success log` y `login on-failure log` generan mensajes de syslog para intentos de inicio de sesión fallidos y exitosos.

Como alternativa al comando `login on-failure log`, se puede configurar el comando `security authentication failure rate` para generar un mensaje de registro cuando se supera la tasa de fallos de inicio de sesión.

```bash
R1(config)#**login on-success log** [**every** *login*]
R1(config)#**login on-failure log** [**every** *login*]
```

```bash
R1(config)#**security authentication failure rate** *threshold-rate* **log**
```

Utilice el comando `show login` para verificar la configuración de `login block-for` para la sesión actual. El comando `show login failures` muestra información adicional sobre los intentos fallidos, como la dirección IP desde la que se originaron dichos intentos.

## Configurar SSH

### Habilitar SSH

- **Paso 1.** Configure un nombre de host único para el dispositivo con el comando del modo configuración global **`hostname** *name*`.
- **Paso 2.** Configure el nombre de dominio IP, el cual es necesario para generar las claves de cifrado y permite que el dispositivo resuelva hostnames que no están completamente calificados. Se usa el comando **`ip domain name** *domain*`.
- **Paso 3.** Generar una clave para cifrar el tráfico SSH. Este comando genera un par de llaves (pública y privada). el parámetro de modulus indica qué tan fuerte es el cifrado. Se recomienda 1024 o superior. SSH versión 2 requiere de al menos 768 bits. El comando de configuración global para esto es **`crypto key generate rsa general-keys modulus** *integer*`.
- **Paso 4.** Verifique o cree una entrada en la base de datos local. Dado que SSH no suele crear una contraseña “general” como Telnet, se requieren usuarios específicos. Se pueden crear con el algoritmo secret para encriptación, así como otros. Se puede usar el comando de configuración global **`username** *name* **secret** *password`* o **`username** *name* **algorithm-type** { **md5** | **scrypt** | **sha256** | **secret** } *password`.*
- **Paso 5.** Autentíquese con la base de datos local. Esto se logra usando el comando `login local` dentro de las líneas de configuración de VTY. Este comando le dice al router que busque en la lista de usuarios del dispositivo un usuario en el cual autenticarse y no pida una contraseña genérica.
- **Paso 6.** Habilitar las sesiones SSH entrantes de VTY. Obligamos al router solo aceptar como entrada de conexión remota las SSH con el comando del modo de configuración de líneas VTY `transport input ssh`.

```bash
Router#configure terminal
Router(config)#hostname R1
R1(config)#ip domain name span.com
R1(config)#crypto key generate rsa general-keys modulus 1024
R1(config)#username Bob secret cisco
R1(config)#line vty 0 4
R1(config-line)#login local
R1(config-line)#transport input ssh
R1(config-line)#exit
R1(config)#
```

### Mejore la Seguridad de Inicio de Sesión SSH

Para verificar la configuración opcional del comando SSH, utilice el comando `show ip ssh`.

Utilice el comando **`ip ssh time-out** *seconds`* en el modo de configuración global para modificar (reducir) el intervalo de tiempo de espera predeterminado de 120 segundos. Esta configuración aplica a la fase de negociación. Este comando tiene otra una opción para los reintentos de autenticación, que por defecto son tres y no pueden ser superiores a cinco.

```bash
R1(config)#**ip ssh** {**timeout** *seconds* | **authentication-retries** *integer*}
```

### Conectar un Router a un Router Habilitado para SSH

Para verificar el estado de las conexiones de los clientes, utilice el comando `show ssh`.

Existen dos formas de conectarse a un router con SSH habilitado. Por defecto, cuando SSH está habilitado, un router Cisco puede funcionar como servidor o cliente SSH. Como servidor, un router puede aceptar conexiones de clientes SSH. Como cliente, un router puede conectarse a mediante SSH a otro router con SSH habilitado.

Para conectar desde un router R2 hacia el router R1, usamos el comando **`ssh -l** *username ip-address*`. En este comando, `ssh` llama al protocolo Secure Shell, el parámetro `-l` es de login que sirve para indicarle al equipo remoto con qué usuario se desea entrar (`*username*`). También se debe especificar la dirección IP de la interfaz del router con la cual se busca establecer la conexión.

### Conectar un Host a un Router Habilitado para SSH

Conéctese utilizando un cliente SSH (PuTTY, OpenSSH, TeraTerm) que se ejecute en un host. El cliente SSH inicia la conexión con el router. El servicio SSH validará las claves y luego se puede usar la sesión como si fuera un Telnet estándar.

## Restauración de Contraseñas en Routers

### Guardar Copias de Seguridad de Configuración

Para guardar la configuración actual en la NVRAM, usamos el comando `copy running-config startup-config`. La sintaxis del comando del modo EXEC privilegiado copy es como se muestra:

```bash
Router#**copy** *source-file destination*
```

Si tenemos un servidor TFTP (servidor de transferencia de archivos basado en UDP), podemos enviar la configuración haciendo `copy source-file tftp`. Se solicitará la dirección del servidor TFTP y el nombre para el archivo.

### Restauración de Contraseñas sin Perder Configuración

Los routers tienen números de registros importantes:

- **0x2102.** Registro predeterminado que carga el sistema operativo Cisco IOS desde la memoria flash y carga la configuración de inicio desde la NVRAM.
- **0x2142.** Registro que ignora la configuración inicial durante el boot, haciendo bypassing de contraseñas, permitiendo ingresar a la CLI para resetearlas.

Para cambiar el registro de configuración (para inicio), se usa el comando de configuración global **`config-register** *registro*`. Si hacemos `config-register 0x2142`, al reiniciar el dispositivo con el comando de modo EXEC privilegiado `reload` o de forma manual, la running-config estará vacía, pero la startup-config mantendrá la configuración inicial previa, además de que el registro de configuración seguirá siendo 0x2142.

El problema con este método es que requiere de que se cambie el registro desde el modo de configuración global. 

Una forma de hacerlo sin la necesidad de haber iniciado sesión antes es, primero, reiniciando el dispositivo y presionando `Ctrl + C` durante el arranque para cortarlo e ir al modo ROMMON (ROM Monitor). El modo ROMMON es un entorno de arranque de emergencia para equipos Cisco (routers y switches) usado para recuperar contraseñas, cargar imágenes de IOS dañadas o diagnosticar fallos. Desde este modo haremos el cambio del registro.

Lo siguiente es configurar el registro con el comando `confreg 0x2142` y luego reiniciarlo con `reset`.

```bash
rommon 1 > confreg 0x2142
rommon 2 > reset
```

Por último, sea cual sea el método que usemos, se debe traer la configuración del startup-config al running-config con el comando `copy startup-config running-config`. Al finalizar con los cambios, volver a establecer el registro de configuración al 0x2102.

Si el archivo del sistema operativo se daña o se borra, al reiniciar el router, accederemos de forma instantánea al modo ROMMON. Podemos recuperar el archivo desde un servidor TFTP externo. Para esto, usaremos el comando `tftpdnld`.

Estando en el prompt `rommon >`, tendremos que ajustar las IP de nuestra red, diciéndole al router quién es él y dónde está el servidor.

```bash
rommon 1 > **IP_ADDRESS=***ip-address*
rommon 2 > **IP_SUBNET_MASK=***subnet-mask*
rommon 3 > **DEFAULT_GATEWAY=***default-gateway*
rommon 4 > **TFTP_SERVER=***ip-address-tftp-server*
rommon 5 > **TFTP_FILE=***file-cisco-ios*
```

Para verificar que las variables están correctamente establecidas, usamos el comando `set`. Considerar que las variables de entorno del sistema son sensibles a espacios y mayúsculas. Si se tiene una conexión directa con el servidor TFTP, no se necesitará configuración adicional. Haremos entonces:

```bash
rommon 6 > tftpdnld
```

Y confirmaremos que queremos continuar ingresando `y`. Después podemos reiniciar el dispositivo con el comando `reset` o podemos iniciarlo con `boot` y el sistema operativo cargará normalmente a RAM.

# Asignación de Roles Administrativos

## Configurar Niveles de Privilegio

### Limitar la Disponibilidad de Comandos

El software Cisco IOS puede proporcionar acceso a la infraestructura mediante la interfaz de línea de comandos (CLI) basada en niveles de privilegio o roles.

Por defecto, la CLI de Cisco IOS tiene dos niveles de acceso a los comandos: Modo EXEC de usuario (nivel de privilegio 1) y modo EXEC privilegiado (nivel de privilegio 15). En total existen 16 niveles de privilegios.

El nivel 0 de privilegios es un nivel muy restringido que solo permite pocos comandos (como `enable`, `exit`, `help`). Los niveles 2 al 14 están vacíos por defecto y se utilizan para crear perfiles personalizados para diferentes roles. Use el comando del modo de configuración global siguiente para configurar un nivel de privilegios.

```bash
R1(configure)#**privilege** *mode* { **level** *level* | **reset** } *command* [ *domain* ]
```

- `*mode*`. Especifica el modo de la CLI donde reside el comando que se desea mover (`exec`, `configure`, `interface`, `line`, `router`).
- **`level`**. Permite establecer un nivel de privilegio con un comando específico.
- `*level*`. El nivel de privilegio asociado a un comando. Puede especificar hasta 16 niveles, utilizando los números del 0 al 15.
- **`reset`**. Se usa para devolver un comando a su nivel de privilegio original por defecto.
- `*domain*`. Argumento opcional que se utiliza cuando se desee restablecer el nivel de privilegio.
- `*command*`. Comando específico que se desea permitir (o retirar) del nivel de privilegio.

### Configuración y Asignación de Niveles de Privilegio

Para asignar un nivel de privilegio a un usuario específico, utilice el comando de configuración global **`username** *name* **privilege** *level* **secret** *password*`.

```bash
Router(config)#**username** *name* **privilege** *level* **secret** *password*
```

Para asignar un nivel de privilegio a un modo EXEC específico, utilice el comando de configuración global **`enable secret level** *level password*`.

```bash
Router(config)#**enable secret level** *level password*
```

### Limitaciones de los Niveles de Privilegio

- En un router no existe control de acceso a interfaces, puertos, interfaces lógicas ni ranuras específicas.
- Los comandos disponibles en niveles de privilegio inferiores siempre se pueden ejecutar en niveles superiores.
- Los comandos configurados específicamente para un nivel superior no están disponibles para usuarios con privilegios inferiores.
- Asignar un comando con múltiples palabras clave permite el acceso a todos los comandos que utilizan esas palabras clave. Por ejemplo, permitir el acceso a `show ip route` permite al usuario acceder a todos los comandos `show` y `show ip`.

## Configurar la CLI Basada en Roles

### Acceso a la Interfaz de Línea de Comandos Basado en Roles

La función de Cisco IOS release 12.3(11)T de acceso a la interfaz de línea de comandos (CLI) basado en roles proporciona un acceso más preciso y granular al controlar qué comandos están disponibles para roles específicos. Esta función permite crear diferentes vistas de las configuraciones del router para diferentes usuarios. Cada vista define los comandos de la CLI a los que cada usuario puede acceder. Abordan la seguridad, disponibilidad y eficiencia operativa.

### Vistas Basadas en Roles

La CLI basada en roles proporciona tres tipos de vistas que determinan qué comandos están disponibles:

- **Vista raíz.** Para configurar cualquier vista del sistema, el administrador debe estar en la vista raíz.
- **Vista de CLI.** Un conjunto específico de comandos se pueden agrupar en una vista de CLI.
- **Supervista.** Consta de una o más vistas de la CLI.

Una única vista de CLI puede compartirse entre varias supervistas. Los comandos no se pueden configurar para una supervista sino que se deben agregar a la vista de CLI y agregar esta a la supervista.

Los usuarios que haya iniciado sesión en una supervista pueden acceder a todos los comandos configurados para cualquiera de las vistas de CLI que forman parte de esta. Eliminar una supervista no elimina las vistas CLI asociadas. Las vistas CLI permanecen disponibles para ser asignadas a otra supervista.

Cada supervista tiene una contraseña que se utiliza para cambiar entre supervistas o desde una vista CLI a una supervista.

### Configurar Vistas Basadas en Roles

- **Paso 1.** Habilite AAA (Autenticación, Autorización y Contabilidad) con el comando de configuración global `aaa new-model`. Es recomendable configurar un usuario local para evitar perder el acceso. Salga y vuelva a entrar en la vista raíz con el comando `enable view`. La contraseña será la misma que se usa para acceder al modo EXEC privilegiado en caso no se añada el parámetro de nombre de la vista.

```bash
Router(config)#**aaa new-model**
Router(config)#**exit**
Router#**enable** [**view** [*view-name*]]
```

- **Paso 2.** Cree una vista utilizando el comando de configuración global **`parser view** *view-name*`. Esto habilita el modo de configuración de vista.

```bash
Router(config)#**parser view** *view-name*
```

- **Paso 3.** Asigne una contraseña a la vista utilizando el comando del como de configuración de vista.

```bash
Router(config-view)#**secret** *password*
```

- **Paso 4.** Asigne comandos a la vista seleccionada utilizando el comando del modo de configuración de vista `commands`.

```bash
Router(config-view)#**commands** *parser-mode* {**include** | **include-exclusive** | **exclude**} [**all**] [**interface** *interface-name* | *command*]
```

- **Paso 5.** Salga del modo de configuración de vista escribiendo el comando `exit`.

#### Desglose del Comando `commands`

1. **`commands`**. Agrega comandos o interfaces a una vista.
2. `*parser-mode*`. Define el modo de la CLI donde vive el comando que se desea agregar (`exec`, `configure`, `interface`).
3. **`include`**. Agrega un comando a la interfaz o vista y permite que el mismo comando o interfaz se agregue a otras vistas.
4. **`include-exclusive`**. Agrega un comando o una interfaz a la vista y excluye que ese mismo comando o interfaz se agregue a todas las demás vistas.
5. **`exclude`**. Excluye un comando o una interfaz de la vista.
6. **`all`**. Comodín que permite que cada comando en un modo de configuración específico que comience con la misma palabra clave o cada subinterfaz para una interfaz específica forme parte de la vista.
7. **`interface** *interface-name*`. Interface que se agrega  a la vista. Limita la vista a una interfaz física o lógica.
8. `*command*`. Comando que se agrega a la vista.

### Configurar Vistas Generales de la CLI Basadas en Roles

Los pasos para configurar una supervista son esencialmente los mismos que para configurar una vista de CLI, con la excepción de que el comando **`view** *view-name*` se utiliza para asignar comandos a la supervista.

- **Paso 1.** Cree una supervista utilizando el comando **`parser view** *view-name* **superview**`. Este comando envía directamente al modo de configuración de vista.
- **Paso 2.** Asigne una contraseña con **`secret** *password*`.
- **Paso 3.** Asigne una vista de CLI existente a la supervista utilizando el comando **`view** *view-name*` del modo de configuración de vista.
- **Paso 4.** Salga del modo de configuración de supervista escribiendo el comando `exit`.

### Verificar las Listas de la CLI Basadas en Roles

Para verificar una vista utilice el comando enable **`view** *view-name*`. Introduzca el nombre de la vista que desea verificar y la contraseña para acceder a ella. Utilice la ayuda contextual (`?`) para comprobar que los comandos disponibles en la vista son correctos.

Al no especificar una vista para el comando `enable view`, puede inicar sesión como root. Desde la vista root, use el comando `show parser view all` para ver un resumen de todas las vistas.

# Control de Acceso

## Controles de Acceso

### Controles de Acceso Físico

Los controles de acceso físico son barreras reales desplegadas para evitar el contacto físico directo con los sistemas. El objetivo es evitar que usuarios no autorizados tengan acceso físico a las instalaciones, equipo y otros activos de la organización.

### Controles de Acceso Administrativo

Los controles de acceso administrativo son las políticas y procedimientos que definen las organizaciones para implementar y hacer cumplir todos los aspectos del control de acceso no autorizado.

Los controles administrativos se enfocan en las prácticas de personal y las prácticas empresariales. Las políticas son declaraciones de intenciones. Los procedimientos son pasos detallados necesarios para realizar una actividad.

El concepto de control administrativo implica tres servicios de seguridad: autenticación, autorización y contabilidad (AAA). Estos servicios proporcionan el marco principal para controlar el acceso, lo que evita el acceso no autorizado en una computadora, red, base de datos u otro recurso de datos.

#### Autenticación

Verifica la identidad de cada usuario para evitar el acceso no autorizado. Los usuarios deben probar que son quienes dicen ser proporcionando información sobre algo que saben, algo que tienen o algo que son.

En el caso de autenticación de dos factores, el sistema requiere de una combinación de dos factores diferentes.

#### Autorización

Determinan a qué recursos pueden acceder los usuarios, así como las operaciones que pueden realizar. Algunos sistemas logran esto con listas de control de acceso (ACL).

Después de que un usuario demuestre su identidad, el sistema verifica a qué recursos de la red puede acceder y qué puede hacer con esos recursos.

#### Contabilidad

Rastrea las actividades de los usuarios, incluidos los sitios a los que accede, cantidad de tiempo que tienen acceso a los recursos y los cambios realizados.

Los servicios de contabilidad de ciberseguridad rastrean cada transacción de datos y brindan resultados de auditoría. Los administradores del sistema pueden configurar políticas informáticas para habilitar la auditoría del sistema. La auditoría de ciberseguridad rastrea y monitorea en tiempo real.

#### ¿Qué es la Identificación?

Aplica las reglas establecidas por la política de autorización. Cada vez que se solicita un recurso, los controles de acceso determinan si se concede o se deniega. Un identificador único garantiza la asociación correcta entre las actividades permitidas y los sujetos. Un nombre de usuario es el método más común utilizado para identificarlo.

### Gestión de Identidad Federada

Hace referencia a varias empresas que permiten a sus usuarios utilizar las mismas credenciales de identificación para obtener acceso a las redes de todas las empresas del grupo.

Una identidad federada vincula la identidad electrónica del sujeto a través de distintos sistemas de gestión de identidades. El objetivo de la administración de identidades federada es compartir la información de identidad automáticamente a través de los límites del dominio.

## Conceptos del Control de Acceso

### Seguridad de Confianza Cero (Zero Trust)

Zero Trust es un enfoque integral para garantizar el acceso a todas las redes, aplicaciones y entornos. Este enfoque ayuda a proteger el acceso de usuarios, dispositivos de usuario final, API, IoT, microservicios, dockers, y más.

Este framework ayuda a evitar el acceso no autorizado, contener brechas y reducir el riesgo de movimiento lateral de un atacante a través de la red. Tradicionalmente, el perímetro de la red era la frontera entre el interior y exterior. En el enfoque zero trust, cualquier lugar en el que se requiera de una decisión de control de acceso debe considerarse un perímetro.

Los tres pilares de zero trust son la fuerza de trabajo, cargas de trabajo y lugar de trabajo.

#### Zero Trust para la Fuerza de Trabajo

Este pilar está formado por personas que acceden a las aplicaciones de trabajo mediante el uso de sus dispositivos personales o gestionados por la empresa. Garantiza que solo los usuarios adecuados y los dispositivos seguros puedan acceder a aplicaciones, independientemente de la ubicación.

#### Zero Trust para Cargas de Trabajo

Este pilar se refiere a las aplicaciones que se ejecutan en la nube, en centros de datos y otros entornos virtualizados que interactúen entre sí. Se centra en el acceso seguro cuando una API, microservicio o docker acceden a una base de datos dentro de una aplicación.

#### Zero Trust para el Lugar de Trabajo

Este pilar se refiere al acceso seguro para todos y cada uno de los dispositivos, incluyendo el internet de las cosas (IoT), redes empresariales a las que se conectan, puntos finales de usuario, servidores físicos y virtuales, impresoras, cámaras, sistemas HVAC, quioscos, bombas de infusión, sistemas de control industrial y más.

### Modelos de Control de Acceso

Una organización debe implementar controles de acceso adecuado para proteger sus recursos de red, de sistema, de información y la información misma. Un modelo de control de acceso es el principio de privilegio mínimo, que especifica un enfoque limitado y con base en las necesidades para otorgar derechos de acceso de usuarios y procesos a información y herramientas específicas.

Un ataque de escalamiento de privilegios, que es un ataque común, consiste en aprovechar las vulnerabilidades en los servidores o sistemas de control de acceso para otorgar a un usuario o proceso de software no autorizado niveles de privilegio más altos de los que debería tener.

#### Control de Acceso Discrecional (DAC)

Este es el modelo menos restrictivo y permite a los usuarios controlar el acceso de sus datos como si fueran propietarios de estos. El DAC permite usar ACL u otros métodos para especificar qué usuarios o grupos de usuarios tienen acceso a la información.

#### Control de Acceso Obligatorio (MAC)

Se aplica el más estricto control de acceso y suele utilizarse en aplicaciones militares o fundamentales para la misión. Asigna etiquetas del nivel de seguridad a la información y habilita el acceso de los usuarios en función del nivel de autorización de seguridad.

#### Controles de Acceso Basados en Roles (RBAC)

Las decisiones de acceso se basan en los roles y responsabilidades del individuo dentro de la organización. Se asignan privilegios de seguridad a diferentes roles y se asignan personas al perfil RBAC para el rol. Las funciones pueden incluir diferentes puestos, clasificaciones de puestos o grupos de clasificaciones de puestos. También se conoce como un tipo de control de acceso no discrecional.

#### Control de Acceso Basado en Atributos (ABAC)

Permite el acceso según los atributos del objeto (recurso) al que se tendrá acceso, el sujeto (usuario) que tendrá acceso al recurso y los factores del entorno respecto de cómo se tendrá acceso al objeto (por ejemplo, la hora).

#### Control de Acceso Reglamentado (RBAC, Ruled-Based Access Control)

El personal de red especifica conjuntos de reglas o condiciones asociadas con el acceso a datos o sistemas. Estas reglas puede especificar direcciones IP, protocolos u otras condiciones.

#### Control de Acceso Basado en Tiempo (TAC)

TAC permite el acceso a los recursos de red en función de la hora y el día.

### Sistemas de Control de Acceso a la Red (NAC)

Admiten la administración de acceso al hacer cumplir las políticas de la organización con respecto a las personas y los dispositivos que intentan acceder a la red. Permiten monitorear a los usuarios y dispositivos que están conectados a la red y controlar manualmente el acceso según sea necesario.

## Administración de Cuentas

### Tipos de Cuentas

Una organización no debe compartir cuentas para usuarios privilegiados, administradores o aplicaciones. La cuenta de administración solo debe utilizarse para administrar un sistema. Los administradores deben conocer las cuentas de grupo y de usuario por defecto que puede instalar un sistema operativo.

### Cuentas de Privilegio

Los administradores usan estas cuentas para implementar y administrar los sistemas operativos, aplicaciones y dispositivos de red. Asegurar y bloquear continuamente las cuentas privilegiadas es fundamental para la seguridad de la organización. Evalúe periódicamente este proceso y realice ajustes para mejorar la protección.

### Control de Acceso a los Archivos

Los permisos son reglas configuradas para limitar el acceso a un individuo o grupo y pueden ayudar a proteger los datos. Los usuarios deben estar limitados solo a los recursos que necesitan en un sistema informático o red (principio de mínimos privilegios).

- **Control Total.** Los usuarios pueden ver el contenido de un archivo o carpeta, modificar y eliminarlas, crear nuevos archivos y carpetas y ejecutar programas de una carpeta.
- **Modificar.** Los usuarios pueden modificar y eliminar archivos y carpetas existentes, pero no pueden crear nuevos.
- **Leer y ejecutar.** Los usuarios pueden ver el contenido de los archivos y carpetas existentes y pueden ejecutar programas en una carpeta.
- **Escritura.** Los usuarios pueden crear nuevos archivos y carpetas y realizar cambios en los archivos y carpetas existentes.
- **Lectura.** Los usuarios pueden ver el contenido de una carpeta y abrir archivos y carpetas.

Si un administrador deniega a un individuo o grupo los permisos para un recurso compartido de red, esto anulará cualquier otra configuración de permisos. El usuario no podrá acceder a ese recurso compartido, incluso si es el administrador o forma parte del grupo de administradores.

La política de seguridad local debe describir los recursos a los que puede acceder cada usuario y grupo, y el tipo de acceso para cada uno. Una vez que se establecen los permisos de la carpeta principal, las carpetas y los archivos que es crean dentro de esta heredan sus permisos.

La ubicación de los datos y la acción realizada con estos determina la propagación de los permisos.

## Uso y Funcionamiento de AAA

### Autenticación AAA

- **Autenticación AAA local.** A veces se la conoce como autenticación independiente porque autentica a los usuarios con nombres de usuario y contraseñas almacenados localmente. Ideal para redes pequeñas.
- **Autenticación AAA basada en servidor.** Este método autentica con un servidor AAA central que contiene los nombres de usuario y las contraseñas de todos los usuarios. Es apropiado para redes medianas y grandes.

Los dispositivos se comunican con el servidor AAA centralizado utilizando los protocolos RADIUS o TACACS+. La autenticación AAA centralizada tiene más capacidad de escalamiento y administración que la local. Puede mantener de forma independiente bases de datos para autenticación, autorización y contabilidad. Además de que puede aprovechar el Directorio Activo o LDAP para autenticación de usuarios y la pertenencia a grupos, mientras mantiene sus propias bases de datos.

#### TACACS+

Separa las funciones de autenticación, autorización y contabilidad de acuerdo con la arquitectura AAA, permitiendo modularidad en la implementación del servidor de seguridad. Es mayormente admitido por Cisco.

Usa el puerto TCP 49. Hace un desafío y respuesta bidireccional como se usa en el protocolo CHAP. Encripta todo el cuerpo del paquete, pero deja un encabezado estándar de TACACS+.

Proporciona autorización de comandos del router por usuario o por grupo. Su contabilidad es limitada.

#### Basada en MAC

Combina autenticación y autorización, pero separa la contabilidad, permitiendo menor flexibilidad en la implementación que TACACS+. Es de estándar abierto (no propietario).

Usa los puertos UDP 1812/1813 o 1645/1646. Hace uso de un desafío y respuesta unidireccional del servidor de seguridad RADIUS al cliente RADIUS. Encripta solamente la contraseña en el paquete de solicitud de acceso del cliente al servidor.

No tiene ninguna opción para autorizar comandos del router por usuario o por grupo y su contabilidad es extensa.

### Registros de Auditoría de AAA

La AAA centralizada también permite el uso de contabilidad. Los registros de contabilidad provenientes de todos los dispositivos se envían a repositorios centralizados, lo que permite simplificar la auditoría de las acciones del usuario. La contabilidad AAA recopila e informa datos de uso en registros AAA que son útiles para la auditoría de seguridad.

Cuando se autentica un usuario, el proceso de auditoría AAA genera un mensaje de inicio que comienza con este proceso.

Cuando el usuario termina, se registra un mensaje de finalización y se da por terminado el proceso de auditoría.

# Monitoreo y Gestión de Dispositivos

## Proteger los Archivos de Imagen y Configuración de Cisco IOS

### Función de Configuración Resiliente de Cisco IOS

La función de configuración resiliente de Cisco IOS permite una recuperación más rápida si alguien reformatea la memoria flash o borra el archivo de configuración de inicio en la NVRAM.

El conjunto de arranque principal (primary boot set) es parte de la función de configuración resiliente de Cisco IOS y almacena una copia oculta y protegida de la imagen del IOS y de la configuración en ejecución.

El archivo de configuración en el conjunto de arranque principal es una copia de la configuración en ejecución que estaba en el enrutador cuando se habilitó la función por primera vez. Esta función garantiza el uso del conjunto mínimo de archivos necesarios para preservar el espacio de almacenamiento persistente.

No se requiere espacio adicional para proteger el archivo de imagen principal de Cisco IOS. Esta función detecta automáticamente las discrepancias en la versión de la imagen o la configuración. Para proteger los archivos, solo se utiliza el almacenamiento local. Esta función solo se puede desactivar a través de una sesión de consola.

### Habilitación de la Función de Resiliencia de Imágenes de IOS

Para proteger la imagen de IOS y habilitar la resiliencia de la imagen de Cisco IOS, utilice el comando de modo de configuración global `secure boot-image`. Al habilitarlo por primera vez, la imagen de Cisco IOS en ejecución queda protegida y se genera una entrada en el registro.

La función de resiliencia de imágenes de Cisco IOS solo se puede deshabilitar a través de una sesión de consola usando el comando `no`. Usar el comando del modo EXEC privilegiado `show secure bootset` para verificar la existencia del archivo.

Para guardar una copia de la running-config, use el comando del modo de configuración global `secure boot-config`.

```bash
R1(config)#secure boot-image
R1(config)#secure boot-config
R1(config)#exit
R1#show secure bootset
```

### Imagen del Conjunto de Arranque Inicial

Restaure un conjunto de arranque principal desde un archivo seguro después de que el enrutador haya sido manipulado siguiendo los pasos:

- **Paso 1.** Reinicie el enrutador usando el comando `reload`. Si es necesario, ejecute la secuencia break para ingresar al modo ROMMON.
- **Paso 2.** Desde el modo ROMMON, introduzca el comando **`dir** *flash-location***:**` para listar el contenido del dispositivo que contiene el archivo de arranque seguro.
- **Paso 3.** Inicie el enrutador con la imagen de arranque segura, utilizando el comando `boot` seguido de la ubicación de la memoria flash, dos puntos y el nombre del archivo encontrado en el paso anterior.
- **Paso 4.** Ingrese al modo de configuración global y restaure la configuración segura en un archivo con el nombre que desee utilizando el comando de configuración global `secure boot-config restore`, seguido de la ubicación de la memoria flash, dos puntos y el nombre del archivo con la configuración.
- **Paso 5.** Salga del modo de configuración global y ejecute el comando `copy` para copiar el archivo de configuración recuperado a la configuración en ejecución.

```bash
Router#**reload**
rommon 1 > **dir** *flash-location***:**
rommon 2 > **boot** *flash-location***:***router-ios.bin*
Router>**enable**
Router#**configure terminal**
Router(config)#**secure boot-config restore** *flash-location***:***rescue-config*
Router(config)#end
Router#**copy** *flash-location***:***rescue-config* **running-config**
```

### Configuración de Secure Copy

La función Secure Copy Protocol (SCP) se utiliza para copiar de forma remota archivos de configuración y de IOS. SCP proporciona un método seguro y autenticado para copiar archivos de configuración o imágenes del router a una ubicación remota. SRC se basa en SSH para asegurar la comunicación y en AAA parar proporcionar autenticación y autorización.

- **Paso 1.** Configure SSH si aún no está configurado. Se requiere tanto del nombre de dominio y de las claves RSA. Esto habilita el puerto 22 para que SCP pueda viajar.
- **Paso 2.** Para autenticación local, configure al menos un usuario en la base de datos local con nivel de privilegios 15.
- **Paso 3.** Habilite AAA con el comando del modo de configuración global `aaa new-model`.
- **Paso 4.** Utilice el comando `aaa authentication login default local` para especificar que se utilice la base de datos local para la autenticación. `authentication login` define cómo se verificará la identidad. `default` indica que se aplica a todas las líneas (VTY, Console). `local` indica que debe usarse la base de datos de usuarios local.
- **Paso 5.** Utilice el comando `aaa authorization exec default local` para configurar la autorización de comandos. Este comando verifica localmente si el usuario autenticado tiene permisos para entrar al modo EXEC privilegiado.
- **Paso 6.** Habilite la funcionalidad del servidor SCP con el comando `ip scp server enable`.

```bash
R1(config)#ip domain-name span.com
R1(config)#crypto key generate rsa general-keys modulus 2048
R1(config)#username Bob privilege 15 algorithm-type scrypt secret cisco12345
R1(config)#aaa new-model
R1(config)#aaa authentication login default local
R1(config)#aaa authorization exec default local
R1(config)#ip scp server enable
```

```bash
R2#copy flash:0R2backup.cfg scp:
Address or name of remote host []?10.1.1.1
Destinantion username [R2]?Bob
Destination filename [R2backup.cfg]?
Writing R2backup.cfg
Password:
```

### Recuperar la Contraseña de un Router

- **Paso 1.** Conéctate al puerto de la consola.
- **Paso 2.** Registre la configuración del registro.
- **Paso 3.** Reinicie el router.
- **Paso 4.** Emitir la secuencia de pausa.
- **Paso 5.** Modifique el registro de configuración predeterminado con el comando `confreg 0x2142`.
- **Paso 6.** Reinicie el router.
- **Paso 7.** Pulse `Ctrl+C` para omitir el proceso de configuración inicial.
- **Paso 8.** Ponga el router en modo EXEC privilegiado.
- **Paso 9.** Copia la configuración de inicio a la configuración en ejecución.
- **Paso 10.** Verifique la configuración.
- **Paso 11.** Cambie la contraseña secreta de habilitación.
- **Paso 12.** Habilite las interfaces.
- **Paso 13.** Reestablezca el registro de configuración a la configuración original registrada en el paso 2. Utilice el comando de configuración global `config-register`.
- **Paso 14.** Guarde los cambios de configuración.

### Recuperación de Contraseñas

Si alguien obtuviera acceso físico a un enrutador, podría, potencialmente, tomar el control de ese dispositivo a través del procedimiento de recuperación de contraseña. Un administrador puede mitigar esta posible brecha de seguridad mediante el comando `no service password-recovery` del modo de configuración global. Este es un comando oculto y al introducirlo, nos pedirá confirmación para habilitarlo.

Cuando esté configurado, el comando `show running-config` muestra un mensaje que indica que está deshabilitado el servicio de recuperación de contraseñas.

Para recuperar un dispositivo después de introducir el comando `no service password-recovery`, inicie la secuencia de interrupción en los cinco segundos posteriores a la descompresión de la imagen durante el arranque. Se le pedirá que confirme la acción de la tecla de interrupción, y una vez confirmada, la configuración de inicio se borrará por completo y se habilitará el proceso de recuperación de contraseña, arrancando el router con la configuración predeterminada de fábrica.

Si no se confirma la acción de interrupción, el router arranca normalmente con el comando `no service password-recovery` habilitado.

## Bloquear un Router Utilizando AutoSecure

### Protocolos de Descubrimiento CDP y LLDP

El protocolo de Descubrimiento de Cisco (CDP, Cisco Discovery Protocol) es un ejemplo de un servicio que está habilitado de forma predeterminada en los routers Cisco.

El protocolo de Descubrimiento de Capa de Enlace (LLDP, Link Layer Discovery Protocol) es un estándar abierto que se puede habilitar en dispositivos Cisco, así como en dispositivos de otros fabricantes compatibles con LLDP.

El objetivo de CDP y LLDP es facilitar a los administradores la detección y resolución de problemas en otros dispositivos de la red. Sin embargo, debido a las implicaciones de seguridad, estos protocolos de detección deben utilizarse con precaución. Los dispositivos de borde son un ejemplo de dispositivos que deberían tener esta función desactivada.

La configuración y verficación de LLDP es similar a la de CDP.

```bash
R1(config)#lldp run
R1(config)#end
R1#show cdp neighbors detail
R1#show lldp neighbors detial
```

### Configuración de Protocolos y Servicios

La configuración por defecto se muestra en las tablas:

!image.png

!image.png

La configuración recomendada se muestra en las tablas:

!image.png

### Cisco AutoSecure

AutoSecure puede bloquear las funciones del plano de administración y los servicios y funciones del plano de reenvío de un router.

#### Management Plane

- **Desactiva Small Servers (Servicios pequeños).** Desactiva protocolos de descubrimiento como CDP o BOOTP, que pueden filtrar información sobre la topología. Desactiva servicios como Echo, Discard, Figner, FTP, TFTP, PAD, UDP y TCP, MOP, ICMP (redirecciones, respuestas enmascaradas), enrutamiento de origen IP, TCP keepalives, ARP gratuito, proxy ARP y difusión dirigida.
- **Seguridad de acceso.** Aviso legal mediante banner, fuerza el uso de SSH, asegura que las contraseñas no se guarden en texto plano.
- **NTP Seguro.** Asegura que el tiempo del sistema (vital para los logs) venga de una fuente confiable.

#### Forwarding Plane

- **Cisco Express Forwarding (CEF).** Lo habilita por defecto. Es el método más rápido y eficiente para procesar paquetes; además ayuda a prevenir ciertos ataques de DoS.
- **Filtros de tráfico (ACL).** Configura listas de control de acceso para bloquear tráfico malicioso conocido.
- **Inspección de Firewall.** Activa funciones básicas de firewall de IOS para monitorear protocolos comunes y asegurar que las conexiones sean legítimas.

### Sintaxis de Comandos de Cisco AutoSecure

Utilice el comando `auto secure` para habilitar la configuración de la función Cisco AutoSecure. Esta configuración peude ser interactiva o no interactiva.

```bash
Router#**auto secure** {**no-interact** | **full**} [**forwarding** | **management**] [**ntp** | **login** | **ssh** | **firewall** | **top-intercept**]
```

- **`full`**. Es la opción recomendada. Inicia el asistente completo que cubre tanto el acceso al router como el tráfico que pasa por él.
- **`no-interact`**. Es la opción automática total. Aplica las configuraciones recomendadas por Cisco sin preguntar nada. Puede bloquear accesos legítimos si no conoces los valores por defecto de Cisco.
- `[**management** | **forwarding**]`. Permite ejecutar el asistente solo para una de las dos áreas.
- `[**ntp** | **login** | **ssh** | **firewall** | **top-intercept**]`. Si solo se desea que el asistente ayude a asegurar una función puntual.

### Utilizando el Comando `auto secure`

Un asistente de línea de comandos guía al administrador a través de la configuración. Se recomienda usar AutoSecure durante la configuración inicial del router. No se recomienda su uso en routers de producción.

- **Paso 1.** Se introduce el comando.
- **Paso 2.** El asistente recopila información sobre las interfaces externas.
- **Paso 3.** AutoSecure protege el plano de administración deshabilitando los servicios innecesarios.
- **Paso 4.** AutoSecure solicita un banner.
- **Paso 5.** AutoSecure solicita las contraseñas y habilita las funciones de contraseña e inicio de sesión.
- **Paso 6.** Las interfaces están protegidas.
- **Paso 7.** El plano de envío está asegurado.

## Autenticación del Protocolo de Enrutamiento

### Protocolos de Enrutamiento Dinámico

Los protocolos de enrutamiento realizan varias actividades, entre ellas el descubrimiento de redes y el mantenimiento de las tablas de enrutamiento.

Las ventajas importantes de los protocolos de enrutamiento dinámico son la capacidad de seleccionar la mejor ruta y la capacidad de descubrir automáticamente una nueva mejor ruta cuando se produce un cambio en la topología.

Un protocolo de enrutamiento dinámico permite que los routers aprendan automáticamente sobre estas redes a partir de otros routers.

### Suplantación de Protocolo de Enrutamiento

Los sistemas de enrutamiento pueden ser atacados interrumpiendo los enrutadores de la red par o falsificando o suplantando la información contenida en los protocolos de enrutamiento. La suplantación de información de enrutamiento generalmente se puede usar para hacer que los sistemas se engañen entre sí, provocar un ataque de DoS o hacer que el tráfico siga una ruta que normalmente no seguiría, pudiendo crearse bucles de enrutamiento o ataques MiTM.

### Autenticación del Protocolo de Enrutamiento OSPF MD5

Habilitar la autenticación OSPF MD5 globalmente:

- Usar el comando de configuración de interfaz **`ip ospf message-digest-key** *key* **md5** *password*`.
    - `*key*`. Número del 1 al 255. Ambos routers vecinos deben usar el mismo ID y la misma contraseña, de lo contrarion no se formará adyacencia.
    - **`md5`**. Especifica el algoritmo de cifrado.
    - `*password*`. Contraseña compartida (hasta 8 caracteres).
- Para el método por áreas, se especifica el área en la cual todos sus routers usen autenticación obligatoriamente. Primero se entra al proceso OSPF con el comando de configuración global **`router ospf** *process-id*`.
- Se continúa activando la autenticación para toda el área con el comando de configuración de router **`area** *area-id* **authentication message-digest**`.
- Este método fuerza la autenticación en todas las interfaces con OSPF habilitado. Si una interfaz no está configurada con el comando `ip ospf message-digest-key`, no podrá establecer adyacencias con otros vecinos OSPF.

Habilitar la autenticación OSPF MD5 por interfaz:

- Usar el comando de configuración de interfaz para la interfaz que se desea configurar en el modo de configuración de interfaz **`ip ospf message-digest-key** *key* **md5** *password*`.
- Se activa la autenticación solo en esta interfaz con el comando de configuración de interfaz **`ip ospf authentication message-digest`**.

```bash
R1#conf t
R1(config)#interface s0/0/0
R1(config-if)#ip ospf message-digest-key 1 md5 cisco12345
R1(config-if)#ip ospf authentication message-digest

R2#conf t
R2(config)#interface s0/0/0
R2(config-if)#ip ospf message-digest-key 1 md5 cisco12345
R2(config-if)#ip ospf authentication message-digest
```

Actualmente, MD5 se considera vulnerable y solo debe utilizarse cuando no se disponga de una autenticación más segura. Los administradores deben usar la autenticación SHA siempre que todos los sistemas operativos de los enrutadores sean compatibles con la autenticación OSPF SHA.

### Autenticación del Protocolo de Enrutamiento OSPF SHA

HMAC-SHA-256 ofrece una resistencia criptográfica mucho mayor. Además, el uso de key chains permite la rotación de claves sin caída de servicio: puedes programar que una clave expire y otra se active automáticamente en un horario específico.

- **Paso 1.** Especifique una cadena de claves de autenticación en el modo de configuración global.
    - Configure un nombre para el llavero con el comando **`key chain** *name*`.
    - Asigne un número y una contraseña al llavero con los comandos **`key** *key-id*` y **`key-string** *string*`.
    - Especifique la autenticación SHA con el comando **`cryptographic-algorithm`**.
    - Opcionalmente, especifique cuándo caducará esta clave con el comando **`send-lifetime`**.

```bash
Router(config)#**key chain** *name*
Router(config-keychain)#**key** *key-id*
Router(config-keychain-key)#**key-string** *string*
Router(config-keychain-key)#**cryptographic-algorithm** {**hmac-sha-1** | **hmac-sha-256** | **hmac-sha-384** | **hmac-sha-512** | **md5**}
Router(config-keychain-key)#**send-lifetime start-time** {**infinite** | **end-time** | **duration** *seconds*}
```

- **Paso 2.** Asigne la clave de autenticación a las interfaces deseadas con el comando **`ip ospf authentication key-chain`**. Aquí no se necesita el comando `ip ospf authentication message-digest`. El simple hecho de vincular `key-chain` activa la autenticación automáticamente en la interfaz.

```bash
Router(config)#**interface** *type number*
Router(config-if)#**ip ospf authentication key-chain** *name*
```

!image.png

```bash
R1(config)#key chain SHA256
R1(config-keychain)#key 1
R1(config-keychain-key)#key-string ospfSHA256
R1(config-keychain-key)#cryptographic-algorithm hmac-sha-256
R1(config-keychain-key)#exit
R1(config-keychain)#exit
R1(config)#interface s0/0/0
R1(config-if)#ip ospf authentication key-chain SHA256

R2(config)#key chain SHA256
R2(config-keychain)#key 1
R2(config-keychain-key)#key-string ospfSHA256
R2(config-keychain-key)#cryptographic-algorithm hmac-sha-256
R2(config-keychain-key)#exit
R2(config-keychain)#exit
R2(config)#interface s0/0/0
R2(config-if)#ip ospf authentication key-chain SHA256
```

## Gestión e Informes Seguros

### Tipos de Acceso de Gestión

Desde el punto de vista de los informes, la mayoría de los dispositivos de red pueden enviar datos de registro que pueden ser invaluables al solucionar problemas de red o amenazas de seguridad.

Al registrar y administrar información, el flujo de información entre los hosts de administración y los dispositivos adminisrados puede seguir dos rutas:

- **En banda.** La información fluye a través de una red de producción empresarial, internet o ambas, utilizando canales de datos habituales.
- **Fuera de banda (OOB).** La información fluye a través de una red de gestión dedicada en la que no reside tráfico de producción

### Acceso Fuera de Banda y Dentro de Banda

La gestión fuera de banda es apropiada para grandes redes empresariales, sin embargo, no siempre es lo más conveniente. La decisión de utilizar la gestión OOB depende del tipo de aplicaciones de gestión en ejecución y de los protocolos que se estén monitorizando.

Las directrices de gestión OOB son:

- Proporcionar el máximo nivel de seguridad.
- Mitigar el riesgo de transmitir protocolos de gestión inseguros a través de la red de producción.
- Se recomienda la gestión en banda en redes pequeñas para lograr una implementación de seguridad más rentable.
- En estas arquitecturas, el tráfico de gestión fluye siempre dentro de la banda.
- Se garantiza la máxima seguridad mediante protocolos de gestión seguros, como SSH en lugar de Telnet.

Las directrices de gestión dentro de la banda son:

- Aplicar únicamente a lo dispositivos que necesiten ser gestionados o supervisados.
- Utilice IPsec, SSH o SSL siempre que sea posible.
- Decida si el canal de gestión debe estar abierto en todo momento.

## Seguridad de Red Mediante Syslog

### Introducción a Syslog

El método más común para acceder a los mensajes del sistema es utilizar un protocolo llamado syslog. Syslog es un término que se utiliza para describir un estándar. También se usa para describir el protocolo desarrollado para dicho estándar.

El protocolo syslog permite que los dispositivos de red envíen sus mensajes del sistema a través de la red (puerto UDP 514) a los servidores syslog. El servicio de registro syslog proporciona tres funciones principales:

- La capacidad de recopilar información de registro para la monitorización y la resolución de problemas.
- La capacidad de seleccionar el tipo de información de registro que se captura.
- La capacidad de especificar los destinos de los mensajes de syslog capturados.

### Operación de Syslog

En los dispositivos de red de Cisco, el protocolo syslog comienza enviando mensajes del sistema y resultados de depuración a un proceso de registro local interno al dispositivo. La forma en que este proceso gestiona dichos mensajes y resultados depende de la configuración del dispositivo.

Los destinos más comunes pra los mensajes de syslog incluyen búfer de registro (memoria RAM dentro de un enrutador o conmutador), línea de consola, línea de terminal, servidor Syslog. Puede usar **`show logging`** para ver los logs en el buffer interno.

Es posible supervisar de forma remota los mensajes del sistema consultando los registros en un servidor syslog o accediendo al dispositivo a través de Telnet, SSH o mediante el puerto de consola.

### Formato de Mensaje de Syslog

Los dispositivos Cisco generan mensajes syslog como resultado de eventos de red. Cada mensaje syslog contiene un nivel de gravedad y una función. Los niveles numéricos más bajos corresponden con alarmas de syslog más críticas.

!image.png

### Instalaciones de Syslog

Las funciones de Syslog son identificadores de servicio que identifican y categorizan los datos de estado del sistema para la generación de informes de errores y eventos. Las opciones de registro disponibles son específicas del dispositivo de red.

Por defecto, el formato de los mensajes syslog en el software Cisco IOS es el siguiente:

```bash
%facility-severity-MNEMONIC: description
```

Por ejemplo, la salida de ejemplo en un switch Cisco para un enlace EtherChannel que cambia de estado a activo es:

```bash
%LINK-3-UPDOWN: Interface Port-channell, changed state to up
```

Aquí la instalación es **LINK**, el nivel de gravedad es **3**, con un MNEMOTIC de **UPDOWN**.

### Configurar Marcas de Tiempo de Syslog

Por defecto, los mensajes de registro no tienen marca de tiempo. Es recomendable que los mensajes de registro tengan marca de tiempo para que, al enviarlos a otro destino, como un servidor Syslog, quede constancia de cuándo se generó el mensaje.

Utilice el comando del modo de configuración global **`service timestamps log datetime`** para forzar que los eventos registrados muestren la fecha y la hora.

### Sistemas Syslog

Las implementaciones de Syslog siempre contienen dos tipos de sistemas:

- **Servidores Syslog.** También conocidos como hosts de registros, estos sistemas aceptan y procesan los mensajes de registro de los clientes syslog.
- **Clientes Syslog.** Routers u otro tipo de equipos que generan y reenvían mensajes de registro a servidores syslog.

!image.png

### Configuración de Syslog

- **Paso 1.** Configure el log host de destino utilizando el comando del modo de configuración global **`logging** *ip-address*`.
- **Paso 2.** De forma opcional, se define el filtro de severidad, el cual establece un nivel máximo desde el cual enviar mensajes syslog. Usar el comando del modo de configuración global **`logging trap** *severity-level*`.
- **Paso 3.** Por defecto, el router usa la IP de la interfaz por la que sale el paquete. Si se tienen varias rutas, el servidor vería logs viniendo de diferentes IP del mismo router. Para definir una interfaz de origen, se usa el comando del modo de configuración global **`logging source-interface** *interface*`. Se recomienda usar una Loopback (`Loopback0` o `lo0`) porque es una interfaz virtual que nunca se cae. Así, el servidor siempre identifica al router por una sola IP única.
- **Paso 4.** Habilite el servicio en todos los clientes habilitados con el comando `logging on`.

Los niveles de syslog son:

- **0 - Emergencies.** Sistema inestable.
- **1 - Alerts.** Acción inmediata necesaria.
- **2 - Critical.** Condiciones críticas.
- **3 - Errors.** Condiciones de error.
- **4 - Warnings.** Advertencias.
- **5 - Notifications.** Eventos normales perso significativos.
- **6 - Informational.** Mensajes informativos (nivel estándar).
- **7 - Debugging.** Solo para pruebas (genera mucho tráfico.

```bash
R1(config)#logging 10.2.2.6
R1(config)#logging trap informational
R1(config)#logging source-interface lo0
R1(config)#logging on
```

## Configuración de NTP

### Servicios de Hora y Calendario

El reloj del software de un router o switch se inicia al arrancar el sistema. Es la principal fuente de hora del sistema. Es importante sincronizar la hora en todos los dispositivos de la red, ya que todos los aspectos de la administración, seguridad, resolución de problemas y planificación de redes requieren una sincronización horaria precisa.

La fecha y hora en un router o switch se puede configurar manualmente mediante el comando del modo EXEC privilegiado **`clock set** [*hh***:***mm***:***ss*] [*day*] [*month*] [*year*]`.

### Operación de NTP

Las redes NTP utilizan un sistema jerárquico de fuentes de tiempo. Cada nivel de este sistema jerárquico se denomina estrato (stratum). El nivel del estrato se define por el número de saltos desde la fuente autorizada.

### Configurar y Verificar NTP

Para verificar la fuente de la hora configurada en el dispositivo, usar el comando del modo EXEC privilegiado `show clock detail`.

Para configurar el servidor NTP que el dispositivo debe usar como origen, utilice el comando del modo de configuración global **`ntp server** *ip-address`.* 

Para dispositivos Cisco, cuando no haya internet y se quiera configurar un router como el servidor NTP, se configura mediante el comando del modo de configuración global **`ntp master** [*stratum*]`, donde el valor por defecto para el estrato es de 8 y puede variar entre 0 y 15.

Para configurar la autenticación MD5 para NTP, seguimos los pasos:

- **Paso 1.** Definir llave con el comando de configuración global **`ntp authentication-key** *key* **md5** *password*`.
    - `*key*`. Este número debe ser el mismo en el servidor y cliente.
    - **`md5`**. Algoritmo de hashing.
    - `*password*`. La contraseña compartida.
- **Paso 2.** Declaramos la llave como confiable. A pesar de todas las llaves que se puedan tener definidas, solo las que se pongan aquí serán aceptadas para sincronizar la hora. Se usa el comando de configuración global **`ntp trusted-key** *key*`.
- **Paso 3.** Activar la autenticación globalmente. Se usa el comando del modo de configuración global **`ntp authenticate`**. Sin este comando, el router aceptará cualquier paquete NTP, tras activarlo, solo los que tengan llaves confiables.
- **Paso 4.** Asociar la llave al servidor mediante el comando del modo de configuración global **`ntp server** *ip-address* **key** *key*`.

Utilizar los comandos del modo EXEC privilegiado **`show ntp associations`** y **`show ntp status`** para verificar que el dispositivo esté sincronizado con el servidor NTP.

## Configuración SNMP

### Introducción a SNMP

SNMP (Simple Network Management Protocol) es un protocolo de la capa de aplicación que proporciona un formato de mensaje para la comunicación entre administradores y agentes. El sistema SNMP consta de tres elementos:

- Administrador SNMP
- Agentes SNMP (nodo gestionado)
- Base de Información de Gestión (MIB, Management Information Base), la cual es una base de datos jerárquica en el router que organiza la información (tráfico, temperatura, CPU)

Para configurar SNMP en un dispositivo de red, primero es necesario definir la relación entre el administrador y el agente. El gestor SNMP forma parte de un sistema de gestión de red (NMS). El gestor SNMP ejecuta el software de gestión SNMP.

El administrador puede recopilar información de un agente mediante la acción “get”. También puede modificar la configuración de un agente mediante la acción “set”.

Los agentes pueden enviar información directamente a un administrador de red mediante “traps”.

!image.png

### Operación SNMP

Existen dos solicitudes principales del administrador SNMP:

- **Solicitud GET.** Es una operación de consulta. El NMS la utiliza para extraer datos específicos del dispositivo.
- **Solicitud de configuración (SET).** La utiliza el sistema de getsión de red (NMS) para modificar variables en el dispositivo agente. También puede disparar acciones físicas.

El administrador SNMP utiliza las acciones get y set para realizar las operaciones descritas en la tabla.

- `get-request`. Recupera el valor de una variable específica definida en la MIB.
- `get-next-request`. Se utiliza para recorrer tablas de datos de forma secuencia sin conocer el nombre exacto de la siguiente variable.
- `get-bulk-request`. Permite recuperar grandes bloques de datos en una sola transmisión. Solo disponible a partir de SNMPv2.
- `get-response`. Respuesta enviada hacia el NMS como contestación a cualquier solicitud get, get-next o set.
- `set-request`. Es la acción de escritura. Almacena o guarda un nuevo valor en una variable específica del dispositivo.

### Management Information Base (MIB)

La MIB organiza todas las variables de forma estructural, similar a los directorios en un ordenador. Estas variables permiten que el software de gestión de red (NMS) no solo monitoree el estado del dispositivo, sino que también tome el control sobre él.

Formalmente, la MIB define cada variable como un identificador de objeto (OID). Los OID identifican de forma única los objetos gestionados en la jerarquía de la MIB. La MIB organiza los OID según los estándares RFC en una jerarquía, que generalmente se representa como un árbol.

!image.png

Estos números representan el camino que el software recorre para encontrar un dato exacto dentro del router.

- **1 (iso).** La carpeta raíz.
- **3 (org).** Subcarpeta para organizaciones.
- **6 (dod).** Carpeta creada por el Departamento de Defensa de EE.UU.
- **1 (internet).** El mundo de internet.
- **4 (private).** Carpetas para empresas privadas.
- **1 (enterprises).** Subcarpeta de fabricantes.
- **9 (cisco).** El home de Cisco.

Así, cualquier número que empiece por 1.3.6.1.4.1.9 le dice al software que está buscando algo que solo existe en equipos Cisco.

### Versiones SNMP

- **SNMPv1.** Este es el Protocolo Simple de Administración de Red, un estándar completo de internet, definido en el RFC 1157.
- **SNMPv2c.** Está definido en los RFC 1901 a 1908. Utiliza un framework administrativo basado en cadenas de comunidad.
- **SNMPv3.** Este es un protocolo interoperable basado en estándares, definido originalmente en los RFC 2273 a 2275. Proporciona acceso seguro a los dispositivos mediante la autenticación y el cifrado de paquetes a través de la red.

Todas las versiones utilizan gestores, agentes y MIB SNMP. El software Cisco IOS es compatible con las tres versiones mencionadas. Tanto SNMPv1 como SNMPv2c emplean un sistema de seguridad basado en la comunidad. La comunidad de gestores que pueden acceder a la MIB del agente se define mediante una cadena de comunidad. SNMPv3 ofrece modelos y niveles de seguridad.

### Vulnerabilidades de SNMP

En cualquier topología de red, al menso un nodo de gestión debe ejecutar software de gestión SNMP. Los dispositivos de red gestionables, como switches, routers, servidores y estaciones de trabajo, están equipados con el módulo de software agente SNMP.

SNMP es vulnerable a ataques precisamente porque los agentes SNMP pueden ser consultados mediante solicitudes GET y aceptar cambios de configuración mediante solicitudes SET.

### Configuración de Seguridad SNMPv3

SNMPv3 proporciona integridad y autenticación, cifrado y control de acceso.

- **Paso 1.** Configure una ACL que permita el acceso a los administradores SNMP autorizados.

```bash
Router(config)#**ip access-list** *acl-name*
Router(config-std-nacl)#**permit** *source_net*
```

- **Paso 2.** Configure una vista SNMP con el comando de configuración global `snmp-server view` para identificar los OID MIB que el administrador SNMP podrá leer. Es necesario configurar una vista para limitar el acceso a los mensajes SNMP a solo lectura.

```bash
Router(config)#**snmp-server view** *view-name oid-tree*
```

- **Paso 3.** Configure las funciones del grupo SNMP con el comando del modo de configuración global `snmp-server group`:
    - Configure un nombre para el grupo
    - Establezca la versión SNMP en 3 con la palabra clave **`v3`**.
    - Requiere de autenticación y cifrado con la palabra clave **`priv`**.
    - Asocie una vista al grupo y otórguele acceso de solo lectura con el comando **`read`**.
    - Especifique la ACL configurada en el paso 1.

```bash
Router(config)#**snmp-server group** *group-name* **v3 priv read** *view-name* **access** [*acl-number* | *acl-name*]
```

- **Paso 4.** Configure las características de usuario del grupo SNMP con el comando `snmp-server user`:
    - Configure un nombre de usuario y asócielo con el nombre de grupo configurado en el paso 3.
    - Establezca la versión SNMP en 3 con la palabra clave **`v3`**.
    - Establezca el tipo de autenticación en **`md5`** o **`sha`** y configure una contraseña de autenticación. Se recomienda SHA, y este método debe ser compatible con el software de administración SNMP.
    - Requiere cifrado con la palabra clave **`priv`** y configurar una contraseña.

```bash
Router(config)#**snmp-server user** *username group-name* **v3 auth** {**md5** | **sha**} *auth-password* **priv** {**des** | **3des** | **aes** {**128** | **192** | **256**}} *priv-password*
```

La configuración en el agente empieza con crear la comunidad. Se pueden crear las comunidades de solo lectura y solo escritura como sigue:

```bash
Sw(config)#**snmp-server community** *password* **ro**
Sw(config)#**snmp-server community** *password* **rw**
```

Estas se deben habilitar en cada agente

### Ejemplo de Configuración de Seguridad SNMPv3

- **Paso 1.** Se crea una ACL estándar llamada **PERMIT-ADMIN** y está configurada para permitir únicamente la red **192.168.1.0/24**.
- **Paso 2.** Se crea una vista SNMP con el nombre **SNMP-RO** y se configura para incluir todo el árbol **iso** de la **MIB**.
- **Paso 3.** Se configura un grupo SNMP con el nombre **ADMIN**, SNMPv3 y acceso para aquellos permitidos con la ACL **PERMIT-ADMIN**.
- **Paso 4.** Se configura un usuario SNMP llamado **BOB** como miembro del grupo **ADMIN** usando SNMPv3, con autenticación SHA, cifrado AES 256 y la contraseña de cifrado dada.

```bash
R1(config)#ip access-list standard PERMIT-ADMIN
R1(config-std-nacl)#permit 192.168.1.0 0.0.0.255
R1(config-std-nacl)#exit
R1(config)#snmp-server view SNMP-RO iso included
R1(config)#snmp-server group ADMIN v3 priv priv read SNMP-RO access PERMIT-ADMIN
R1(config)#snmp-server user BOB ADMIN v3 auth sha cisco12345 priv aes 256 cisco54321
R1(config)#end
R1#
```

Para verificar la configuración, consulte la running-config. Utilice el comando del modo EXEC privilegiado `show snmp user` para ver la información del usuario.

```bash
R1#show runnin-config | include snmp
R1#show snmp user
```

# Conceptos de ACL

## Propósito de las ACL

Una ACL es una serie de comandos del IOS que controlan si un router reenvía o descarta paquetes según la información que se encuentra en el encabezado del paquete. De forma predeterminada, un router no tiene ninguna ACL configurada. Sin embargo, cuando se aplica una ACL a una interfaz, el router realiza la tarea adicional de evaluar todos los paquetes de red a medida que pasan a través de la interfaz para determinar si el paquete se puede reenviar.

Una ACL es una lista secuencial de instrucciones `permit` o `deny`, conocidas como entradas de control de acceso (ACE), también denominadas instrucciones de ACL.

Cuando el tráfico de red pasa a través de una interfaz configurada con una ACL, el router compara la información dentro del paquete con cada ACE, en orden secuencial, para determinar si el paquete coincide con una de las ACE. Este proceso se denomina filtrado de paquetes.

Permiten limitar tráfico para controlar el flujo, proporcionar un nivel básico de seguridad, filtrar según el tipo de tráfico, filtrar hosts y proporcionar prioridad a determinadas clases de tráfico de red.

### Filtrado de Paquetes

El filtrado de paquetes controla el acceso a la red mediante el análisis de los paquetes entrantes y salientes y la transferencia o el descarte de estos según criterios determinados.

El filtrado de paquetes se puede realizar en capa 3 o capa 4. Los routers Cisco admiten dos tipos de ACL:

- **ACL estándar.** Solo filtran paquetes en la capa 3 utilizando únicamente la dirección IPv4 de origen.
- **ACL extendidas.** Filtran en la capa 3 mediante dirección IPv4 de origen y/o destino. También pueden filtrar en la capa 4 usando TCP, puertos UDP e información de tipo de protocolo opcional para un control más fino.

### Funcionamiento de una ACL

Las ACL se pueden configurar para filtrar tanto el tráfico entrante como saliente. Las ACL no operan sobre paquetes que se originan en el router mismo. Las ACL de entrada filtran los paquetes que ingresan a una interfaz específica antes de que se enruten en la salida, ahorrando la sobrecarga de enrutar búsquedas si el paquete se descarta. Las ACL de salida filtran después de que se enrutan independientemente de la interfaz de entrada.

Cuando se aplica una ACL a una interfaz, sigue el procedimiento operativo. Para una ACL estándar:

1. Un router configurado con una ACL de IPv4 estándar recupera la dirección IPv4 de origen del encabezado del paquete.
2. El router comienza en la parte superior de la ACL y compara la dirección con cada ACE de manera secuencial.
3. Si se encuentra coincidencia, el router realiza la instrucción, que puede ser permitir o denegar el paquete. Las demás entradas no son analizadas.
4. Si la dirección IPv4 de origen no coincide con ninguna ACE de la ACL, el paquete se descarta porque hay una ACE de denegación implícita aplicada automáticamente a todas las ACL.

La última instrucción de una ACL siempre es una instrucción deny implícita que bloquea todo el tráfico. Está oculta y no se muestra en la configuración. Una ACL deberá tener al menos una instrucción permit o todo el tráfico se denegará.

### Máscaras Wildcard en ACL

Una máscara wildcard es similar a una máscara de subred, ya que utiliza el proceso ANDing para identificar los bits de una dirección IPv4 que deben coincidir. Sin embargo, a diferencia de una máscara de subred en la que el 1 binario equivale a una coincidencia y el 0 binario no es una coincidencia, en las máscaras wildcard es al revés.

Un ACE IPv4 utiliza una máscara wildcard de 32-bits para determinar qué bits de la dirección debe examinar para obtener una coincidencia. Las máscaras wildcard utilizan las siguientes reglas para establecer la coincidencia entre unos y ceros binarios:

- **Máscara wildcard bit 0.** Se establece la coincidencia con el valor del bit correspondiente a la dirección.
- **Máscara wildcard bit 1.** Se omite el valor del bit correspondiente en la dirección.

!image.png

Supongamos que **ACL 10** necesita una ACE que solo permita un host con dirección IPv4 `192.168.1.1`. Para que coincida con la dirección IPv4 específica, se requiere una wildcard que conste de ceros, es decir, `0.0.0.0`. El ACE resultante sería `access-list 10 permit 192.168.1.1 0.0.0.0`.

Supongamos ahora que **ACL 10** necsita una ACE que permita todos los hosts de la red `192.168.1.0/24`. La máscara wildcard adecuada será `0.0.0.255`, pues indica que los tres primeros octetos coincidan, pero el cuarto no. El ACE resultante será `access-list 10 permit 192.168.1.0 0.0.0.255`.

Suponinendo que **ACL 10** necesita que se permitan todos los hosts en las redes desde `192.168.16.0/24` hasta `192.168.31.0/24`. La wildcard adecuada será 0.0.15.255. El ACE será `access-list 10 permit 192.168.16.0 0.0.15.255`.

### Palabras Clave de una Máscara Wildcard

Cisco IOS proporciona dos palabras clave para los usos más comunes de enmascaramiento wildcard:

- **`host`**. Reemplaza la máscara `0.0.0.0`. Esta máscara indica que todos los bits de la dirección IPv4 deben coincidir para filtrar solo una dirección de host.
- **`any`**. Sustituye la máscara `255.255.255.255`. Esta máscara estblece que se omita la dirección IPv4 completa o que se acepte cualquier dirección.

## Pautas para la Creación de ACL

### Número de ACL por Interfaz

Existe un límite en el número de ACL que se pueden aplicar en una interfaz de router. Por ejemplo, una interfaz dual-stack de un router, es decir, IPv4 e IPv6, puede tener hasta cuatro ACL aplicadas.

!image.png

Las ACL no deben configurarse en ambos sentidos. El número de ACL y su dirección aplicada a la interfaz dependerá de la política de seguridad de la organización.

!image.png

## Tipos de ACL IPv4

### ACL IPv4 Estándar y Extendidas

- **ACL estándar.** Permiten o denienga basados únicamente en la dirección IPv4 de origen.
- **ACL extendida.** Permite o deniega paquetes basados en la dirección IPv4 de origen y la dirección IPv4 de destino, el tipo de protocolo, los puertos TCP o UDP de origen y destino y más.

### ACL IPv4 Numeradas y Nombradas

#### ACL Numeradas

Las ACL numeradas 1-99 o 1300-1999 son ACL estándar, mientras que las ACL numeradas 100-199 o 2000-2699 son ACL extendidas.

#### ACL Nombradas

Las ACL con nombre son el método preferido para configurar ACL. Específicamente las ACL estándar y extendidas se pueden nombrar para proporcionar información sobre el propósito de la ACL. El comando de configuración global `ip access-ilst` se utiliza para crear una ACL con nombre.

### Dónde Ubicar las ACL

Cada ACL debe colocarse donde tenga más impacto en la eficiencia. Las ACL extendidas deben ubicarse lo más cerca posible del origen del tráfico que se desea filtrar. Las ACL estándar deben aplicarse lo más cerca posible del destino.

!image.png

!image.png

Considerando un ejemplo donde el administrador desee impedir que el tráfico originado en la red `192.168.10.0/24` llegue a la red `192.168.30.0/24`, se tendrá:

!image.png

Si bien es cierto, las ACL extendidas se deben ubicar lo más cerca posible del origen del tráfico, los adminsitradores de red solo pueden colocar las listas ACL en los dispositivos que controlan. Por lo tanto, la colocación se debe determinar en el contexto de hasta dónde se extiende el control del administrador de red.

# ACL para Configuración IPv4

## Configurar ACL IPv4 Estándar

### Crear una ACL

Todas las listas de control de acceso (ACL) deben planificarse. Al configurar una ACL compleja, se sugiere que utilice un editor de texto y escriba los detalles de la política que se implementará. Agregue los comandos de configuración de IOS para realizar esta tarea e incluya comentarios para documentar la ACL. Copie y pegue los comandos en el dispositivos y pruebe exhaustivamente una ACL para asegurarse de que aplica correctamente la política deseada.

### Sintaxis de una ACL de IPv4 Estándar Numerada

Para crear una ACL estándar numerada, utilice el comando `access-list`.

```bash
Router(config)#**access-list** *access-list-number* {**deny** | **permit** | **remark** *text*} *source* [*source-wildcard*] [**log**]
```

- `*access-list-number*`. El rango de números es de 1 a 99 o de 1300 a 1999.
- **`deny`**. Deniega el acceso si se dan las condiciones.
- **`permit`**. Permite el acceso si se dan las condiciones.
- **`remark** *text*`. Entrada de texto opcional para fines de documentación.
- `*source*`. Identifica la red de origen o la dirección de host que se va a filtrar.
- `*source-wildcard*`. Entrada opcional con la máscara wildcard de 32 bits para aplicarla al origen.
- **`log`**. Entrada opcional que genera y envía un mensaje de log cuando el ACE coincide.

Utilice el comando de configuración global **`no access-list** *access-list-number*` para eliminar una ACL estándar numerada.

### Sintaxis de una ACL de IPv4 Estándar Nombrada

Para crear una ACL estándar nombrada, utilice el comando `ip access-list standard`. Los nombres de las ACL son alfanuméricos, distinguen mayúsculas de minúsculas y deben ser únicos.

```bash
Router(config)#**ip access-list standard** *access-list-name*
```

Tras ingresar el comando, se entrará al modo de configuración de listas de acceso estándar con nombre. Dentro de esta, podemos ingresar comandos para gestión o acción, además de que se tiene la opción de ingresar un número de secuencia antes del comando para que este se inserte en la posición exacta. Por defecto cisco pone las reglas en posiciones numeradas de 10 en 10. Los comandos:

- `default`. Devuelve un comando específico a su comportamiento original o predeterminado.
- `deny`. Especifica paquetes a rechazar.
- `exit`. Devuelve al modo de configuración global.
- `no`. Niega un comando o lo establece por defecto. Puede borrar reglas específicas, para las cuales solo será necesario ingresar el número de línea.
- `permit`. Define los paquetes a enviar.
- `remark`. Para poner un comentario o una descripción.

### Aplicación de la ACL IPv4 Estándar

Después de configurar una ACL IPv4 estándar, debe vincularse a una interfaz o entidad. El comando del modo de configuración de interfaz `ip access-group` se utiliza para enlazar una ACL IPv4 estándar numerada o nombrada a una interfaz. Para eliminar una ACL de una interfaz, primero introduzca el comando `no ip access-group` en el modo de configuración de interfaz.

```bash
Router(config-if)#**ip access-group** {*access-list-number* | *access-list-name*} {**in** | **out**}
```

Use el comando **`show running-config | section access-list`** para revisar el ACL en la configuración.

Use el comando **`show ip int** *interface* **| include access list**` para verificar que el ACL está aplicado a la interfaz.

El prefijo de comando `do` permite ejecutar comandos del modo EXEC privilegiado en el modo de configuración global o de otros modos sin tener que salir de estos.

Use el comando **`show access-lists`** para ver todas las ACL en el dispositivo.

## Modificación de ACL IPv4

### Método de Números de Secuencia

Una ACE se puede eliminar o agregar utilizando los números de secuencia ACL. Para las listas numeradas, los números de secuencia son múltiplos de 10, empezando con el 10. Se puede acceder al modo de configuración de listas de acceso estandar nombradas para listas numeradas usando el número como su nombre. Se deben eliminar las lineas que se deseen cambiar y luego escribir los comandos adecuados.

### Estadísticas de una ACL

Con el comando `show access-lists`, se muestran las estadísticas de cada sentencia. La declaración implícita `deny any` no mostrará ninguna estadística. Para realizar seguimiento, y la mejor práctica, es configurar manualmente el comando `deny any` para finalizar cada lista de acceso.

Para borrar las estadísticas de las ACL, use el comando **`clear access-list counters** *access-list*`.

## Protección de Puertos VTY con una ACL IPv4 Estándar

### Comando `access-class`

Una ACL estándar puede proteger el acceso administrativo remoto a un dispositivo mediante las líneas VTY implementando los  pasos:

- Cree una ACL para identificar a qué hosts administrativos se debe permitir el acceso remoto.
- Aplique la ACL al tráfico entrante en las líneas VTY.

```bash
R1(config-line)#**access-class** {*access-list-number* | *access-list-name*} {**in** | **out**}
```

## Configuración de ACL IPv4 Extendidas

Las ACL extendidas proporcionan un mayor rango de control, filtrando dirección de origen, destino, protocolo (IP, TCP, UDP, ICMP) y número de puerto. Estas se pueden crear como:

- **ACL extendida numerada.** Creada mediante el comando de configuración global **`access-list** *access-list-number*`. Usa un identificador numérico en el rango de 100 a 199 o 2000 a 2699.
- **ACL extendida nombrada.** Se crean usando el comando **`ip access-list extended** *access-list-name*`.

### Sintaxis de ACL Extendida Numerada IPv4

```bash
**access-list** *access-list-number* {**deny** | **permit** | **remar** *text*} *protocol source source-wildcard* [*operator* {*port*}] *destination destination-wildcard* [*operator* {*port*}] [**established**] [**log**]
```

- `*access-list-number*`. Número decimal entre 100 a 199 y 2000 a 2699.
- `{**deny** | **permite** | **remark** *text*}`. Deniega o permite el acceso si la condición coincide.  **`remark`** permite la entrada de texto para fines de documentación con un límite de 100 caracteres.
- `*protocol*`. Nombre o número de un protocolo de internet. La palabra clave `ip` coincide con todos los protocolos de IP.
- `*source*`. Esto indica la red de origen o la dirección de host que se va a filtrar. Utiliza la palabra clave **`any`** para especificar todas las redes o `[**host**] *****ip-address*` para identificar una IP específica.
- `*souce-wildcard*`. Máscara wildcard opcional para el origen.
- `*destination*`. Esto identifica a la red destino o la dirección de host que se va a filtrar. Utilizar la palabra clave **`any`** para especificar todas las redes. Utilizar `[**host**] *ip-address*`.
- `*destination-wildcard*`. Campo opcional de máscara wildcard para el destino.
- `*operator*`. Campo opcional que compara los puertos de origen y destino. Los operadores posibles incluyen **`lt`** (less than) , **`gt`** (greater than), **`eq`** (equal), **`neq`** (not equal).
- `*port*`. Nombre o número decimal de un puerto TCP o UDP.
- **`established`**. Campo opcional solo para el protocolo TCP que permite que el tráfico entrante de retorne para las conexiones inicializadas dentro de la red, bloqueando a los hosts externos de inicializar una conexión. Es una característica de firewall de primera generación.
- **`log`**. Esta palabra clave genera y envía un mensaje informativo siempre que se haga coincidir el ACE, incluyendo el número de ACL, la condición coincidente (permitido o denegado), dirección de origen y número de paquetes. Se genera para el primer paquete coincidente.

Las ACL extendidas puede filtrarse en diferentes opciones de número de puerto y nombre de puerto. Por ejemplo, una ACL extendida número 100 que filtra tráfico HTTP usará en el primer ACE como nombre de puerto `www` o `80`, logrando ambos lo mismo:

```bash
access-list 100 permit tcp any any eq www
access-list 100 permit tcp any any eq 80
```

La configuración de número de puerto es necesaria cuando no se tiene un nombre de protocolo específico, como SSH (22) o HTTPS (443):

```bash
access-list 100 permit tcp any any eq 22
access-list 100 permit tcp any any eq 443
```

Las ACL extendidas se puede aplicar en varias ubicaciones, pero normalmente cerca del origen. Por ejemplo, una ACL que permite el tráfico HTTP y HTTPS de la red 192.168.10.0 para cualquier destino:

```html
access-list 110 permit tcp 192.168.10.0 0.0.0.255 any eq www
access-list 110 permit tcp 192.168.10.0 0.0.0.255 any eq 443
interface g0/0/0
ip access-group 110 in
exit
```

### Creación de ACL Extendidas Nombradas

Para crear una, utilice el comando del modo de configuración global `ip access-list extended`.

El indicador cambiará a modo de configuración de ACL extendida nombrada y permitirá el ingreso de ACE.

```html
**ip access-list extended** *access-list-name*
```

# NAT IPv4

## Características de NAT

NAT proporciona traducción de direcciones privadas internas a una organización a públicas con el fin de conservar las direcciones IPv4 públicas.

### Funcionamiento de NAT

Cuando un dispositivo envía tráfico saliente, el router cambia la IP privada a una pública y le asigna un puerto único. Esta información es anotada en la tabla de traducción. Cuando el tráfico regresa, el router revisa la tabla para identificar a qué dispositivo y puerto interno corresponde el paquete y se lo entrega.

La terminología NAT siempre se aplica desde la perspectiva del dispositivo con la dirección traducida. NAT incluye cuatro tipos de direcciones.

- **Dirección interna.** Dirección del dispositivo NAT que está traduciendo.
- **Dirección externa.** Dirección del dispositivo de destino.
- **Dirección local.** Cualquier dirección que aparece en la parte interna de una red.
- **Dirección global.** Cualquier dirección que aparece en la parte externa de la red.

#### Dirección Local Interna

Dirección de la fuente vista desde dentro de la red. Normalmente es una IPv4 privada.

#### Direcciones Globales Internas

Dirección de origen vista desde la red externa.

#### Dirección Global Externa

Dirección destino vista desde la red externa.

#### Dirección Local Externa

Dirección destino vista desde la red interna. Si bien es poco frecuenta, esta dirección podría ser diferente de la dirección globalmente enrutable del destino.

## Tipos de NAT

### NAT Estática

NAT estática utiliza una asignación uno a uno de direcciones locales y globales configuradas por el administrador de red, las cuales permanecen constantes. Útil para servidores web o dispositivos que deben tener una dirección coherente a la que se pueda acceder desde internet, como el servidor web de una empresa, así como dispositivos que solo puedan ser accedidos por personal autorizado y no por el público general.

Esta requiere que hayan suficientes direcciones públicas disponibles para satisfacer el número total de sesiones de usuarios simultáneas.

!image.png

### NAT Dinámica

Utiliza un conjunto de direcciones públicas y las asigna según el orden de llegada. Cuando un dispositivo interno solicita acceso a una red externa, la NAT dinámica asigna una IPv4 y las otras seguirán disponibles para uso.

Esta también requiere suficientes direcciones públicas disponibles para satisfacer el número total de sesiones de usuario simultáneas.

!image.png

### Port Address Translation (PAT)

PAT, también conocida como NAT con sobrecarga, asigna varias direcciones IPv4 privadas a una única IPv4 pública o a algunas direcciones.

Cuando el router NAT con PAT recibe un paquete del cliente, utiliza el número de puerto origen para identificar de forma exclusiva la traducción NAT específica. PAT garantiza que los dispositivos usen un número de puerto TCP diferente para cada sesión con un servidor en internet.

!image.png

PAT intenta conservar el puerto de origen inicial. Si este ya está en uso, PAT asigna el primer número de puerto disponible a partir del comienzo del grupo de puertos apropiado `0-511`, `512-1023` o `1024-65535`.

Cuando no hay más puertos disponibles y hay más de una dirección externa en el conjunto de direcciones, PAT avanza a la siguiente dirección para intentar asignar el puerto de origen inicial. El proceso continúa hasta que no hayan más puertos disponibles o direcciones IPv4 externas en el grupo de direcciones.

Algunos paquetes no contienen un número de puerto capa 4 (no son segmentos de capa 4), como ICMPv4. PAT maneja cada uno de estos tipos de protocolos de manera diferente. Por ejemplo, los paquetes ICMPv4 incluyen una ID de consulta que se usa para identificar una solicitud con su respectiva respuesta. PAT les asigna identificadores únicos para poder mapear las respuestas.

## Ventajas y Desventajas de NAT

NAT proporciona muchos beneficios, entre ellos la conservación del esquema de direccionamiento legalmente registrado al permitir la privatización de las intranets, conserva las direcciones mediante multiplexación de aplicaciones en el nivel de puerto, aumenta la flexibilidad de las conexiones a la red pública, proporciona coherencia a los esquemas de direccionamiento de red interna, permite manejar el esquema de direcciones IPv4 privadas existente a la vez que facilita el cambio a un nuevo esquema de direccionamiento público y oculta las direcciones IPv4 de los usuarios y otros dispositivos.

Pero NAT también tiene inconvenientes como que aumenta los retrasos de envío, se pierde el direccionamiento de extremo a extremo, se pierde trazabilidad de extremo a extremo, complica el uso de protocolos de túnel como IPSec y los servicios que requieren que se inicie una conexión TCP desde la red externa, o protocolos sin estado como los servicios que utiliza UDP, puede interrumpirse.

## NAT Estática

La NAT estática es una asignación uno a uno entre una dirección interna y una externa. Esta permite que los dispositivos externos inicien conexiones a los dispositivos internos mediante la dirección pública asignada de forma estática.

!image.png

### Configuración de NAT Estática

Existen dos pasos básicos para lograr la configuración:

- **Paso 1.** Crear una asignación entre las direcciones locales internas y globales internas con el comando **`ip nat inside source static** *local-ip* global*-ip*`.
- **Paso 2.** Se configuran las interfaces que participan en la traducción con internas y externas con respecto a NAT con los comandos **`ip nat inside`** o **`ip nat outside`**.

```html
R2(config)#ip nat inside source static 192.168.10.254 209.165.201.5
R2(config)#interface serial 0/1/0
R2(config-if)#ip address 192.168.1.2 255.255.255.252
R2(config-if)#ip nat inside
R2(config-if)#exit
R2(config)#interface serial 0/1/1
R2(config-if)#ip address 209.165.200.1 255.255.255.252
R2(config-if)#ip nat outside
```

### Verificar NAT Estática

Para verificar la operación NAT, emitir el comando del modo EXEC privilegiado `show ip nat translation`. Este comando muestra las traducciones NAT activas, y para las estáticas, aunque no hayan comunicaciones activas con estas, siempre se mostrarán. Si se emite el comando durante una sesión activa, la salida indica también la dirección del dispositivo externo.

Otro comando útil es `show ip nat stats`. Este comando muestra información sobre el número total de traducciones activas, los parámetros de configuración de NAT, el número de direcciones en el grupo y el número de direcciones que se han asignado.

Para verificar que la traducción NAT está funcionando, lo mejor es borrar las estadísticas de las traducciones anteriores con el comando `clear ip nat statistics` antes de realizar la prueba.

## NAT Dinámica

NAT dinámico asigna automáticamente direcciones locales internas a direcciones globales internas no permanentes. Esta utiliza un conjunto de direcciones globales internas que están disponibles para cualquier dispositivo de la red interna por orden de llegada. Si todas están en uso, el dispositivo debe esperar una dirección disponible antes de poder acceder a la red externa.

!image.png

### Configurar NAT Dinámico

- **Paso 1.** Definir el conjunto de direcciones que se utilizará para la traducción con el comando **`ip nat pool`**. Las direcciones se definen indicando la primera y última dirección IPv4 del conjunto. Las palabras clave **`netmask`** o **`prefix-length`** indican qué bits de la dirección pertenecen a la red y cuáles al host en el rango de direcciones.

```html
**ip nat pool** *name start-ip end-ip* {**netmask** *netmask* | **prefix-length** *prefix-length*}
```

- **Paso 2.** Configurar una ACL estándar para identificar (permitir) solo aquellas direcciones que se deben traducir. Una ACL demasiado permisiva puede generar resultados impredecibles.

```html
**access-list** *access-list-number* **permit** *source* [*wildcard*]
```

- **Paso 3.** Conectar la ACL al conjunto. Se utiliza el comando **`ip nat inside source list** *access-list-number* **pool** *name*` para vincular la ACL al conjunto. El router utiliza **`pool`** para identificar qué dirección recibe cada dispositivo de **`list`**.

```html
**ip nat inside source list** *access-list-number* **pool** *name*
```

- **Paso 4.** Identificar las interfaces internas con respecto a NAT (interfaces que se conectan a la red interna).

```html
**interface** *type number*
**ip nat inside**
```

- **Paso 5.** Identifique las interfaces externas con respecto a NAT (interfaces conectadas a la red externa).

```html
**interface** *type number*
**ip nat outside**
```

### Ejemplo de Configuración

```html
ip nat pool NAT-POOL1 209.165.200.226 209.156.200.240 netmask 255.255.255.224
access-list 1 permit 192.168.0.0 0.0.255.255
ip nat inside source list 1 pool NAT-POOL1
interface serial 0/1/0
ip nat inside
interface serial 0/1/1
ip nat outside
```

### Verificar NAT Dinámico

La salida del comando `show ip nat translation` muestra todas las traducciones estáticas que se han configurado y cualquier traducción dinámica que haya sido creada por el tráfico. Si se agrega la palabra clave `verbose`, se muestra información adicional acerca de cada traducción, incluido el tiempo transcurrido desde que se creó y utilizó la entrada.

```html
show ip nat translation verbose
```

De forma predeterminada, las entradas de traducción expiran después de 24 horas, a menos que se configuren los temporizadores con **`ip nat translation timeout** *timeout-seconds*` en el modo de configuración global. Para borrar entradas dinámicas antes de que se exceda e tiempo de espera, utilice el comando `clear ip nat translation` del modo EXEC privilegiado.

!image.png

## PAT

PAT o NAT con sobrecarga permite el uso de una dirección global interna para múltiples direcciones locales internas. Cuando se configura, el router mantiene suficiente información sobre los protocolos de capa superior, como los puertos TCP o UDP, para hacer las traducciones. Los números de puertos TCP o UDP de cada host interno distinguen entre las direcciones locales. La cantidad de direcciones internas teóricas que se pueden asignar a una IPv4 son 65536, pero en la realidad, llega hasta aproximadamente 4000 direcciones.

### Configurar PAT para Usar un Grupo de Direcciones

Si el ISP emitió más de una dirección IPv4 pública para un sitio, estas pueden ser parte de un conjunto utilizado por PAT. Esto es similar a NAT dinámica, pero utiliza la palabra clave **`overload`** para habilitar PAT.

- **Paso 1.** Definir el conjunto de direcciones globales que se deben usar para la traducción de sobrecarga.

```html
**ip nat pool** *name start-ip end-ip* {**netmask** *netmask* | **prefix-length** *prefix-length*}
```

- **Paso 2.** Definir una lista de acceso estándar que permita las direcciones que se deben traducir.

```html
**access-list** *access-list-number* **permit** *source* [*wildcard*]
```

- **Paso 3.** Especificar la lista de acceso y el conjunto para establecer la traducción de sobrecarga.

```html
**ip nat inside source list** *access-list-number* **pool** *name* **overload**
```

- **Paso 4.** Identificar las interfaces internas.

```html
**interface** *type number*
**ip nat inside**
```

- **Paso 5.** Identificar la interfaz externa.

```html
**interface** *type number*
**ip nat outside**
```

### Configurar PAT para Usar una Única Dirección IPv4

- **Paso 1.** Definir una lista de acceso estándar que permita las direcciones que se pueden traducir.

```html
**access-list** *access-list-number* **permit** *source* [*wildcard*]
```

- **Paso 2.** Especificar las opciones de ACL, interfaz de salida y sobrecarga para establecer la traducción dinámica de origen. Para estos casos, la IP que de traducción será la que esté establecida en la interfaz de salida.

```html
**ip nat inside source list** *access-list-number* **interface** *type number* **overload** 
```

- **Paso 3.** Identificar las interfaces internas.

```html
**interface** *type number*
**ip nat inside**
```

- **Paso 4.** Identificar la interfaz externa.

```html
**interface** *type number*
**ip nat outside**
```

La configuración es similar a la NAT dinámica, pero utiliza la palabra clave `interface` para identificar el puerto del cual se utilizará la IPv4 externa.

### Verificación de PAT

Se utilizan los mismos comandos que para NAT estático y dinámico. `show ip nat translations` muestra las traducciones de dos hosts distintos a servidores web distintos.

## NAT64

IPv6 se desarrolló para que no sea necesario utilizar NAT, sin embargo, IPv6 sí incluye su propio espacio de direcciones privadas IPv6, las ULA (Unique Local Address), en el rango `fc00::/7` que se asemejan a las direcciones IPv4 privadas. Estas están destinadas únicamente a las comunicaciones locales dentro de un segmento.

IPv6 también proporciona traducción de protocolos entre IPv4 e IPv6, conocida como NAT64. Estas son diferentes a las NAT IPv4, pues solo se utilizan para proporcionar acceso transparente entre redes de solo IPv6 a IPv4 y viceversa. Es un mecanismo de ayuda para la transición a IPv6.

# Tecnologías de Firewall

## Redes Seguras con Firewalls

### Firewalls

Un firewall es un sistema o grupo de sistemas que impone una política de control de acceso entre redes.

!image.png

#### Firewall para Filtrado de Paquetes (Sin Estado)

Suelen formar parte de un firewall de router, que autoriza o rechaza el tráfico a partir de la información de las capas 3 y 4. Son firewalls sin estad que utilizan una simple búsqueda en la tabla de políticas que filtra el tráfico según criterios específicos.

!image.png

#### Firewall con Detección de Estado

Son más versátiles y las tecnologías más comúnmente usadas. Estos proporcionan un filtrado de paquetes utilizando la información de conexión que se mantiene en una tabla de estados. El filtrado con estado es una arquitectura de firewall que se clasifica en la capa de red. También analiza el tráfico en las capas 4 y 5 de OSI.

!image.png

#### Firewall de Gateway de Aplicaciones

Un firewall de gateway de aplicación, llamado también firewall proxy, filtra información en las capas 3, 4, 5 y 7 del modelo de referencia OSI. La mayor parte del control y filtrado del firewall se realiza en el software.

Cuando un cliente necesita tener acceso a un servidor remoto, se conecta a un servidor proxy. El servidor proxy se conecta al servidor remoto en nombre del cliente. Por lo tanto, el servidor solamente ve una conexión desde el servidor proxy.

#### Firewall de Próxima Generación

Los NGFW van más allá que los con estado, ofreciendo IPS integrada, control y reconocimiento de aplicaciones para ver y bloquear aplicaciones riesgosas, rutas de actualización para incluir futuros datos de información, técnicas para afrontar amenazas de seguridad en constante evolución.

#### Otros Tipos

Los firewalls basados en host (servidor y personal) consisten en una computadora o servidor que ejecuta software de firewall.

Los firewalls transparentes filtran tráfico Ip entre un par de interfaces conectadas con puente.

Un firewall híbrido es una combinación de distintos tipos de firewalls. Por ejemplo, un firewall de inspección de aplicación es una combinación entre un firewall con estado y un firewall de gateway de aplicación.

## Firewalls en el Diseño de Redes

### Arquitecturas de Seguridad Comunes

El diseño de firewall tiene por objetivo principal permitir o denegar el tráfico según el origen, destino y tipo de tráfico.

#### Privado y Público

La red pública no es de confianza y la interna sí. El tráfico proveniente de la red privada se autoriza e inspecciona mientras viaja hacia la red pública. También se autoriza el tráfico inspeccionado que regresa de la red pública y está relacionado con el tráfico que se originó en la red privada. Generalmente se bloquea el tráfico procedente de la red pública que viaja hacia la red privada.

#### Zona Desmilitarizada (DMZ)

Diseño de firewall donde normalmente hay una interfaz conectada a la red privada, una a la pública y una a la interfaz DMZ. El tráfico de la red privada se inspecciona mientras viaja hacia la red pública o la DMZ. También se permite el tráfico inspeccionado que regresa a la red privada desde la DMZ o la red pública.

Con frecuencia, se bloquea el tráfico procedente de la DMZ que viaja hacia la red privada. El tráfico procedente de la DMZ y que viaja hacia la red pública se permite, siempre y cuando cumpla con los requisitos de servicio. El tráfico procedente de la red pública que viaja hacia la DMZ se permite de manera selectiva y se inspecciona. Se bloquea el tráfico de la red pública que viaja hacia la red privada.

#### Firewall de Políticas Basados en Zonas (ZPF)

Los ZPF emplean el concepto de zonas para brindar más flexibilidad. Una zona es un grupo de al menos una interfaz con funciones o características similares. Las zonas ayudan a especificar dónde se debe implementar una regla o política de firewall Cisco IOS.

De forma predeterminada, el tráfico entre interfaces de la misma zona no está sujeto a ninguna política y pasa libremente. Todo el tráfico de zona a zona está bloqueado. Configurar una política que permita o inspeccione el tráfico para permitir el tráfico entre zonas. La única excepción a esta negación predeterminada de cualquier política es la zona propia del router.

# Firewalls de Políticas Basadas en Zonas

## Descripción General de ZPF

ZPF es un modelo de configuración en el que las interfaces se asignan a zonas de seguridad, y la política de firewall se aplica al tráfico que se mueve entre estas. ZPF ofrece un enfoque estructurado, útil para la documentación y comunicación. La facilidad de uso hace que las implementaciones de seguridad de la red sean más accesibles para una comunidad más grande de profesionales de seguridad.

Si se añade una interfaz adicional a la zona privada, los hosts conectados a la nueva interfaz en la zona privada pueden pasar tráfico a todos los hosts de la misma interfaz existente en la misma zona.

!image.png

Las ventajas que ofrece ZPF es que no depende de las ACL, su postura de seguridad es bloquear, las políticas son fáciles de leer y solucionar problemas con la política de clasificación común de Cisco Language (C3PL, Cisco Common Classification Policy Language). C3PL es un método estructurado para crear políticas de tráfico basadas en eventos, condiciones y acciones, proporcionando escalabilidad porque una política afecta cualquier tráfico dado, en lugar de necesitar varias ACL y acciones de inspección para diferentes tipos de tráfico. Las interfaces virtuales y físicas pueden agruparse en zonas. Las políticas se aplican al tráfico unidireccional entre zonas. Al decidir si implementar IOS Classic Firewall o un ZPF, es importante tener en cuenta que ambos modelos de configuración pueden habilitarse simultáneamente en un router.

### Diseño de ZPF

#### Paso 1: Determinar la Zona

El administrador se centra en la separación de la red en zonas. Las zonas establecen las fronteras de seguridad de la red. Una zona define una frontera donde el tráfico se somete a las restricciones de las políticas al pasar a otra región de la red.

#### Paso 2: Establecer Políticas Entre Zonas

Para cada par de zonas de origen-destino, definir las sesiones que los clientes de las zonas origen pueden solicitar a los servidores de las zonas destino. Para el tráfico que no se basa en el concepto de sesiones, el administrador debe definir flujos de tráfico unidireccionales desde el origen hasta el destino y viceversa.

#### Paso 3: Diseñar la Infraestructura Física

Después de identificar las zonas y documentar los requisitos de tráfico entre ellas, el administrador debe diseñar la infraestructura física. Se debe considerar los requisitos de seguridad y disponibilidad al diseñar la infraestructura física. Esto incluye dictar la cantidad de dispositivos entre las zonas más seguras y las menos seguras y determinar los dispositivos redundantes.

#### Paso 4: Identificar Subconjuntos Dentro de las Zonas y Unificar los Requisitos de Tráfico

Para cada dispositivo de firewall en el diseño, el administrador debe identificar los subconjuntos de zonas que están conectados a sus interfaces y combinar los requisitos de tráfico para esas zonas.

## Operación de ZPF

### Acciones ZPF

Las políticas identifican las acciones que ZPF realizará en el tráfico de red. Se pueden configurar tres acciones para procesar tráfico por protocolo, zonas de origen y destion y otros criterios:

- `inspect`. Realiza inspección de paquetes con estado de Cisco IOS.
- `drop`. Análogo a `deny` en una ACL. La opción `log` está disponible para registrar los paquetes rechazados.
- `pass`. Análogo a `permit` en una ACL. No realiza seguimiento de estado de las conexiones o sesiones dentro del tráfico.

### Reglas para el Tráfico en Tránsito

- Si ninguna de las interfaces es miembro de alguna zona, la acción resultante es pasar el tráfico.
- Si ambas interfaces son miembros de la misma zona, la acción resultante es pasar el tráfico.
- Si una interfaz es miembro de una zona, pero la otra no, la acción resultante es eliminar el tráfico, independientemente de si existe un par de zonas.
- Si ambas interfaces pertenecen al mismo par de zonas y existe una política, la acción resultante es inspeccionar, permitir o descartar según lo definido por la política.

!image.png

### Reglas para el Tráfico a la Zona Propia

La zona propia es el propio router e incluye todas las direcciones IP asignadas a las interfaces del mismo. Este es el tráfico que se origina en el router o se dirige a una interfaz de router. Comprende el tráfico para administración de dispositivos (SSH) o para control de reenvío de tráfico, como el tráfico del protocolo de enrutamiento. Las reglas para un ZPF son diferentes para la zona propia.

!image.png

## Configurar un ZPF

- **Paso 1.** Cree las zonas.
- **Paso 2.** Identifique el tráfico con un `class-map`.
- **Paso 3.** Defina una acción con `policy-map`.
- **Paso 4.** Identifique un par de zonas y relaciónelo con `policy-map`.
- **Paso 5.** Asigne zonas a las interfaces correspondientes.

!image.png

Para habilitar la seguridad, hay que usar el comando:

```html
license boot module c1900 technology-package securityk9
```

Para llegar al comando teniendo diferentes dispositivos, usar la ayuda contextual con el comando `license boot`.

#### Paso 1. Crear las Zonas

Debemos considerar las interfaces a incluir, el nombre de cada zona y el tráfico necesario entre las zonas y su dirección. Cree las zonas con el comando del modo de configuración global:

```html
**zone security** *zone-name*
```

```html
R1(config)#zone security PRIVATE
R1(config-sec-zone)#exit
R1(config)#zone security PUBLIC
R1(config-sec-zone)#exit
```

#### Paso 2. Identificar Tráfico

Se utiliza un `class-map` para identificar el tráfico al que se aplicará una política. Una clase es una forma de identificar un conjunto de paquetes según su contenido mediante condiciones de coincidencia. Por lo general, se define una clase para poder aplicar una acción al tráfico identificado que refleja una política. Se definen con `class-map`.

Para configurar una ZPF, se utiliza la palabra clave `inspect` para definir el mapa de clase. Para determinar cómo se evaluarán los paquetes cuando hay varios criterios de coincidencia, se usa `match-any` o `match-all`.

```html
**class-map type inspect** [**match-any** | **match-all**] *class-map-name*
```

- **`match-any`.** Los paquetes deben cumplir con uno de los criterios de coincidencia para ser considerados miembros de la clase.
- **`match-all`.** Los paquetes deben cumplir con todos los criterios de coincidencia para ser considerados miembros de la clase.
- `*class-map-name*`**.** Nombre que se usará para configurar la política para la clase con `policy-map`.

Se ingresará al modo de subconfiguración de `class-map`. Se puede hacer coincidir el tráfico con una ACL, protocolo específico u otro mapa de clase:

```html
**match access-group** {*acl-number* | *acl-name*}
**match protocol** *protocol-name*
**math class-map** *class-map-name*
```

- **`match access-group`.** Configura los criterios de coincidencia para un mapa de clase en función del número o nombre de ACL.
- **`match protocol`.** Configura los criterios de coincidencia para un mapa de clase en función del protocolo especificado.
- **`match class-map`.** Utiliza otro mapa de clases para identificar el tráfico.

```html
R1(config)#class-map type inspect match-any HTTP-TRAFFIC
R1(config-cmap)#match protocol http
R1(config-cmap)#match protocol https
R1(config-cmap)#match protocol dns
```

#### Paso 3. Definir una Acción

Lo siguiente es utilizar un `policy-map` para definir qué medidas tomar para el tráfico miembro de una clase. Se define el mapa de políticas en el modo de configuración global. Una acción es una funcionalidad específica. Por lo general, se asocia con una clase de tráfico. Por ejemplo, `inspect`, `drop` y `pass` son acciones. Dentro del modo de subconfiguración del mapa de políticas, se asocia la política con la clase de tráfico, entrando a otro modo de subconfiguración en el cual se definen las acciones para esta clase de tráfico.

```html
**policy-map type inspect** *policy-map-name*
**class type inspect** *class-map-name*
{**inspect** | **drop** | **pass**}
```

- **`inspect`.** Control de tráfico basado en estado. El router mantiene la información de sesión para TCP y UDP y permite el tráfico de retorno.
- **`drop`.** Descarta el tráfico no deseado.
- **`pass`.** Una acción sin estado que permite que el router reenvíe el tráfico de una zona a otra.

```html
R1(config)#policy-map type inspection PRIV-TO-PUB-POLICY
R1(config-pmap)#class type inspect HTTP-TRAFFIC
R1(config-pmap-c)#inspect
```

#### Paso 4. Identificar un Par de Zonas y Hacerlo Coincidir con una Política

Se identifica un `zone-pair` y se asocia ese par de zonas a un `policy-map`. Se crea el par de zonas con el comando `zone-pair security` y se usa `service-policy type inspect` para adjuntar un mapa de políticas y su acción asociada al par de zonas.

```html
**zone-pair security** *zone-pair-name* **source** {*source-zone-name* | **self**} **destination** {*destination-zone-name* | **self**}
**service-policy type inspect** *policy-map-name*
```

- **`source** *source-zone-name*`**.** Especifica el nombre de la zona desde la que se origina el tráfico.
- **`destination** *destination-zone-name*`**.** Especifica el nombre de la zona a la que se destina el tráfico.
- **`self`.** Especifica la zona definida por el sistema. Indica si el tráfico irá hacia o desde el propio router.

```html
R1(config)#zone-pair security PRIV-PUB source PRIVATE destination PUBLIC
R1(config-sec-zone-pair)#service-policy type inspect PRIV-TO-PUB-POLICY
```

#### Paso 5. Asignar Zonas a las Interfaces

La asociación de una zona a una interfaz aplicará inmediatamente la `service-policy` asociada a la zona. Si aún no se configuró una política de servicio para la zona, se descartará todo el tráfico de tránsito. Utilice el comando del modo de configuración de interfaz `zone-member security` para asignar una zona a una interfaz:

```html
**zone-member security** *zone-name*
```

```html
R1(config)#interface GigabitEthernet 0/0
R1(config-if)#zone-member security PRIVATE
R1(config-if)#interface Serial 0/0/0
R1(config-if)#zone-member security PUBLIC
```

### Verificar la Configuración de un ZPF

```html
show run | begin class-map
show policy-map type inspect zone-pair sessions
show class-map type inspection
show zone security
show zone-pair security
show policy-map type inspection
```

# Conceptos de los FHRP

## First Hop Redundancy Protocols

Los FHRP son mecanismos que proporcionan puertas de enlace predeterminadas alternativas en redes conmutadas donde dos o más routers están conectados a las mismas VLAN.

### Redundancia del Router

Una forma de evitar un único punto de falla en el default gateway es implementar un router virtual. Varios routers están configurados para trabajar juntos y presentar la ilusión de un solo router a los hosts en la LAN, compartiendo una IP y MAC.

La IPv4 del router virtual se configura como la default gateway para las estaciones de trabajo de un segmento específico de IPv4. Cuando se envían tramas desde los dispositivos host hacia el default gateway, los hosts utilizan ARP para resolver direcciones MAC y reciben la MAC del router virtual. El router actualmente activo dentro del grupo de routers virtuales puede procesar físicamente las tramas que se envían a la dirección MAC del router virtual.

Los protocolos se utilizan para identificar dos o más routers como los dispositivos responsables de procesar tramas que se envían a la dirección MAC o IP del router virtual. El router físico que envían el tráfico es transparente para los hosts.

Un protocolo de redundancia proporciona el mecanismo para determinar qué router debe cumplir la función activa en el reenvío de tráfico y cuándo el router de reserva debe asumir la función de reenvío. Esta transición también es transparente para los hosts. La capacidad que tiene una red para recuperarse dinámicamente de la falla de un dispositivo que funciona como default gateway se conoce como redundancia de primer salto.

### Pasos para la Conmutación por Error del Router

1. El router de reserva deja de recibir los mensajes de saludo del router de reenvío.
2. El router de reserva asume la función del router de reenvío.
3. Debido a que el nuevo router de reenvío asume tanto la dirección IPv4 como la MAC del router virtual, los dispositivos host no perciben interrupción en el servicio.

### Opciones FHRP

!image.png

## HSRP

Proporcionado por Cisco como una forma de evitar la pérdida de acceso externo a la red si falla el router predeterminado. Es el protocolo FHRP exclusivo de Cisco, diseñado para permitir la conmutación por falla, transparente para los dispositivos IPv4 de primer salto.

En un grupo de interfaces de dispositivo, el dispositivo activo es el que se utiliza para enrutar paquetes, y el de reserva es el que toma control cuando falla el activo o cuando se cumplen ciertas condiciones. El router de suspensión controla el estado operativo del grupo HSRP y asume responsabilidades rápidamente si cae el router activo.

### Prioridad e Intento de Prioridad del HSRP

El rol del router activo y de reserva se determina durante el proceso de elección de HSRP. De forma predeterminada, el router con la IPv4 más alta será el activo, sin embargo, se puede modificar la prioridad HSRP para que el que la tenga más alta sea el activo. De forma predeterminada, la prioridad es 100. Esta se puede configurar con el comando `standby priority` y el rango es de 0 a 255.

El intento de prioridad es la capacidad de un router HSRP de activar el proceso de la nueva elección. Esto se puede lograr con el comando `standby preempt`. Esto permite que nuevos routers en línea con prioridades HSRP más altas asuman el rol de activos.

El intento de prioridad solo permite que un router con prioridad mayor desplace a otro como activo. Si tienen la misma prioridad pero IPv4 más alta, no desplazará al otro router.

### Estados y Temporizadores de HSRP

- **Inicial.** Se entra a este estado después de un cambio de configuración o cuando la una interfaz está disponible por primera vez.
- **Aprender.** En este estado se espera el mensaje de saludo del router activo. Se conoce la IP virtual, pero no se es activo ni de reserva.
- **Escuchar.** El router conoce la IP virtual, pero no es activo ni de reserva, solo escucha los saludos de estos routers.
- **Hablar.** El router envía mensajes de saludo y participa en la elección del router activo y de reserva.
- **En espera.** El router es candidato a volverse el siguiente router activo y envía mensajes de saludo.

Los routers activos y de reserva HSRP envían mensajes a la dirección multicast cada 3 segundos. El router de reserva se volverá activo si no recibe mensaje de saludo del router activo después de 10 segundos. 

### Configuración HSRP

```html
**interface** *type number*
**standby version 2**
**standby** *group-number* **ip** *ip-address*
**standby** *group-number* **priority** *priority*
**standby** *group-number* **preempt**
```

Para verificar la configuración HSRP:

```html
**show standby brief**
```

# Conceptos de VPN e IPsec

## Tecnolgía VPN

### Redes Privadas Virtuales

Las VPN sirven para crear conexiones de red privada end-to-end, transportando información privada a través de una red pública, encriptando los datos.

Las VPN modernas admiten funciones de encriptación, como IPsec y las SSL VPN para proteger el tráfico de red etre sitios.

- **Ahorro de costos.** Reduce costos de conectividad y aumenta el ancho de banda de la conexón remota.
- **Seguridad.** Los protocolos de encriptación y autenticación protegen los datos del acceso no autorizado.
- **Escalabilidad.** Las VPN proporcionan escalabilidad.
- **Compatibilidad.** Son compatibles con conexiones de banda ancha WAN, permitiendo conexiones de alta velocidad.

### VPN de Sitio a Sitio y Acceso Remoto

En las VPN de sitio a sitio, el tráfico solo se cifra entre las gateway. Los hosts internos no tienen conocimiento de que se está utilizando una VPN.

Una VPN de acceso-remoto se crea dinámicamente para establecer una conexión segura entre un cliente y dispositivo.

- **VPN empresarial.** Creadas y administradas por la empresa con VPN IPsec y SSL.
- **VPN de proveedores de servicios.** Creadas y administradas por la red del proveedor. Se utiliza el switching de etiquetas multiprotocolo (MPLS) en las capas 2 o 3 para crear canales seguros, segregando el tráfico del de otros clientes.

## Tipos de VPN

### VPN de Acceso Remoto

Las VPN de acceso remoto permiten a los usuarios remotos y móviles conectarse de forma segura a la empresa. Se habilitan dinámicamente por el usuario ccuando es necesario y se pueden crear utilizando IPsec o SSL.

- **Conexión VPN sin cliente.** La conexión se asegura utilizando SSL del navegador web.
- **Conexión VPN basada en el cliente.** El software del cliente VPN debe instalarse en el dispositivo final del usuario remoto.

### SSL VPN

Utiliza una infraestructura de llave pública y certificados digitales para autenticar a sus pares. Se basa en los requisitos de acceso de los usuarios y en los procesos de TI de la organización.

!image.png

### IPsec VPN de Sitio a Sitio

Se usa para enviar tráfico a través de una red no confiable. Los hosts finales envía y reciben tráfico sin cifrar mediante la gateway VPN, la cual se encarga de encriptar y encapsular el tráfico, enviándolo a través del tunel VPN. La receptora desencapsula y desencripta los contenidos para enviarlos al dispositivo destino de su red privada.

### GRE sobre IPsec

Generic Routing Encapsulation (GRE) es un protocolo de VPN de sitio a sitio básico y no seguro. Permite encapsular protocolos de capa de red, tráfico multicast y broadcast, pero no proporciona encriptación.

Una VPN IPsec estándar (no GRE) solo crea túneles seguros para tráfico unicast. Se puede encapsular tráfico GRE sobre IPsec, permitiendo enviar las actualizaciones del protocolo de enrutamiento.

- **Protocolo del Pasajero.** Paquete original que será encapsulado por GRE.
- **Protocolo del Operador.** GRE, el cual encapsula el paquete de pasajero.
- **Protocolo de Transporte.** Protocolo para reenviar el paquete (IPv4 o IPv6).

!image.png

### Dynamic Multipoint VPN (DMVPN)

DMVPN es una solución de Cisco para conecntar múltiples VPN a una ubicación centralizada, con topología tipo hub-and-spoke. Estos sitios establecen una conexión segura con el sitio central y se configuran usando Multipoint Generic routing Encapsulation (mGRE). Esta permite que una única interfaz GRE admita dinámicamente múltiples túneles IPsec.

### IPsec Virtual Tunnel Interface (VTI)

Se puede configurar entre sitios o en una topología hub-and-spoke y simplifica el proceso de configuración para admitir múltiples sitios remotos. IPsec VTI estalbece una interfaz virtual que funciona como una interfaz física, permitiendo enviar tráfico unicast y multicast sin configurar GRE, además de que permite firewalls.

!image.png

### Multiprotocol Label Switching (MPLS) VPN del ISP

El tráfico se envía a través de la red troncal mediante etiquetas, y los clientes no pueden ver el tráfico de los demás. MPLS proporciona soluciones VPN administradas, por lo tanto, es responsabilidad del ISP asegurar el tráfico entre los sitios del cliente.

### Configuración de Túnel VPN

- **Paso 1.** Se crea el túnel y se le asigna un número en el modo de configuración global.

```bash
**interface tunnel** *number*
```

- **Paso 2.** Se asigna una dirección IP en modo de subconfiguración de interfaz.

```html
**ip address** *ipv4 subnet-mask*
```

- **Paso 3.** Asignamos la interfaz de origen, en la cual se hará el encapsulado VPN para enviarla por el túnel. Esta interfaz será la de salida del dispositivo a internet (debe haber una salida de red por defecto) y será el destino del otro extremo del túnel.

```html
**tunnel source** *interface*
```

- **Paso 4.** Se asigna la IP destino, la cual será el otro extremo del túnel.

```html
**tunnel destination** *ipv4*
```

- **Paso 5.** De manera opcional, si se desea mandar información de enrutamiento entre las redes, usar el modo GRE.

```html
**tunnel mode gre ip**
```

## IPsec

### Tecnologías IPsec

Es un estándar IETF (RFC 2401-2412) que define cómo asegurar una VPN a través de redes IP, protegiendo y autenticando los paquetes IP.

- **Confidencialidad.** IPsec utiliza algoritmos de encriptación.
- **Integridad.** IPsec utiliza algoritmos de hash.
- **Autenticación de origen.** IPsec utiliza IKE (Internet Key Exchange) para autenticar origen y destino.
- **Diffie-Hellman.** Permite asegurar el intercambio de claves.

!image.png

### Protocolo de Encapsulación IPsec

- **Authentication Header (AH).** Apropiado cuando la confidencialidad no es requerida o permitida.
- **Encapsulation Security Payload (ESP).** Proporciona confidencialidad y autenticación.

### Confidencialidad

El grado de confidencialidad depende de la longitud de la clave y el algoritmo de encriptación.

- DES utiliza una clave de 56 bits.
- 3DES utiliza tres claves de cifrado independientes de 56 bits por bloque de 64 bits.
- AES ofrece llaves de 128, 192 y 256 bits.
- SEAL es un cifrado de flujo (encripta datos de forma continua y no por bloques) con llave de 160 bits.

### Integridad

La integridad de datos significa que los datos no cambiaron en tránsito. Hash-based Message Authentication Code (HMAC) es un algoritmo que garantiza tango la integridad como la autenticación.

- MD5 (Message-Digest 5) utiliza una llave secreta compartida de 128 bits.
- SHA (Secure Hash Algorithm) utiliza una llave secreta de 160 bits.

### Autenticación

- **Pre-Shared Key (PSK).** Algoritmo de clave simétrica, es fácil de configurar, pero debe configurarse en cada par, por lo que no es fácil de escalar.
- **Rivest, Shamir y Adleman (RSA).** Utiliza certificados digitales para autenticar los pares. Cada par debe autenticar a su par opuesto antes de considerar seguro el túnel.

### Intercambio Seguro de Llaves con Diffie-Hellman

DH permite que dos pares puedan establecer una clave secreta compartida a través de un canal inseguro.

- DH 1, 2 y 5 ya no deben usarse.
- DH 14, 15 y 16 usan tamaños de clave más grandes, con 2048, 3072 y 4096 bits respectivamente.
- DH 19, 20, 21 y 24 usan tamaños de clave de 256, 384, 521 y 2048 bits. Admiten la criptografía de curva elíptica, que reduce el tiempo necesario para generar las claves.

# Seguridad en la Nube

## Virtualización y Computación en la Nube

### Máquinas Virtuales

Un hipervisor es un programa de software o hardware que permite ejecutar varios sistemas operativos independientes en un solo sistema físico.

- **Hipervisor de tipo 1 (virtualización de hardware).** El sistema operativo invitado se ejecuta directamente en una plataforma de hardware, bajo el control del sistema host.
- **Hipervisor de tipo 2 (virtualización en host).** Un programa que se ejecuta en la máquina del sistema host permite crear máquinas virtuales enteramente de software.

Los entornos de máquinas virtuales pueden utilizar un sistema operativo. Las máquinas virtuales comparten hardware y se ejecutan con privilegios elevados.

### Contenedores

Un contenedor consiste de solo la aplicación y sus dependencias. Docker utiliza la virtualización a nivel del sistema operativo. Kubernetes permite administrar contenedores.

### Virtuali Desktop Infrastructure (VDI)

Los entornos de escritorio de usuario se guardan de forma remota en un servidor de alta disponibilidad al que pueden conectarse a través de la red.

### Tecnología Basada en Nube

Permiten trasladar el componente tecnológico a un proveedor externo.

- **Software as a Service (SaaS).** Los proveedores administran la infraestructura mientras que los usuarios almacenan datos (bases de datos, aplicaciones).
- **Plataform as a Service (PaaS).** Permite acceder de forma remota a herramientas de desarrollo para desplegar aplicaciones sin preocuparse de la infraestructura física subyacente.
- **Infrastructure as a Service (IaaS).** El proveedor aloja el hardware, software, servidores y componentes de almacenamiento, mientras que el usuario los usa a demanda, eliminando la necesidad de comprar sus propios equipos.

### Computación en la Nube

#### Nube Privada

También llamada nube interna, corporativa o empresarial, está alojada en una plataforma privada. Ofrece más control sobre tus datos, pero suele ser más costosa.

#### Nube Pública

Alojada por un proveedor de servicios en una instalación externa. Alternativa más económica, pero con menor control sobre datos.

#### Nube Híbrida

Combina la nube privada y pública, ofreciendo el control de los datos de la organización, que siguen alojados en la nube pública.

#### Nube Comunitaria

Esfuerzo de colaboración en el que más de una organización comparte y utiliza la misma plataforma.

### Fog Computing (Computación en la Niebla) y Edge Computing (Computación en el Borde)

La computación en el borde analiza los datos donde se producen. Es ideal para respuestas en milisegundos y destaca por su privacidad.

La computación en la niebla actúa como puente, recibiendo datos de dispositivos de borde y procesando para mandar solo la información crítica a la nube central.

### Principales Amenazas para la Computación en la Nube

!image.png

!image.png

## Los Dominios de Seguridad en la Nube

### Dominios de Seguridad en la Nube

Cloud Security Alliance (CSA), promueve las mejores prácticas para garantizar la seguridad dentro de los dominios de computación en la nube en el documento Security Guidance for Critical Areas of Focus in Cloud Computing v4, cubriendo 14 dominios.

!image.png

!image.png

!image.png

## Seguridad de la Infraestructura en la Nube

### Seguridad de la Infraestructura

La infraestructura en la nube es la base sobre la cual se construyen e implementan los recursos de la nube virtualizados, tales como cómputo, redes y almacenamiento de datos. Esta compuesto por cómputo físico y lógico, e infraestructura virtual.

Debido a la naturaleza de la nube, las medidas de seguridad deben evaluarse antes de implementarse, pues pueden producir cuellos de botella.

### Responsabilidades de Seguridad en la Nube

La seguridad en la nube funciona en un modelo de responsabilidad compartida. Un cliente y proveedor de servicio (CSP) es responsable de los servicios para el cliente, pero este es responsable de todo lo demás. Este reparto varía según el tipo de servicio prestado.

!image.png

### Consideraciones de Seguridad de la Infraestructura en la Nube

#### Políticas de Seguridad de la Compañía

Las aplicaciones no supervisadas por la organización pueden crear brechas de seguridad y puntos ciegos. La forma más eficaz de gestionar las apliciones desconocidas es mediante políticas de seguridad bien definidas.

#### Seguridad en Capas

Se pueden aplicar estrategias de defensa en capas a las cuatro capas (hardware, infraestructura, plataforma, aplicación) de los recursos de nube.

#### Microsegmentación

También denominada hipersegregación, aprovecha las topologías de red individuales para ejecutar redes múltiples, más pequeñas y aisladas. Permiten un control más granular del tráfico y los flujos de trabajo dentro de la nube.

## Seguridad de Aplicaciones en la Nube

### Desarrollo de Aplicaciones

#### Desarrollo y Prueba

El software se crea en un entorno donde se puede depurar y probar antes de implementar. Este entorno de desarrollo es menos restrictivo que el real. Los desarrolladores también pueden trabajar en un entorno aislado (sandbox) para que el código no se sobrescriba mientras lo desarrollan.

#### Aprovisionamiento y Desaprovisionamiento

Creación o actualización de software y su eliminación. Se puede utilizar un portal de autoservicio para automatizar estos procesos.

#### Ensayo y Producción

Los entornos de ensayo deben coincidir con los entornos de producción. Aquí se debe verificar que el software se ejecuta en la configuración de seguridad requerida para poder desplegar el programa.

### Ténicas de Codificación Seguras

La normalización covierte cualquier entrada a su forma más simple para garantizar la integridad y unicidad de los datos. Se utiliza para organizar los datos de una base de datos.

Los procedimientos almacenados son instrucciones precompiladas en SQL almacenadas en una base de datos que ejecutan tareas. Permite reducir el tráfico de red.

Se utiliza la ofuscación y camuflaje para evitar que el software sea objeto de ingeniería inversa. La ofuscación oculta los datos originales con aleatorios. El camuflaje sustituye datos sensibles por ficticios realistas.

Se reutiliza código para ahorrar tiempo y costos, pero siempre debe verificarse para evitar la introducción de vulnerabilidades.

Las third-party libraries y Software Development Kit (SDK) proporcionan un repositorio de código para desarrollo más rápido, pero podrían introducir fallos en la cadena de suministros.

### Validación de Entrada

Es importante validar la introducción de datos para mantener la integridad de la base de datos. Muchos atacantes ingresan datos con formato incorrecto.

Una regla de validación verifica que los datos respondan a los parámetros definidos por el diseñador de la base de datos.

Una comprobación de integridad es realizada por una función hash. Una suma de comprobación (checksum) es un ejemplo de función de hash.

Las checksum convieten cada pieza de información en un valor y suman el total. Se realiza el proceso a la salida y la llegada y se compara.

## Seguridad de los Datos en la Nube

### Estados de los Datos

- **Datos en reposo.** Datos almacenados. Están en este estado cuando ningún usuario o proceso accede, los solicita y los modifica. Todos los datos que no están en tránsito ni en proceso se consideran datos en reposo.
- **Datos en tránsito.** Se refiere a cualquier dato que se esté transmitiendo. Es fundamental el uso de criptografía y hashing para portegerlos.
- **Datos en proceso.** Datos durante la entrada inicial, modificación, cálculo o salida.

### Criptografía

- **Criptografía.** Es la ciencia de crear y descifrar códigos secretos. Los datos cifrados solo pueden ser leídos por aquel con el conocimiento adecuado del algoritmo.
- **Ecriptación.** Es el proceso de cifrar los datos. Codifican el texto plano a texto cifrado o ciphertext.
- **Algoritmos de cifrado simétrico.** Utilizan la misma clave precompartida para cifrar y descifrar datos, método también conocido como cifrado de clave privada.
- **Algoritmos de cifrado asimétrico.** Utilizan una clave para el cifrado que es diferente de la utilizada para el descrifrado. Incluyen Rivest-Shamir-Adleman (RSA), Diffie-Hellman, ElGamal y Criptografía de Curva Elíptica (ECC).

### Hashing

Los hashes garantizan la integridad de los datos tomando cadenas binarias y produciend una representación de longitud fija llamada valor hash.

Las funciones de hash son funciones unidireccionales utilizadas para verificar y garantizar la integridad. También pueden verificar autenticación.
