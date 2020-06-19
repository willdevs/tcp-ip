---
layout: default
title: TCP - Control de Congestión
nav_order: 36
permalink: /control-congestion-tcp
---
##### **Autores:** Willson Omar Trujillo 
{: .no_toc }


##### **Fecha de creación:** 19/06/2020
{: .no_toc }

##### **Revisiones:** 
{: .no_toc }

##### **Fecha de revisión:** 
{: .no_toc }

# TÍTULO 

# control-de-congestion-en-tcp

{: .no_toc } 

#### Contenido: **Slow-start** es un algoritmo de control de congestión del protocolo [TCP](https://es.wikipedia.org/wiki/Transmission_Control_Protocol).

Ni el emisor ni el receptor tienen forma de saber cual es el máximo volumen de datos que puede transmitir la red, ninguno tiene información sobre los elementos de red que transmitirán la información. Si la red se satura comenzará a descartar paquetes, que tendrán que ser retransmitidos, lo cual puede incrementar aún más la saturación de la red. La solución que plantea este algoritmo, consiste en comenzar enviando un volumen de datos pequeño, que se irá aumentando hasta que la red se sature, en cuyo caso se reducirá la tasa de envío para reducir la saturación.



### Ventana de congestión[[editar](https://es.wikipedia.org/w/index.php?title=Slow-start&action=edit&section=2)]

Es el valor límite de la ventana del emisor. El objetivo de los mecanismos de control de congestión será lograr una buena estimación de ese valor, de manera que en todo momento tenga un valor óptimo. Es decir, sea lo más grande posible (de manera que no sea una merma para la velocidad real a la que se transmite), pero sin llegar a provocar congestiones en la red (que también redundarán en una merma de la velocidad real o efectiva).

El valor de la ventana de emisión, es decir, el número de bytes que el emisor puede llegar a emitir sin esperar a recibir un [ACK](https://es.wikipedia.org/wiki/ACK), será siempre el mínimo entre la ventana de congestión y el valor de ventana indicado por el receptor.

### Umbral de congestión[[editar](https://es.wikipedia.org/w/index.php?title=Slow-start&action=edit&section=3)]

Se establece un valor llamado *umbral de congestión* ("congestion threshold") que trata de ser una estimación del tamaño de la ventana del emisor a partir del cual existe riesgo de congestión. Hasta que la ventana de congestión alcance el valor del umbral, se emplea el algoritmo slow-start para su crecimiento. A partir de haberse alcanzado el valor umbral, se aplica el algoritmo de "evitación de congestión" ([congestion avoidance](https://es.wikipedia.org/wiki/Congestion_avoidance)).

El valor inicial del umbral es el del máximo segmento admitido por TCP: 65.535 bytes. Es actualizado a la mitad del valor de la *ventana de congestión* cuando el transmisor detecta una congestión en la red, pero nunca será inferior a dos segmentos.

### Slow-start[[editar](https://es.wikipedia.org/w/index.php?title=Slow-start&action=edit&section=4)]

Algoritmo para el cálculo de la ventana de congestión aplicado al principio de la conexión, y hasta que se alcanza el umbral de congestión. Consiste en lo siguiente:

- La ventana de congestión se inicia con el valor de un segmento de tamaño máximo (MSS).
- Cada vez que se recibe un [ACK](https://es.wikipedia.org/wiki/ACK), la ventana de congestión se incrementa en tantos bytes como hayan sido reconocidos en el ACK recibido. En la práctica, esto supone que el tamaño de la ventana de congestión será el doble por cada [RTT](https://es.wikipedia.org/wiki/RTT), lo que da lugar a un crecimiento exponencial de la ventana.
- Cuando un ACK no llega al transmisor:
  - Se toma como una señal de congestión en la red y se reinicia la ventana de congestión a un MSS.
  - Se aplica el algoritmo de [congestion avoidance](https://es.wikipedia.org/wiki/Congestion_avoidance).

### Congestion Avoidance[[editar](https://es.wikipedia.org/w/index.php?title=Slow-start&action=edit&section=5)]

Cada vez que se recibe un ACK( la ventana de congestión se incrementa un número de bytes igual al MSS. En la práctica, esto supone que la ventana crece de manera lineal.

![](D:\Universidad Galileo\Postgrado Redes y Computadoras\Segundo Trimestre\SUITE DE PROTOCOLOS TCPIP\git\control1.png)











{: .no_toc }

1. TOC
{:toc}

---


## Resumen
La tabla de contenido se crea automáticamente con los títulos y subtítulos de su página.
Debe agregar los títulos y subtítulos necesarios. Tome como ejemplo las otras páginas del sitio.
(Recuerde eliminar este texto)


## TÍTULO
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object.

## SUBTÍTULO
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object. Esta es una prueba