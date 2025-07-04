## 🧬 Instalación de AMBER 24 + AmberTools en Ubuntu Server

> 📌 **Requisitos previos**: Asegurate de tener permisos de sudo, suficiente espacio en disco (10+ GB recomendados), y una conexión a Internet activa.

---

### 🔧 1. Instalar dependencias necesarias

```bash
sudo apt update
sudo apt install -y build-essential \
    gfortran \
    flex bison \
    patch cmake \
    xorg-dev \
    libxext-dev \
    zlib1g-dev \
    libbz2-dev \
    libnetcdf-dev \
    libfftw3-dev \
    libopenmpi-dev openmpi-bin \
    wget git \
    python3 python3-pip
```

tenemos lo siguiente


```
📦 ambertools25.tar.bz2      (583 MB)
📦 pmemd24.tar.bz2           (255 MB)
📁 ambertools25_src/         (descomprimido)
📁 pmemd24_src/              (descomprimido)
```

---

## ✅ Próximo paso: compilar e instalar AmberTools 25 + PMEMD 24

Vamos paso a paso ahora que ya los tenés listos.

---

## 🔧 1. Preparar entorno y dependencias

Ya lo habías hecho, pero recordá instalar lo necesario:

```bash
sudo apt update
sudo apt install -y \
    build-essential \
    gfortran \
    cmake \
    flex bison \
    patch \
    xorg-dev \
    libxext-dev \
    zlib1g-dev \
    libbz2-dev \
    libnetcdf-dev \
    libfftw3-dev \
    libopenmpi-dev openmpi-bin \
    python3 python3-pip
```

---

## 📦 2. Instalar AmberTools 25

```bash
cd ambertools25_src
mkdir build
cd build

cmake ../ -DCMAKE_INSTALL_PREFIX=$HOME/amber25 \
    -DCOMPILER=GNU \
    -DBUILD_SHARED_LIBS=TRUE \
    -DMPI=TRUE \
    -DCMAKE_BUILD_TYPE=Release

make -j$(nproc)
make install
```

---

## ⚙️ 3. Instalar PMEMD 24

```bash
cd ~/pmemd24_src
mkdir build
cd build

cmake ../ -DCMAKE_INSTALL_PREFIX=$HOME/amber25 \
    -DCOMPILER=GNU \
    -DMPI=TRUE \
    -DCMAKE_BUILD_TYPE=Release

make -j$(nproc)
make install
```

> Esto instalará `pmemd` junto con lo que ya contiene AmberTools.

---

## 🌿 4. Configurar el entorno

Agregá esto al final de tu `~/.bashrc`:

```bash
export AMBERHOME=$HOME/amber25
export PATH=$AMBERHOME/bin:$PATH
export LD_LIBRARY_PATH=$AMBERHOME/lib:$LD_LIBRARY_PATH
```

Luego aplicalo:

```bash
source ~/.bashrc
```

---

## 🧪 5. Verificar instalación

```bash
which sander
which pmemd
```

Y si querés ejecutar los tests:

```bash
cd $AMBERHOME
make test
```

---

### 🧪 5. Verificación de instalación

Probá la suite de tests (opcional):

```bash
cd $AMBERHOME
make test
```

