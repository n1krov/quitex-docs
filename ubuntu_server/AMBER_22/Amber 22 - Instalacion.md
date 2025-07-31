
## 🔧 Paso 1: Instalar dependencias

Conectado por SSH como usuario con permisos `sudo`, ejecutá:

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y \
  build-essential \
  cmake \
  flex \
  bison \
  zlib1g-dev \
  libbz2-dev \
  libnetcdf-dev \
  libopenmpi-dev \
  openmpi-bin \
  gfortran \
  tcsh \
  python3 \
  python3-pip \
  git \
  libffi-dev \
  liblzma-dev \
  curl \
  wget
```

---

## 🚀 Paso 2: Instalar CUDA Toolkit

Verificá que tenés CUDA 12.2 instalado. Si no, instalalo desde el repositorio oficial de NVIDIA:

```bash
nvidia-smi
nvcc --version
```

Si no tenés `nvcc`, seguí estos pasos para instalar CUDA 12.2:

```bash
# Descargar instalador
wget https://developer.download.nvidia.com/compute/cuda/12.2.0/local_installers/cuda_12.2.0_535.54.03_linux.run

# Dar permisos e instalar
chmod +x cuda_12.2.0_535.54.03_linux.run
sudo ./cuda_12.2.0_535.54.03_linux.run
```

Durante la instalación, **desmarcá el driver si ya está instalado** (ya que `nvidia-smi` funciona).

Agregá CUDA al PATH:

```bash
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## 📦 Paso 3: Descargar AmberTools24 + Amber24

1. Registrate y descargá desde [https://ambermd.org/GetAmber.php](https://ambermd.org/GetAmber.php)
    
2. Subí los archivos `.tar.bz2` al servidor con `scp` o `rsync` o descargalos directamente con `wget` si tenés el link.
    

Por ejemplo, desde tu máquina local:

```bash
scp AmberTools24.tar.bz2 Amber24.tar.bz2 usuario@IP_SERVIDOR:~
```

---

## 📁 Paso 4: Extraer e instalar

```bash
mkdir -p ~/amber24
cd ~/amber24

tar -xjf ~/AmberTools24.tar.bz2
tar -xjf ~/Amber24.tar.bz2
```

---

## ⚙️ Paso 5: Configurar e instalar

```bash
cd ~/amber24/AmberTools

# Crear carpeta de build
mkdir build
cd build

# Configuración base
cmake ../cmake \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_GUI=FALSE \
  -DMPI=TRUE \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda
```

---

## 🔨 Paso 6: Compilar (esto tarda)

Usamos todos los núcleos disponibles:

```bash
make -j$(nproc)
make install
```

---

## 🔬 Paso 7: Test (opcional)

Esto es para asegurarte de que se compiló bien:

```bash
cd $HOME/amber24
source amber.sh
cd $AMBERHOME/test
make test
```

---

## ✅ Paso 8: Agregar al `.bashrc`

```bash
echo "source \$HOME/amber24/amber.sh" >> ~/.bashrc
source ~/.bashrc
```


---

errores

### 📌 Error actual:

Tu comando:

```bash
cmake ../cmake \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_GUI=FALSE \
  -DMPI=TRUE \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda
```

te tira:

```
CMake Error: The source directory ".../ambertools25_src/cmake" does not appear to contain CMakeLists.txt.
```

Eso es porque le estás diciendo a `cmake` que el **source directory** es `../cmake`, pero **ahí no hay un `CMakeLists.txt`**, y `cmake` necesita ese archivo para construir.

---

### ✅ Lo correcto:

Debes apuntar al directorio raíz de `ambertools25_src`, no a `ambertools25_src/cmake`.

#### Estando en:

```bash
~/programs/amber24/ambertools25_src/build
```

Ejecutá este comando corregido:

```bash
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_GUI=FALSE \
  -DMPI=TRUE \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda
```

---

### 🧠 Aclaración sobre Amber y AmberTools

- **AmberTools** contiene las herramientas básicas: `tleap`, `sander`, `cpptraj`, etc.
    
- **Amber** (Amber24 en tu caso) incluye lo de AmberTools **más** `pmemd`, `pmemd.cuda`, etc., que son versiones optimizadas para CPU y GPU.
    

> 🧬 Sí, **necesitás instalar ambos**: primero AmberTools, luego Amber.

---

### 🧩 Próximos pasos:

1. **Configurar** AmberTools con `cmake ..` (como vimos arriba).
    
2. **Compilar** con:
    
    ```bash
    make -j$(nproc)
    ```
    
3. **Instalar** con:
    
    ```bash
    make install
    ```
    

Cuando termines eso, te paso cómo compilar Amber (`pmemd24`) correctamente.

¿Te parece bien? ¿Ejecutás el `cmake ..` y me contás si anda?


---

# 📌 Mensaje clave

> **Amber recomienda instalar Miniconda automáticamente para manejar las dependencias Python necesarias para sus extensiones científicas.**

El sistema detectó que no tenés Miniconda y por defecto te pide que descargues e instales Miniconda en la carpeta de Amber.

---

# ✅ Qué hacer ahora

Simplemente ejecutá nuevamente el comando **cmake** para que descargue Miniconda automáticamente:

```bash
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_GUI=FALSE \
  -DMPI=TRUE \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda
```

> Esta vez debería aceptar la descarga e instalación automática de Miniconda y continuar con la configuración.

---

# 📝 Si NO querés que descargue Miniconda automáticamente

Podés correr cmake con esta opción para **deshabilitar la descarga**:

```bash
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_GUI=FALSE \
  -DMPI=TRUE \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda \
  -DDOWNLOAD_MINICONDA=FALSE
```

Pero en ese caso, deberías instalar manualmente un entorno de Miniconda compatible y configurar las variables correspondientes.

---

# 💡 Consejo

Yo recomiendo dejar que Amber maneje la instalación de Miniconda para evitar problemas con dependencias Python en el futuro.


LUEGO DE QUE BUILDEE


## Qué significa lo que ves:

- AmberTools se configuró para compilar, **pero sin CUDA** (porque dice `CUDA: OFF`). Esto puede ser porque la configuración detectó que CUDA no está habilitado o la ruta no se configuró bien.
    
- También indica que **pmemd no está incluido en AmberTools**, eso es normal porque `pmemd` está en el paquete Amber comercial (`Amber24`).
    
- Te indica además dónde se va a instalar todo: `/home/lautaro/amber24`
    

---

# Próximos pasos:

### 1) Compilar AmberTools:

Desde la misma carpeta `build`, ejecuta:

```bash
make -j$(nproc)
make install
```

---

### 2) Compilar Amber (pmemd24)

Luego, para compilar Amber (que incluye `pmemd` y la versión CUDA):

1. Salí de `ambertools25_src`, y entra a la carpeta `pmemd24_src` (que tienes ahí).
    
2. Creamos la carpeta `build` (si no existe):
    

```bash
cd ~/programs/amber24/pmemd24_src
mkdir -p build
cd build
```

3. Configuramos con CMake:
    

```bash
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda \
  -DMPI=TRUE
```

4. Compilamos e instalamos:
    

```bash
make -j$(nproc)
make install
```

---

### 3) Fuente de entorno para usar Amber

Una vez terminadas ambas instalaciones, hacé esto para cargar las variables:

```bash
source ~/amber24/amber.sh
```

Podés agregarlo a tu `.bashrc` o `.zshrc`:

```bash
echo "source ~/amber24/amber.sh" >> ~/.bashrc
```

---

### 4) Verificar versión y CUDA

```bash
pmemd.cuda -h
```

o

```bash
pmemd.cuda -v
```

Y con esto vas a verificar que funciona el binario CUDA de Amber.

---

---

Si querés, te ayudo paso a paso con la compilación de `pmemd24_src` cuando termines con AmberTools.



ahora me pide para pmemd me pide conda de nuevo


Perfecto, te volvió a pedir lo mismo que con AmberTools: que deje descargar Miniconda para configurar el entorno Python que Amber necesita.

---

### Para avanzar, simplemente vuelve a ejecutar el mismo comando CMake sin cambios, para que acepte y descargue Miniconda automáticamente:

```bash
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda \
  -DMPI=TRUE
```

Esto debería iniciar la descarga automática de Miniconda y seguir configurando sin error.

---

### Si no querés que descargue Miniconda y ya tenés uno configurado, puedes agregar esta opción para evitar la descarga:

```bash
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
  -DCOMPILER=GNU \
  -DBUILD_CUDA=TRUE \
  -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda \
  -DMPI=TRUE \
  -DDOWNLOAD_MINICONDA=FALSE
```

Pero si elegís esta opción, vas a tener que tener un entorno Python con todas las dependencias necesarias (por ejemplo, con un conda instalado y activado).

---

### Consejo personal

Dejá que Amber descargue Miniconda automáticamente para evitar problemas de dependencias.
Compilamos e instalamos:

bash
Copiar
Editar
make -j$(nproc)
make install
3) Fuente de entorno para usar Amber
Una vez terminadas ambas instalaciones, hacé esto para cargar las variables:

bash
Copiar
Editar
source ~/amber24/amber.sh
Podés agregarlo a tu .bashrc o .zshrc:

bash
Copiar
Editar
echo "source ~/amber24/amber.sh" >> ~/.bashrc
4) Verificar versión y CUDA
bash
Copiar
Editar
pmemd.cuda -h
o

bash
Copiar
Editar
pmemd.cuda -v
Y con esto vas a verificar que funciona el binario CUDA de Amber.




4. Compilamos e instalamos:
    

bash

CopiarEditar

`make -j$(nproc) make install`

---

### 3) Fuente de entorno para usar Amber

Una vez terminadas ambas instalaciones, hacé esto para cargar las variables:

bash

CopiarEditar

`source ~/amber24/amber.sh`

Podés agregarlo a tu `.bashrc` o `.zshrc`:

bash

CopiarEditar

`echo "source ~/amber24/amber.sh" >> ~/.bashrc`

---

### 4) Verificar versión y CUDA

bash

CopiarEditar

`pmemd.cuda -h`

o

bash

CopiarEditar

`pmemd.cuda -v`

Y con esto vas a verificar que funciona el binario CUDA de Amber.

---

### Ahora, para asegurarnos de que CMake detecte CUDA, hagamos esto:

1. Exporta explícitamente las variables de entorno para CUDA (solo en la terminal actual):
    

bash

CopiarEditar

```
export PATH=/usr/local/cuda/bin:$PATH 
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH`
```
2. Borra la carpeta `build` para limpiar la configuración vieja y crea una nueva:
    

bash

CopiarEditar
```
cd ~/programs/amber24/pmemd24_src 
rm -rf build 
mkdir build 
cd build`
```


