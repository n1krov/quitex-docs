
---

# Guía de Instalación

## 1. Instalar Drivers de NVIDIA GPU

### Detectar tu dispositivo
Utiliza el siguiente comando para detectar tu dispositivo y los drivers compatibles:
```bash
ubuntu-drivers devices
```

### Instalar el driver recomendado
Ejecuta el siguiente comando para instalar el driver recomendado:
```bash
sudo apt install nvidia-driver-470
```

#### Posible error y solución
Si obtienes el siguiente error (o similar):
```
desviación de /usr/lib/i386-linux-gnu/libGL.so.1 a /usr/lib/i386-linux-gnu/libGL.so.1.distrib por nvidia-340
dpkg-divert: error: diferencia en el paquete
```
Remueve las instalaciones anteriores con:
```bash
apt remove -y --purge nvidia-* libnvidia-ifr1-*:i386 libnvidia-ifr1-* nvidia-driver-*
dpkg-divert --remove "/usr/lib/i386-linux-gnu/libGL.so.1"
dpkg-divert --remove "/usr/lib/x86_64-linux-gnu/libEGL.so"
dpkg-divert --remove "/usr/lib/x86_64-linux-gnu/libEGL.so.1"
```

Luego, ejecuta:
```bash
sudo apt --fix-broken install
```

## 2. Instalar Drivers de Cuda

Descarga el Cuda Toolkit 9 desde [aquí](https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1704&target_type=deblocal) y sigue las instrucciones de instalación.

### Remover instalaciones previas de Cuda
```bash
dpkg -l | grep cuda- | awk '{print $2}' | xargs -n1 sudo dpkg --purge
```

### Instalar el paquete de Cuda
```bash
dpkg --install cuda-repo-ubuntu*-9.0-local*.deb
sudo apt-get update
sudo apt-get install cuda
```

### Cambiar variables de entorno relacionadas con Cuda en `.bashrc`
```bash
export PATH=/usr/local/cuda-9.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=/usr/local/cuda-9.0
```

## 3. Downgradear Compiladores

Otro problema durante la instalación es que los compiladores `gcc` y `gfortran` en Ubuntu 20.04 son demasiado nuevos para `amber16`:
```
#error -- unsupported GNU version! gcc versions later than 6 are not supported!
```

### Añadir gcc-6, gfortran-6 y g++-6 a la lista de fuentes
Sigue las instrucciones [aquí](https://askubuntu.com/questions/1236552/how-can-i-downgrade-gcc-to-version-6-on-20-04) para añadir las versiones necesarias.

```bash
sudo vi /etc/apt/sources.list
```
Añade la siguiente línea:
```plaintext
deb http://dk.archive.ubuntu.com/ubuntu/ bionic main universe
```
Actualiza e instala:
```bash
sudo apt-get update
sudo apt install gcc-6 g++-6 gfortran-6
```

#### Resolver conflicto con `gcc-6-base`
Si hay conflicto con `gcc-6-base`, remuévelo e intenta de nuevo:
```bash
sudo apt remove gcc-6-base
sudo apt install gcc-6 g++-6 gfortran-6
```

### Crear enlaces simbólicos a los compiladores antiguos
```bash
sudo ln -sfn /usr/bin/gfortran-6 /usr/bin/gfortran
sudo ln -sfn /usr/bin/gcc-6 /usr/bin/gcc
sudo ln -sfn /usr/bin/g++-6 /usr/bin/g++
```

## 4. Instalar Amber

A partir de septiembre de 2021, la última versión de AmberTools es la 21, pero no funciona con `amber16`. Por lo tanto, instalamos `Amber16` junto con `AmberTools17`.

### Desempaquetar Amber16 y AmberTools17 en la misma carpeta
```bash
tar xvvf /tmp/Amber16.tar.bz2
tar xvvf /tmp/AmberTools17.tar.bz2
cd /home/user/amber16
```

### Configurar variable de entorno `http_proxy` (si es necesario)
Para los usuarios que se conectan a Internet a través de un servidor proxy, configura la variable de entorno `http_proxy` en el archivo `.bashrc`:
```bash
vi ~/.bashrc
```
Añade la siguiente línea al final del archivo:
```bash
export http_proxy="10.40.1.252:3128"
```
Esto permitirá las actualizaciones de parches al instalar Amber.

### Instalar Amber
```bash
./configure --with-python “python2.7” gnu
make install
```

### Instalar la versión de Cuda
```bash
./configure -cuda gnu
make install
```

---
