---
title: Soltando la mano con la aws cli, laboratorio 01
layout: post
date: 2024-08-05
categories: aws

---

En mayo de este 2024 decidí invertir el tiempo necesario para entender más en detalle el funcionamiento de todo lo que es el ecosistema de AWS. A los pocos días por arte de magia del algoritmo, me apareció una promoción por un curso de Arquitecto de Soluciones en AWS, impartido por una institución llamada SmarData de Perú.
Busqué reviews en internet, me gustó lo que encontré y pagué los 214USD, una semana después estaba iniciando este gran curso con el profesor Jose Zamalloa y aprox. otros 60 compañeros que se unian a este curso a través de Zoom.

Además de los laboratorio que imparte el profesor, también me ha gustado desarrollar mis propias actividades guiadas por mi curiosidad y la ayuda de algunas herramientas de IA como Claude.ai y Chat GPT.

En este primer laboratorio voy a compartir los pasos necesarios para crear una instancia EC2 desde la CLI y finalmente conectarse a ella via ssh.


# Creando Access Key en la Consola AWS
1.- Selecciona el usuario desde el panel de usuarios de IAM
2.- Clickea en **Security credentials** 
3.- Baja hasta encontrar la opción **Create Access Key**
4.- Copia ambas cadenas, el ID y el secret key en un archivo de texto
5.- Desde tu terminal, escribe **aws configure**
6.- Pega el access key ID y secret key del paso 4
7.- Escribe tu zona y json para finalizar la configuración

Ahora con tu cuenta configurada para aws-cli comenzaremos a preparar todo lo necesario para iniciar una instancia EC2 desde la CLI.

# Obteniendo datos necesarios para ejecutar instrucciones
1) Arrancamos con el comando **aws ec2 describe-vpcs**.
El campo "VpcId" no indicara este valor
En mi caso este valor corresponde a "vpc-0662e736559f90bda".

2) Ahora hacemos algo similar para obtener el SubnetId, corremos **aws describe-subnets** y copiamos el valor de "SubnetId", en mi caso:subnet-0cbffd6431141df9a

VPC/Subnet IDs: 
vpc-0662e736559f90bda
subnet-0cbffd6431141df9a

3) Ahora vamos por la AMI ID, esto lo obtenemos en la AMI Catalog desde la AWS Console:
En mi caso: ami-03972092c42e8c0ca


# Creating a Security group

1) Como vamos a necesitar seleccionar un secutiry group para nuestra neuva instancia, debemos usar este comando(reemplaza los valores de group-name, tag-specifications y vpc-id con el que obtuviste anteriormente):
aws ec2 create-security-group \
    --group-name secGroup-awscli \
    --description "AWS ec2 CLI Demo SG" \
    --tag-specifications 'ResourceType=security-group,Tags=[{Key=Name,Value=secGroup-awscli}]' \
    --vpc-id "vpc-0662e736559f90bda"

2) Copia el GroupId del retorno del comando anterior:
sg-03b2d8248465661a5

3) Agrega inbound (ingress) firewall rules al security group que creamos, aca reemplaza el group-id del punto anterior.

aws ec2 authorize-security-group-ingress \
    --group-id "sg-03b2d8248465661a5" \
    --protocol tcp \
    --port 22 \
    --cidr "0.0.0.0/0" 

aws ec2 authorize-security-group-ingress \
    --group-id "sg-03b2d8248465661a5" \
    --protocol tcp \
    --port 80 \
    --cidr "0.0.0.0/0" 

# Creando ssh key pair

1) Corre este comando para crear una key-pair para la futura instancia.

aws ec2 create-key-pair --key-name demo-key --query 'KeyMaterial' --output text > ~/.ssh/demo-key

# En resumen  
Tenemos los siguientes valores que utilizaremos en el paso final:

VPC ID: vpc-0662e736559f90bda
Subnet ID: subnet-0cbffd6431141df9a
AMI ID: ami-03972092c42e8c0ca
Security Group ID: sg-03b2d8248465661a5
Key name: demo-key

 
***
Creando la Instancia
***
aws ec2 run-instances \
    --image-id ami-03972092c42e8c0ca \
    --count 1 \
    --instance-type t2.micro \
    --key-name demo-key \
    --security-group-ids sg-03b2d8248465661a5 \
    --subnet-id subnet-0cbffd6431141df9a \
    --associate-public-ip-address \
    --block-device-mappings "[{\"DeviceName\":\"/dev/sdf\",\"Ebs\":{\"VolumeSize\":30,\"DeleteOnTermination\":false}}]" \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=servidor-con-ip}]' 'ResourceType=volume,Tags=[{Key=Name,Value=server-ip-disk}]'



