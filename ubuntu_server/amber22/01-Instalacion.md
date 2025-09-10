# Instalación de Amber22 en Servidor con Ubuntu 24.04 LTS

Este documento describe los pasos realizados para instalar **Amber22** y **AmberTools23** en un servidor con **Ubuntu 24.04 LTS**, resolviendo los conflictos con CUDA y Python.

---

## 🚀 Pasos de instalación

### 1. Instalación de CUDA 12.1

La instalación inicial de **Amber22** requería CUDA 12.1.  
En Ubuntu 24.04 LTS se presentaron conflictos de compatibilidad, pero finalmente se logró instalar con éxito la versión correcta de CUDA.

---

### 2. Descarga de Amber22 y AmberTools23

Se utilizaron los instaladores de **Amber22** y **AmberTools23** provistos por el grupo **QUITEX**, siguiendo la documentación oficial:  
👉 [https://ambermd.org](https://ambermd.org)

---

### 3. Configuración de Python con Miniconda

El instalador de Amber intenta descargar e instalar su propia versión de **Miniconda**, lo que generaba conflictos con **Python 3.13** (incompatible con algunos paquetes requeridos).

Para solucionarlo, se creó un **entorno virtual con Miniconda3** y se configuró CMake para que utilice esa versión de Python.

#### Entorno virtual `amber22_build`

```bash
# Crear y activar el entorno virtual
conda create -n amber22_build python=3.10
conda activate amber22_build

# Instalar las dependencias necesarias
pip install build-essential scipy matplotlib pandas
```

#### Cambios realizados en archivos de configuración:

**1. `PythonInterpreterConfig.cmake`**

- Comentadas las líneas 71 y 72:

  ```cmake
  # include(UseMiniconda)
  # download_and_use_miniconda()
  ```

- Modificada la línea 74:

  ```cmake
  set(PYTHON_EXECUTABLE /home/lautaro/miniconda3/envs/amber22_build/bin/python)
  ```

**2. `CMakeLists.txt`**

- Comentado el bloque entre las líneas 268 y 279 para evitar instalación automática de Miniconda:

  ```cmake
  # if(BUILD_PYTHON AND DOWNLOAD_MINICONDA)
  #   install_miniconda()
  # endif()
  ```

---

### 4. Instalación de Amber22

Con los cambios aplicados, se siguieron los pasos de instalación recomendados:

```bash
# Asegurarse de activar el entorno virtual
conda activate amber22_build

# Entrar a la carpeta build del instalador
cd amber22_src/build

# Ejecutar cmake
./run_cmake

# Compilar e instalar
make install

# Correr script final
cd ..
source amber.sh
```

---

### 5. Pruebas de verificación

Para comprobar que la instalación fue exitosa:

```bash
which sander
which pmemd
which tleap
```

Ejemplo de prueba rápida:

```bash
cd $AMBERHOME/test/dhfr
./Run.dhfr
```

Los programas (`sander`, `pmemd`, `tleap`, etc.) se ejecutaron correctamente y los tests confirmaron que la instalación es funcional.

---

## ✅ Conclusión

La instalación de **Amber22 + AmberTools23** en **Ubuntu 24.04 LTS** requirió:

- Ajustar la versión de CUDA a **12.1**
- Configurar un entorno de **Miniconda** externo
- Editar los archivos de CMake para evitar conflictos de Python

Con estas modificaciones, Amber22 quedó instalado y operativo en el servidor.

---
