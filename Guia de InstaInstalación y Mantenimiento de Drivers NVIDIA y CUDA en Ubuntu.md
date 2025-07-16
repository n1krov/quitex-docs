# Verificación de NVIDIA

> Guardar/anotar por si alguna actualización la sobreescribe y aparte copiar carpetas y archivos para backup.

## 1. Versión del Driver de NVIDIA:
- Ejecutar uno de los siguientes comandos:
  ```bash
  nvidia-smi
  ```
  o
  ```bash
  sudo lshw -C display
  ```
  o
  ```bash
  lspci | grep -i nvidia
  ```
  
## 2. Drivers compatibles:
- Verificar drivers compatibles con:
  ```bash
  ubuntu-drivers devices
  ```
  o
  ```bash
  sudo apt update
  apt search nvidia-driver
  ```

## 3. Localización de la Carpeta de CUDA:
- Listar la carpeta de CUDA:
  ```bash
  ls /usr/local/cuda
  ```

## 4. Variables de Entorno:
- Mostrar las variables de entorno de CUDA:
  ```bash
  echo $CUDA_HOME
  echo $CUDA_PATH
  ```

## 5. Reinstalar la misma versión de CUDA:
- Desinstalar la versión actual de CUDA:
  ```bash
  sudo /usr/local/cuda/bin/uninstall_cuda_XX.Y.pl
  sudo ./cuda_XX.Y_linux.run --toolkit --silent
  ```
  *Sustituye `XX.Y` con la versión exacta de CUDA que tienes instalada.*

## 6. Reparación de Paquetes en Ubuntu:
- Configurar paquetes:
  ```bash
  sudo dpkg --configure -a
  ```

## 7. Reparar Dependencias de Paquetes:
- Instalar dependencias faltantes:
  ```bash
  sudo apt-get install -f
  ```

## 8. Limpiar Paquetes Obsoletos o Dañados:
- Limpiar paquetes:
  ```bash
  sudo apt-get autoremove
  sudo apt-get autoclean
  ```

## 9. Reinstalar un Paquete Específico:
- Reinstalar un paquete:
  ```bash
  sudo apt-get install --reinstall nombre-del-paquete
  ```

---

# Instalación de NVIDIA

## 1. Instalar dependencias:
```bash
sudo apt-get install build-essential gcc-multilib dkms
sudo apt-get install libglew-dev libcheese7 libcheese-gtk23 libclutter-gst-2.0-0 libcogl15 libclutter-gtk-1.0-0 libclutter-1.0-0
```

## 2. Crear archivo de configuración para blacklist:
- Crear un archivo en `/etc/modprobe.d/blacklist-nouveau.conf` con el siguiente contenido:
  ```plaintext
  blacklist nouveau
  options nouveau modeset=0
  ```

## 3. Salir de modo gráfico:
- Usar las combinaciones de teclas:
  ```plaintext
  Ctrl + Alt + F3
  Ctrl + Alt + F7  # Volver a xorg
  ```

## 4. Detectar e instalar el driver de NVIDIA:
- Detectar drivers:
  ```bash
  ubuntu-drivers devices
  ```
- Añadir el repositorio de drivers gráficos:
  ```bash
  sudo add-apt-repository ppa:graphics-drivers
  ```
- Instalar el driver:
  ```bash
  sudo apt-get install nvidia-390
  ```
  *Ej. Nvidia-390 es la última versión del controlador para la serie GTX 1xxx. Buscarlo aquí: [NVIDIA Drivers](https://www.nvidia.com/en-us/drivers/unix/legacy-gpu/)*

- O instalar automáticamente:
  ```bash
  sudo ubuntu-drivers autoinstall
  ```
- O instalar manualmente desde el sitio de NVIDIA:
  - Descargar el controlador:
    ```bash
    ls
    NVIDIA-Linux-x86_64-410.73.bin
    ```
  - Verificar instalación:
    ```bash
    nvidia-smi
    ```

---

# Instalación de CUDA desde el sitio oficial

- Buscar la versión para tu sistema [aquí](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal):
  ```bash
  wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
  chmod +x cuda_10.2.89_440.33.01_linux.run
  sudo sh cuda_10.2.89_440.33.01_linux.run
  ```

- Añadir configuración a `ld.so.conf.d`:
  ```bash
  sudo bash -c "echo /usr/local/cuda/lib64/ > /etc/ld.so.conf.d/cuda.conf"
  sudo ldconfig
  ```

- Exportar variables de entorno:
  ```bash
  export PATH=/usr/local/cuda-7.5/bin:$PATH
  export LD_LIBRARY_PATH=/usr/local/cuda-7.5/lib64:$LD_LIBRARY_PATH
  ```

---

# Selección de GPU

- Iniciar con la placa integrada:
  ```bash
  sudo prime-select intel
  ```
- Iniciar con la placa NVIDIA:
  ```bash
  sudo prime-select nvidia
  ```

- Reparar una versión específica:
  ```bash
  sudo apt --fix-broken install nvidia-dkms-470 --fix-missing
  ```

- Eliminar drivers de la lista negra y reinstalar `nouveau`:
  ```bash
  sudo apt-get install nouveau-firmware
  sudo dpkg-reconfigure xserver-xorg
  ```

---

# Reinstalación de Componentes del Sistema

- Reinstalar headers del kernel:
  ```bash
  sudo apt install --reinstall linux-headers-$(uname -r)
  ```
- Reinstalar paquetes esenciales:
  ```bash
  sudo apt install --reinstall build-essential
  sudo apt install --reinstall dkms
  sudo apt install --reinstall linux-generic
  sudo apt install --reinstall linux-signed-generic
  ```

---

# Reparación y Configuración

- Verificar la instalación de CUDA:
  ```bash
  nvcc --version
  echo 'export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}' >> ~/.bashrc
  ```

- Reparar paquetes:
  ```bash
  sudo apt --fix-broken install
  ```

- Exportar variables de entorno:
  ```bash
  export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64\${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
  ```

- Solución de problemas con `pmemd`:
  ```bash
  sudo ln -s /usr/local/cuda-10.0 /usr/local/cuda
  ```

- Verificar la localización de `nvidia`:
  ```bash
  whereis nvidia
  ```

- Desinstalar drivers de NVIDIA:
  ```bash
  sudo apt-get purge nvidia*
  sudo purge nvidia*
  ```

---

# Solución de Fallas

## 1. Verificar la detección de la GPU:
```bash
lspci | grep -i nvidia
```

## 2. Revisar el estado del módulo del kernel de NVIDIA:
```bash
lsmod | grep nvidia
sudo modprobe nvidia
```

## 3. Verificar errores en el arranque del sistema:
```bash
dmesg | grep NVRM
```

## 4. Archivo `.bashrc`:
```bash
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
source ~/.bashrc
```

## 5. Verificación de la Configuración de CUDA:
```bash
nvcc -V
```

## 6. Revisar conflictos de versión o bibliotecas:
```bash
ldconfig -p | grep cuda
```


---
