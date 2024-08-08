---
title: Instalando-bookstack-en-AMI-AWS2023
layout: post
date: 2024-08-08
categories: AWS-labs
---

#Arrancando la instancia desde la Cli:
1) Primero nos coenctamos a la instancia, ya se apor ssh utilizando tu access key o directamente utilizando la consola y AWS connect

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

2) Nos logueamos via ssh a nuestra instancia recien salida del horno despues del comando del punto 1. La ip publica asignada por aws en mi caso corresponde a *54.224.27.223*.

`ssh -i demo-keys ec2-user@54.224.27.223`


3) Descargamos la imagen desde el repositorio de imagenes:

`sudo docker pull ghcr.io/linuxserver/bookstack:v24.05.3-ls157`