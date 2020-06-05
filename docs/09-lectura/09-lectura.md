##### Autor:** Luis Enrique Valenzuela Navarro
##### *Fecha de creación:* 04-06-2020
##### *Revisiones:*  Nombre Apellido, Nombre Apellido
##### *Fecha de revisión:* 20-20-2020
# The Defense against ARP Spoofing
## Resumen

Una de las amenazas mas importantes contra la data de los usuarios en Internet son los ataques realizados a la capa de enlace de datos. Entre los mas importantes esta la suplantación de identidad. Estos ataques son posibles gracias a vulnerabilidades en el protocolo ARP. Una solución básica es agregar de manera estática las direcciones MAC e IP a la Tabla de cache ARP. Sin embargo debido a lo laborioso que podría llegar a ser, muchos administradores no están dispuestos a realizarlo. En este paper se propuso un método para mejorar la solución antes propuesta.  Para ello se propone agregar una función de validación ARP para administrar la tabla de cache ARP de manera automática. El método es simple y compatible con el protocolo ARP estándar. No requiere modificaciones en el protocolo. Se realizaron pruebas exitosas del método con el fin de proteger a los anfitriones contra los ataques de suplantación de ARP.

## INTRODUCCIÓN

Internet es una herramienta que utilizamos diariamente. Se estima que el 54,2% de la población humana utiliza internet. Estos usuarios utilizan el Internet para comunicarse y compartir data, por lo que asegurar la información se ha vuelto un desafío.  Internet utiliza el modelo de comunicación *"packet switching"*. Este modelo consiste en descomponer la data en pequeños trozos para formar paquetes discretos que se envían por diferentes canales en una secuencia y se reensamblan en un nodo final.  Una de las amenazas principales son los ataques a la capa de enlace de datos. A esto se le conoce como *ARP SPOOFING*. Para mitigar este ataque se propusieron estas 5 soluciones:

1. Modificar ARP por medio de técnicas criptográficas.
2. Parchear el Kernel del Sistema Operativo.
3. Asegurar los puertos del Switch.
4. Utilizar software externo para detección de ataques.
5. Configuración manual de la tabla de cache ARP.

Esta investigación propuso un método para administrar la tabla de cache ARP de manera automática. Esta propuesta es compatible con el protocolo ARP estándar y no requiere modificaciones en el protocolo o el remplazo de dispositivos antiguos. 

## ARP SPOOFING ATTACK

Todos los dispositivos que se conectan a Internet poseen dos tipos de direcciones. La dirección IP es una dirección lógica en el dispositivo que se utiliza para identificar la ubicación de este mientras este conectado a la red. Cambia de manera dinámica cuando un usuario se conecta a Internet en una ubicación distinta.  La dirección MAC es una dirección física que se almacena dentro de la interfaz de la tarjeta de red. En teoría, esta es única e inmutable. 

Cuando se envía un frame en una LAN, el remitente debe conocer la dirección MAC del receptor. El remitente utiliza el protocolo ARP para hacer una traducción de la dirección IP a la dirección MAC del destinatario. El protocolo ARP consta de dos tipos de mensajes. 

1. ARP Request: especifica la dirección IP del de la dirección MAC del destino.
2. ARP Reply:  especifica la dirección MAC asociada con esa IP.

Cuando un frame llega a un gateway, este trata de encontrar la dirección MAC asociada con la IP destino en su propia tabla de cache ARP. Si no la encuentra, realiza un broadcast que contiene la dirección IP del destino a todos los dispositivos de la subred. El dispositivo que reconoce su IP, envía un ARP Reply y el resto de dispositivos ignoran el frame. El gateway actualiza su tabla de cache ARP basado en la respuesta. Un atacante puede manipular fácilmente dicha tabla realizando un ARP SPOOFING.

ARP SPOOFING es un tipo de ataque que falsifica un ARP Request o un ARP Reply. El atacante finge la dirección MAC del gateway para hacer que la victima reenvié los frames destinados para el gateway hacia otro sitio. Con ello podrí conseguir monopolizar el ancho de banda y cortar la comunicación con otros dispositivos. Un ataque de MiTM utiliza ARP SPOOFING para escuchar a escondidas la comunicación entre una victima y su gateway.

Debido a que ARP se trata de un protocolo stateless no correlaciona las peticiones con las respuestas. ARP es capaz de aceptar respuestas incluso si no se realizo ninguna petición. No hay forma de verificar la disponibilidad del emisor en ARP. 

### ARP REQUEST SPOOFING

El atacante envía un ARP Request hacia la victima. Este paquete contiene data falsificada para hacer creer a la victima que el es el gateway o redireccionar peticiones a un dispositivo desconocido.  Por lo que la victima almacenara en su tabla de cache ARP la dirección MAC del atacante. 

Esta técnica es utilizada por diversos ataques. Entre los mas conocidos NetCut y MiTM. NetCut es un ataque que cambia la dirección MAC de su gateway por una falsa. La victima no podrá acceder al Internet porque sus paquetes gamas llegaran al gateway. MiTM es un tipo de ataque que es considerado espionaje. Permite escuchar comunicación entre dos dispositivos y alterar la antes mencionada. Una victima por lo general no se percata de este ataque debido a que no pierde salida a Internet.

### ARP REPLY SPOOFING

Es similar al ARP REQUEST SPOOFING únicamente varia el mensaje ARP que se utiliza. Sin embargo este tipo de ataque es detectable de una manera sencilla por medio de un IDS. Debido a que es inusual recibir una respuesta de ARP sin haber realizado una solicitud.

## SOLUCIÓN

El método propuesto consta de tres procedimientos principales. El primero de ellos es enumerar las direcciones IP y MAC que se encuentren actualmente en la tabla cache de ARP. El segundo de ellos consta en validad estas direcciones y por ultimo establecer las direcciones MAC e IP validas de manera estática en la tabla cache de ARP.

Para iniciar debemos tener una lista blanca vacía. Esta lista contendrá las direcciones IP y MAC confiables. Posterior debemos verificar los registros en la tabla cache de ARP enviando ARP Requests.  Para esto podemos usar ARPING que es una herramienta que realiza este trabajo. Si no se reciben respuestas, se deben eliminar de la tabla. Esto podría darse debido a que el equipo se encuentre inactivo o que ya se encuentre falsificado. Posterior a esto, todas aquellas que si reciban respuesta serán almacenadas de manera estática en la tabla cache de ARP. Si la IP se encontraba ya en lista blanca, el sistema verificara si el registro esta expirado. Si se encontraba expirado, la nueva dirección MAC remplazara la anterior.

Este método no verifica todas las direcciones IP en la subred. Solo chequea las que posee en su tabla cache de ARP para minimizar el broadcast de las peticiones ARP. 

## IMPLEMENTACIÓN

La solución antes mencionada se implemento utilizado el lenguaje de programación Python. No se modifico el protocolo ARP.  Se creo un ambiente de pruebas utilizando VirtualBox.  Dicho ambiente consta de 4 maquinas virtuales:

1. Dos clientes
2. Un Router
3. Un Atacante

Todos ellos fueron virtualizados utilizado un Ubuntu Server 16.04. Este modelo puede implementarse en otros sistemas operativos. Se escogió este sistema operativo por ser liviano al momento de virtualizar. 

Para probar la efectividad del método propuesto, se configuro el método en el router y en uno de los clientes. Luego se lanzo un ARP SPOOFING por medio de Scapy. Scapy es una herramienta que permite manipular paquetes en una LAN.

## RESULTADOS

Como era de esperarse, la identidad de algunos nodos fue falsificada. En el escenario 4 los dos clientes se ven afectados y se corre el programa de mitigación únicamente en el cliente 1. Este programa fue capaz de recuperar la tabla y de no permitir alteraciones en esta en futuros ataques.  Se compararon ambas tablas, la del cliente 1 y el cliente 2, donde se evidencian las diferencias.

El cliente 1 fue capaz de recuperarse y mitigar los ataques en su contra. Mientras que el cliente 2 no es capaz de ello. Con esto se concluyo que el método propuesto es exitoso contra ARP SPOOFING ATTACK.

## CONCLUSIONES

Se probo con efectividad el método propuesto. Este método es capas de proteger a equipos contra ataques de suplantación de identidad incluyendo MiTM. No se realizo ninguna modificación del protocolo ARP estándar. El método es fácil de implementar en los dispositivos. Este método no puede alertar a otros equipos que no tengan implementado el método. Se debería implementar comunicación entre los equipos para evitar la falsificación de ARP dentro de la red.

## REFERENCIAS

[1] Statista, “Global digital population as of July 2018 (in millions),” The Statistics Portal, 2018. [Online]. Available: https://www.statista.com/statistics/617136/digital-populationworldwide/. [Accessed: 30-Jul-2018]. 

[2] D. Srinath, S. P. S.Panimalar, A. J. Simla, and J. D. J.Deepa, “Detection and Prevention of ARP spoofing using Centralized Server,” Int. J. Comput. Appl., vol. 113, no. 19, pp. 26–30, Mar. 2015. 

[3] J. Singh and V. Grewal, “A Survey of Different Strategies to Pacify ARP Poisoning Attacks in Wireless Networks,” Int. J. Comput. Appl., vol. 116, no. 11, pp. 25–28, 2015. 

[4] M. A. Carnut and J. J. C. Gondim, “ARP spoofing detection on switched Ethernet networks,” in the 5th Simpósio Segurança em Informática, 2003. 

[5] R. K. Bijral, A. Gupta, and L. Sen Sharma, “Study of Vulnerabilities of ARP Spoofing and its detection using SNORT,” Int. J. Adv. Res. Comput. Sci., vol. 8, no. 5, pp. 2074–2077, 2017. 

[6] T. Kiravuo, M. Sarela, and J. Manner, “A Survey of Ethernet LAN Security,” IEEE Commun. Surv. Tutorials, vol. 15, no. 3, pp. 1477– 1491, 2013. 

[7] M. Al-Hemairy, S. Amin, and Z. Trabelsi, “Towards more sophisticated ARP Spoofing detection/prevention systems in LAN networks,” in 2009 International Conference on the Current Trends in Information Technology (CTIT), 2009, pp. 1–6. 

[8] S. Shukla and I. Yadav, “An innovative method for detection and prevention against ARP spoofing in MANET,” Int. J. Comput. Sci. Inf. Technol. Secur., vol. 5, no. 1, pp. 207–214, 2015. 

[9] M. Conti, N. Dragoni, and V. Lesyk, “A Survey of Man In The Middle Attacks,” IEEE Commun. Surv. Tutorials, vol. 18, no. 3, pp. 2027– 2051, 2016. 

[10] Sudhakar and R. K. Aggarwal, “A survey on comparative analysis of tools for the detection of ARP poisoning,” in 2017 2nd International Conference on Telecommunication and Networks (TEL-NET), 2017, pp. 1–6. 

[11] L. Allen, T. Heriyanto, and S. Ali, Kali Linux – Assuring Security by Penetration Testing. Packt Publishing, 2014
