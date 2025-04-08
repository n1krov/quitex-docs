
# Verificar que tarjeta gráfica tienes

```bash
lspci | grep -i nvidia
```

## 🚧 Paso 1: Instalar driver propietario de NVIDIA

### ✅ Opción recomendada (automática):

```bash
sudo apt update
sudo apt install nvidia-driver-535
sudo reboot
```

> 🔎 El `535` es el driver más moderno **compatible** con la TITAN X. Si tu sistema devuelve error, probá con `nvidia-driver-525` o `nvidia-driver-470`.

---

### 🔍 Verificación después del reinicio:

Una vez que tu sistema reinicie, corré:

```bash
nvidia-smi
```

Y deberías ver algo así:

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 535.xx.xx   Driver Version: 535.xx.xx   CUDA Version: 12.x      |
|-------------------------------+----------------------+----------------------+
```

---

## 🔄 Paso 2: Instalar CUDA

Una vez que `nvidia-smi` funcione, seguimos con la instalación de CUDA como te mostré antes. Te repito el resumen rápido por si querés avanzar más ágil:

---

### 🧩 Instalación de CUDA resumida

1. Ir a [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)
2. Seleccionar:
   - OS: Linux
   - Distro: la tuya (ej. Ubuntu 22.04)
   - Installer Type: `.deb (local)`

3. Ejecutar los comandos que te da la web, algo como:

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600

wget https://developer.download.nvidia.com/compute/cuda/12.3.2/local_installers/cuda-repo-ubuntu2204-12-3-local_12.3.2-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2204-12-3-local_12.3.2-1_amd64.deb

sudo cp /var/cuda-repo-ubuntu2204-12-3-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt update
sudo apt install cuda
```

