---
title: Instalación de NVIDIA
date: 2024-01-01
---

### **Paso 1: Verifica tu GPU NVIDIA**
Primero, asegúrate de que el sistema detecta correctamente la tarjeta gráfica NVIDIA:
```bash
lspci | grep -i nvidia
```

Si aparece una línea con detalles de tu GPU, puedes proceder con la instalación.

---

### **Paso 2: Instala los controladores NVIDIA**
1. **Agrega el repositorio de controladores NVIDIA**  
   Ubuntu proporciona controladores actualizados a través de un repositorio:
   ```bash
   sudo apt install software-properties-common
   sudo add-apt-repository ppa:graphics-drivers/ppa
   sudo apt update
   ```

2. **Determina el controlador recomendado**  
   Usa este comando para ver el controlador adecuado para tu GPU:
   ```bash
   ubuntu-drivers devices
   ```

3. **Instala el controlador recomendado**  
   Por ejemplo, si el sistema sugiere `nvidia-driver-525`, instálalo con:
   ```bash
   sudo apt install nvidia-driver-525 -y
   ```

4. **Reinicia el servidor**  
   Aplica los cambios reiniciando el servidor:
   ```bash
   sudo reboot
   ```

5. **Verifica la instalación**  
   Después de reiniciar, verifica que los controladores están instalados correctamente:
   ```bash
   nvidia-smi
   ```
   Este comando debe mostrar información sobre tu GPU, incluida la versión del controlador.

---

### **Paso 3: Instalar CUDA (si es necesario)**
Si Amber o alguna otra herramienta necesita CUDA para simulaciones en GPU, instálalo:

1. **Descarga el instalador de CUDA**  
   Ve a la [página oficial de NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) y descarga el instalador correspondiente para tu versión de Ubuntu.

2. **Instala CUDA**  
   Una vez descargado, sigue las instrucciones específicas del instalador, por ejemplo:
   ```bash
   sudo dpkg -i cuda-repo-ubuntu<version>_<cuda_version>_amd64.deb
   sudo apt update
   sudo apt install cuda -y
   ```

3. **Configura las variables de entorno**  
   Agrega las rutas de CUDA al archivo `~/.bashrc` de cada usuario:
   ```bash
   echo "export PATH=/usr/local/cuda/bin:\$PATH" >> ~/.bashrc
   echo "export LD_LIBRARY_PATH=/usr/local/cuda/lib64:\$LD_LIBRARY_PATH" >> ~/.bashrc
   source ~/.bashrc
   ```

4. **Verifica la instalación de CUDA**  
   Asegúrate de que CUDA está funcionando:
   ```bash
   nvcc --version
   ```

---

### **Paso 4: Configura Amber para usar GPU**

Si Amber usará GPU, asegúrate de que las herramientas de Amber estén configuradas correctamente:

1. **Verifica que el binario `pmemd.cuda` esté disponible**  
   Esto asegura que Amber esté configurado para ejecutar en GPU.

2. **Prueba una simulación en GPU**  
   Ejecuta un caso de prueba en Amber para confirmar que la GPU está siendo utilizada:
   ```bash
   pmemd.cuda -O -i input.in -o output.out -p prmtop -c inpcrd -r restrt -x mdcrd
   ```
   Si la simulación funciona correctamente y `nvidia-smi` muestra uso de GPU, ¡todo está configurado!

---

### **Seguridad adicional al instalar drivers**
Si en el futuro necesitas actualizar los controladores NVIDIA, puedes hacerlo directamente con:
```bash
sudo apt update
sudo apt upgrade -y
```

