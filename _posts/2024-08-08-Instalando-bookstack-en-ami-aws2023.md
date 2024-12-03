---
title: Instalando-bookstack-en-AMI-AWS2023
layout: post
date: 2024-08-08
categories: AWS-labs
---

En el invierno de 2023, tuve la ardua y muy entretenida tarea de trabajar durante 2 meses en la creación de un manual de operaciones, con documentación del departamento de comunicaciones de mi empleador anterior.
Se trataba de una tarea compleja, llevar la ejecución de nuestros roles de trabajo desde nuestras costumbres de trabajo a palabras e instrucciones en una plataforma consolidada. Esta documentación describiria porque hacemos las cosas como las hacemos y cómo aprender a la brevedad las mejores prácticas del dia a dia para que futuros operadores de comunicaciones pudieran desenvolverse en el cargo después de un período de entrenamiento.

Estudié distintas alternativas para montar una especie de plataforma similar a una wiki y poder agrupar temáticas en capítulos y categorías. Después de ver videos y leer en foros, me decidí por Bookstack, una solución de código abierto escrita en PHP para conexión a base de datos MySQL.

En este laboratorio, vamos a ir directo a la manera más sencilla de montar Bookstack en una instancia EC2, dejándola configurada y lista para recibir contenido.


#Arrancando la instancia desde la Cli:
1) Primero nos conectamos a la instancia, ya se apor ssh utilizando tu access key o directamente utilizando la consola y AWS connect. Para los efectos de este laboratorio estare usando los siguientes valores ya creados en mi consola:
--key-name demo-key 
--security-group-ids sg-03b2d8248465661a5 
--subnet-id subnet-0cbffd6431141df9a 

```
aws ec2 run-instances \
    --image-id ami-03972092c42e8c0ca \
    --count 1 \
    --instance-type t2.micro \
    --key-name demo-key \
    --security-group-ids sg-03b2d8248465661a5 \
    --subnet-id subnet-0cbffd6431141df9a \
    --associate-public-ip-address \
    --block-device-mappings "[{\"DeviceName\":\"/dev/sdf\",\"Ebs\":{\"VolumeSize\":30,\"DeleteOnTermination\":false}}]" \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=bookstack-lab}]' 'ResourceType=volume,Tags=[{Key=Name,Value=bookstack-lab}]'

```
2) Opcional, para asegurarnos de que nuestra instancia ya esta corriendo, podemos ejecutar el comando:
`aws ec2 describe-instances | grep "Value"`

3) Nos logueamos via ssh a nuestra instancia recien salida del horno despues del comando del punto 1. La ip publica asignada por aws en mi caso corresponde a *54.224.27.223*.

`ssh -i demo-keys ec2-user@54.224.27.223`


4) Desde la consola ejecutamos:
`sudo yum install docker` 

5) Despues que se ha instalado todo, podemos chequear la version instalada ejecutando:
```
[ec2-user@ip-10-0-0-167 ~]$ docker version
Client:
 Version:           25.0.5
 API version:       1.44
 Go version:        go1.22.5
 Git commit:        5dc9bcc
 Built:             Mon Jul 29 17:21:34 2024
 OS/Arch:           linux/amd64
 Context:           default
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```
En mi caso, cuento con docker version 25.0.5

6) Descargamos la imagen desde el repositorio de imagenes:

`sudo docker pull ghcr.io/linuxserver/bookstack:v24.05.3-ls157`

7) 