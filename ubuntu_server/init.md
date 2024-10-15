# Instalación y Configuración de un Servidor Ubuntu 22.04 LTS para Cura, Amber y Acceso Remoto

## 1. **Instalación del Sistema Operativo**
### Requisitos previos:
- Descarga la imagen **Ubuntu 22.04 LTS** (64-bit) desde [aquí](https://releases.ubuntu.com/22.04/).
- Un USB booteable preparado (puedes usar herramientas como **Rufus** o **Etcher**).

### Pasos:
1. **Arranca desde el USB**: Inserta el USB booteable y arranca la máquina desde él.
2. **Instalación de Ubuntu Server**: Sigue el asistente de instalación. Durante el proceso:
   - Elige el idioma, la ubicación y la zona horaria.
   - Configura las particiones del disco (puedes optar por "Instalar automáticamente usando LVM").
   - Configura el usuario administrador y el hostname del servidor.
   - No instales servicios adicionales aún (lo haremos manualmente).
3. **Reinicia y Retira el USB**: Una vez que termine la instalación, retira el USB y reinicia el servidor.

## 2. **Primeros pasos tras la instalación**
### Actualización del sistema:
Antes de hacer cualquier cosa, asegúrate de que tu sistema está actualizado.

```bash
sudo apt update
sudo apt upgrade -y
```

### Instalar herramientas esenciales:
Instala algunos paquetes que necesitarás para trabajar en el servidor, como **SSH** y herramientas básicas.

```bash
sudo apt install openssh-server build-essential curl wget git -y
```

## 3. **Configuración del acceso remoto mediante SSH**
Verifica que **SSH** esté activo y habilitado para iniciarse en cada reinicio:

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

Si necesitas que otros usuarios se conecten por **SSH**, deben conocer la dirección IP del servidor. Para obtenerla, ejecuta:

```bash
ip a
```

Pide a los usuarios conectarse a través de:
```bash
ssh usuario@direccion-ip
```

### Seguridad adicional (opcional):
Es recomendable desactivar el acceso SSH como root y cambiar el puerto por defecto (22) para mayor seguridad.

Edita el archivo de configuración de SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

Haz los siguientes cambios:
- Desactiva el acceso SSH para el usuario root: 
  ```
  PermitRootLogin no
  ```
- Cambia el puerto del servicio SSH (ejemplo: 2222):
  ```
  Port 2222
  ```
  
Reinicia SSH para aplicar los cambios:
```bash
sudo systemctl restart ssh
```

## 4. **Instalación de los drivers NVIDIA**
### Paso 1: Agregar los repositorios de NVIDIA
Asegúrate de tener el repositorio de controladores NVIDIA configurado en el sistema.

```bash
sudo apt install ubuntu-drivers-common
```

### Paso 2: Instalar los drivers
Instala el controlador recomendado para tu tarjeta NVIDIA:

```bash
sudo ubuntu-drivers autoinstall
```

Si necesitas instalar una versión específica del controlador, puedes hacerlo con:
```bash
sudo apt install nvidia-driver-XXX  # Sustituir XXX por la versión
```

### Paso 3: Verificar la instalación
Reinicia el servidor para cargar el nuevo driver y verifica que está correctamente instalado con:

```bash
nvidia-smi
```

## 5. **Instalación de Cura**
### Paso 1: Descargar el paquete de Cura
Visita el [sitio web de Ultimaker Cura](https://ultimaker.com/software/ultimaker-cura) para descargar la versión más reciente para Linux (archivo `.AppImage`).

### Paso 2: Configurar Cura
Una vez descargado el archivo, hazlo ejecutable:

```bash
chmod +x Ultimaker_Cura-<version>.AppImage
```

### Paso 3: Ejecutar Cura
Puedes iniciar Cura con el siguiente comando:

```bash
./Ultimaker_Cura-<version>.AppImage
```

## 6. **Instalación de Amber**
### Paso 1: Dependencias
Amber requiere varias dependencias que debemos instalar primero:

```bash
sudo apt install build-essential gfortran flex bison cmake -y
```

### Paso 2: Descargar Amber
Regístrate y descarga la versión de Amber desde el [sitio oficial de Amber](https://ambermd.org/). Asegúrate de descargar la versión correcta y descomprimir el archivo.

### Paso 3: Compilación de Amber
Accede al directorio de Amber y sigue las instrucciones del manual para compilar:

```bash
./configure gnu
make install
```

### Paso 4: Verificar la instalación
Prueba la instalación con algunos ejemplos incluidos en el paquete Amber para asegurarte de que todo funciona correctamente.

## 7. **Configuración para acceso gráfico remoto (Opcional)**
Si los usuarios necesitan interactuar con la interfaz gráfica de **Cura** o **Amber**, puedes configurar acceso remoto gráfico con **X2Go** o **NoMachine**.

### X2Go:
1. Instala el servidor de X2Go en el servidor:

```bash
sudo apt install x2goserver x2goserver-xsession
```

2. En las máquinas cliente, descarga e instala **X2Go Client** desde [aquí](https://wiki.x2go.org/doku.php/download:start).

3. Conéctate al servidor con el cliente **X2Go** especificando la dirección IP y las credenciales de usuario.

### NoMachine:
1. Descarga e instala **NoMachine** en el servidor desde [aquí](https://www.nomachine.com/download).
2. Instala el cliente en las máquinas que se conectarán remotamente.

## 8. **Mantenimiento y monitoreo**
### Monitoreo de hardware (NVIDIA)
Para monitorear la GPU en tiempo real:

```bash
watch -n 1 nvidia-smi
```

### Actualizaciones
Es importante mantener el servidor actualizado para recibir parches de seguridad y actualizaciones de los controladores:

```bash
sudo apt update && sudo apt upgrade -y
```
---


Para configurar un servidor con Ubuntu 22.04 LTS para que el usuario **root** tenga acceso directo (y habilitar el inicio de sesión como **root**), debes seguir algunos pasos. Normalmente, en Ubuntu, el usuario **root** está deshabilitado por defecto por razones de seguridad, y en su lugar se utiliza el comando `sudo` para ejecutar tareas administrativas.

A continuación, te muestro cómo habilitar el acceso del usuario **root** y configurarlo para que pueda iniciar sesión en el servidor, ya sea localmente o por **SSH**.

---

### 1. **Establecer contraseña para el usuario root**
El usuario **root** tiene su contraseña deshabilitada por defecto. Para habilitar el acceso como root, primero necesitas asignarle una contraseña.

Ejecuta el siguiente comando para establecer la contraseña del usuario **root**:

```bash
sudo passwd root
```

Te pedirá que ingreses y confirmes la nueva contraseña para el usuario **root**.

### 2. **Habilitar el inicio de sesión como root por SSH**
Por razones de seguridad, el acceso SSH como root está deshabilitado por defecto en Ubuntu. Si necesitas habilitarlo, sigue estos pasos:

#### Paso 1: Edita el archivo de configuración de SSH

```bash
sudo nano /etc/ssh/sshd_config
```

#### Paso 2: Habilita el acceso de root

Dentro de este archivo, busca la línea siguiente (probablemente estará comentada):

```bash
#PermitRootLogin prohibit-password
```

Cambia esa línea a:

```bash
PermitRootLogin yes
```

Si prefieres una opción más segura, como permitir solo la autenticación con claves SSH para root, puedes usar:

```bash
PermitRootLogin prohibit-password
```

Esto permitirá que el usuario root se conecte solo con claves SSH (y no con contraseñas).

#### Paso 3: (Opcional) Cambiar el puerto SSH
Si no lo has hecho antes, puedes cambiar el puerto SSH para mejorar la seguridad, por ejemplo, a `2222` en lugar del puerto predeterminado `22`:

```bash
Port 2222
```

#### Paso 4: Reiniciar el servicio SSH

Después de hacer los cambios, guarda el archivo (`Ctrl + O`, luego `Enter` y `Ctrl + X` para salir) y reinicia el servicio **SSH** para aplicar los cambios:

```bash
sudo systemctl restart ssh
```

### 3. **Acceso remoto como root**
Ahora que el acceso de root está habilitado, puedes conectarte remotamente usando **SSH** como el usuario root.

Si cambiaste el puerto SSH a `2222`, asegúrate de conectarte especificando el puerto:

```bash
ssh root@direccion-ip-del-servidor -p 2222
```

Si mantuviste el puerto predeterminado (22), el comando sería:

```bash
ssh root@direccion-ip-del-servidor
```

### 4. **Consideraciones de Seguridad**
Habilitar el acceso de **root** directamente en el servidor conlleva ciertos riesgos de seguridad. Algunas recomendaciones adicionales:

- **Utiliza autenticación con claves SSH** en lugar de contraseñas para el usuario root.
  
  Genera una clave SSH en la máquina desde la que vas a conectarte (si no tienes una ya):

  ```bash
  ssh-keygen -t rsa -b 4096
  ```

  Copia la clave pública a tu servidor:

  ```bash
  ssh-copy-id root@direccion-ip-del-servidor
  ```

- **Configura el firewall** con **UFW** para permitir solo el puerto SSH y bloquear el resto, si aún no lo has hecho:

  ```bash
  sudo ufw allow 2222/tcp  # O el puerto que uses para SSH
  sudo ufw enable
  ```

- **Deshabilita el acceso root por SSH cuando no sea necesario**. Puedes usar el acceso **sudo** para tareas administrativas y habilitar el acceso root solo temporalmente si es estrictamente necesario.

---



