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

---

## **Primeros pasos tras la instalación**
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

---

## **Configuración del acceso remoto mediante SSH**
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
---

## **Instalación de los drivers NVIDIA**


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

---
