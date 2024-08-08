---
title: Conectandome a maquina Ubuntu (en aws) via RDP
date: 2024-08-07
category: AWS-labs
---

#1.- Lanzando instancia EC2.
1) Nos logueamos a nuestra cuenta AWS con nuestro usuario IAM.
2) Clickeamos en Launch instance desde el panel de nuestra consolo AWS.
3) Creamos los correspondientes security-groups, access-key para utilizar ssh (puerto 22), utilizamos la AMI ubuntu, seleccionamos **enable public ip** y lanzamos la instancia.

#2.- instalacion y configuracion de xfce/xrdp.

1) Desde la terminal:
```

#Actualizamos repositorios
sudo apt update

# Instalar Xfce y xrdp
sudo apt install -y xfce4 xfce4-goodies xorg dbus-x11 x11-xserver-utils
sudo apt install -y xrdp

# Configurar xrdp para usar Xfce
echo xfce4-session > ~/.xsession

# Reiniciar el servicio xrdp
sudo service xrdp restart

# Establecer contrasena, necesario para loguearse despues.
sudo passwd $USER

```

### Pero... que es Xfce y xrdp?

## Xfce

Xfce es un entorno de escritorio ligero para sistemas operativos tipo Unix, como Linux. Algunas de sus características principales son:

Ligero: Consume menos recursos que otros entornos de escritorio más pesados como GNOME o KDE.
Rápido: Debido a su ligereza, es muy rápido incluso en hardware más antiguo o menos potente.
Personalizable: Ofrece muchas opciones de configuración para adaptar la apariencia y el comportamiento.
Intuitivo: Tiene una interfaz de usuario bastante tradicional y fácil de usar.

## xrdp

xrdp es un servidor de Protocolo de Escritorio Remoto (RDP) de código abierto para sistemas operativos Unix/Linux. Características clave:

Compatibilidad: Permite que los sistemas Linux acepten conexiones RDP desde clientes Windows, macOS, o incluso otros sistemas Linux.
Seguridad: Utiliza el mismo protocolo RDP que Windows, que incluye cifrado.
Flexibilidad: Puede trabajar con varios backends de visualización, incluyendo VNC.


# Coneccion con xrdp.

En mi caso, corro sistema operativo Linux Mint version Cinammon. Linux Mint no viene con un cliente RDP preinstalado, así que primero necesitamos instalar uno. Remmina es una excelente opción, ya que es fácil de usar y viene con soporte para RDP.

```
sudo apt update
sudo apt install remmina remmina-plugin-rdp
```

# Configurar una nueva conexión RDP:

- En Remmina, haz clic en el botón "+" para crear una nueva conexión.

- En el campo "Protocolo", seleccione "RDP - Remote Desktop Protocol".

- En "Nombre de servidor", introduzca la dirección IP pública de su instancia AWS Ubuntu.

![Image](/assets/img/public-ip.png)
En mi caso es la direccion 3.84.174.142


- En "Nombre de usuario", introduzca el nombre de usuario de su instancia Ubuntu.

Puede dejar la contraseña en blanco por ahora, se le pedirá cuando se conecte.
En la pestaña "Avanzado", asegurate de que "Modo de color" esté configurado en "Alta calidad (32 bpp)".
Guarde la configuración.

# Iniciando conexion

Haz doble clic en la conexión que acabas de crear.
Se pedirá que aceptes el certificado del servidor. Verifica que la huella digital coincida y clickea en aceptar..
Introducela contraseña de su usuario en la instancia Ubuntu cuando se solicita.

