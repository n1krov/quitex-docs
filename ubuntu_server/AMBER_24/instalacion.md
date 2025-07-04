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

---

### 📦 2. Descargar AMBER 24 y AmberTools

Debés tener una cuenta válida y licencia para **AMBER 24**. El acceso suele hacerse a través del portal oficial:  
🔗 [https://ambermd.org/GetAmber.php](https://ambermd.org/GetAmber.php)

```bash
mkdir ~/amber24
cd ~/amber24
# Copiá aquí los archivos descargados:
# amber24.tar.bz2 y AmberTools24.tar.bz2
```

Luego, descomprimí ambos:

```bash
tar -xvjf AmberTools24.tar.bz2
tar -xvjf amber24.tar.bz2
```

Esto generará el directorio `amber24_src`.

---

### ⚙️ 3. Compilar AmberTools + AMBER

Ingresá al directorio:

```bash
cd amber24_src
```

Configurá la compilación (usamos CMake como recomienda AMBER):

```bash
mkdir build
cd build
cmake ../ \
    -DCMAKE_INSTALL_PREFIX=$HOME/amber24 \
    -DCOMPILER=GNU \
    -DBUILD_SHARED_LIBS=TRUE \
    -DMPI=TRUE \
    -DCMAKE_BUILD_TYPE=Release
```

Compilá e instalá (puede demorar bastante):

```bash
make -j$(nproc)
make install
```

---

### ✅ 4. Configurar entorno

Agregá estas líneas a tu `~/.bashrc` (o `~/.zshrc` si usás zsh):

```bash
export AMBERHOME=$HOME/amber24
export PATH=$AMBERHOME/bin:$PATH
export LD_LIBRARY_PATH=$AMBERHOME/lib:$LD_LIBRARY_PATH
```

Luego:

```bash
source ~/.bashrc
```

Verificá que `pmemd` y `sander` estén disponibles:

```bash
which pmemd
which sander
```

---

### 🧪 5. Verificación de instalación

Probá la suite de tests (opcional):

```bash
cd $AMBERHOME
make test
```

---

## 🚧 ¿Y Gaussian09?

Cuando estés listo, te ayudo con Gaussian09, que requiere pasos adicionales y licencias especiales. Avísame si querés seguir con eso ahora o más adelante.

---

¿Querés que esto te lo exporte como `.md` o `.pdf` también?