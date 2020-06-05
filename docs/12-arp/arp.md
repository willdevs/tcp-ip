---
layout: default
title: ARP - Protocolo de resolución de direcciones
nav_order: 22
permalink: /arp
---
##### **Autores:** Rafael Mazariegos, Fernando López
{: .no_toc }
[ Marvin Chigüil](https://github.com/mrosendo782)

##### **Fecha de creación:** 05-06-2020
{: .no_toc }

##### **Revisiones:**  
{: .no_toc }

##### **Fecha de revisión:** 
{: .no_toc }

# ARP - Protocolo de resolución de direcciones
{: .no_toc }

#### Contenido:
{: .no_toc }

1. TOC
{:toc}

---


## Resumen
ARP (Address Resolution Protocolo) es un protocolo de comunicación que se encuentra en Link Layer (capa de datos). de una forma muy resumida este protocolo se encarga de asociar una dirección MAC que corresponde a una dirección IP. Se envía desde un dispositivo un "ARP request" hacia la dirección de broadcast (ff:ff:ff:ff:ff:ff), aquí se encuentra la dirección IP por la que se pregunta, la máquina asociada a esta IP realiza un "ARP reply". Cada máquina tiene un caché con las direcciones traducidas, esta tabla se tiene las direcciones MAC que se utilizan con más frecuencia. Este protocolo se puede utilizar cuando hay dos hosts en una misma red y uno quiere enviar paquetes hacia el otro, si hay dos hosts en diferentes redes se utiliza un router para llegar al otro. Un router puede necesitar enviar un paquete a un host a través de otro router o puede enviarlo desde la misma red.

## ARP
### Propósito de ARP
Cuando un host en una red va a enviar un paquete IP a cierto host con cierta dirección IP dentro de la red local,
primero necesita empaquetarlo dentro de una trama de link layer. Sin embargo, el host sólo conoce la IP del
dispositivo al que desea enviar el paquete, y no su **dirección física** (MAC). Por esta razón necesita una 
forma de obtener la MAC a partir de la IP del host de destino.
### Funcionamiento de ARP


## Ejemplo de entrega directa
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object.

## Ejemplo de entrega a través de un router
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object.

## Caché ARP
El caché ARP es una tabla que almacena las direcciones IP que corresponden a direcciones físicas de los dispositivos a los que ha enviado información recientemente y sirve para saber a que dirección MAC enviar la información para que esta llegue al destino deseado sin tener que enviar un ARP request cada vez que desea enviar información a otro dispositivo saturando la red. Estas pueden ser dinámicas (que se hacen automáticamente) o estáticas (que se agregan manualmente).

## Formato del frame ARP
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object.

## ARP Gratuito
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object.


## Preguntas

### Pregunta 1
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object.

### Pregunta 2
Create model object and add values to it and `save()` the model. After saving model **model id** and 
**model key** is attached with model object.

