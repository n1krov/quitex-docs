
Parece que estás en una red institucional (frre.utn.edu.ar) que podría estar bloqueando o redirigiendo las conexiones a GitHub. El archivo `/etc/resolv.conf` muestra que estás usando un DNS local (127.0.0.53), probablemente gestionado por `systemd-resolved`, y el dominio de búsqueda es `frre.utn.edu.ar`.

### Soluciones para el problema de conexión a GitHub:

#### 1. **Verifica la conectividad a GitHub**:
   ```bash
   curl -v https://github.com
   ping github.com
   wget https://github.com --no-check-certificate
   ```
   Si falla, es probable que haya un bloqueo o problema de DNS/proxy.

#### 2. **Usa un DNS público temporalmente** (como Google o Cloudflare):
   ```bash
   sudo nano /etc/resolv.conf
   ```
   Reemplaza el contenido con:
   ```plaintext
   nameserver 8.8.8.8       # Google DNS
   nameserver 1.1.1.1       # Cloudflare DNS
   options edns0 trust-ad
   ```
   **Nota**: En sistemas con `systemd-resolved`, este cambio puede ser temporal (se restablecerá al reiniciar o cuando `systemd-resolved` se actualice). Para hacerlo permanente:
   ```bash
   sudo nano /etc/systemd/resolved.conf
   ```
   Agrega:
```ini
[Resolve]
DNS=8.8.8.8 1.1.1.1
Domains=frre.utn.edu.ar
```
   Luego reinicia el servicio:
```bash
sudo systemctl restart systemd-resolved
```

#### 3. **Descarga Miniforge manualmente** (recomendado en redes restrictivas):
```bash
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O ~/Miniforge3-Linux-x86_64.sh
chmod +x ~/Miniforge3-Linux-x86_64.sh
./Miniforge3-Linux-x86_64.sh -b -p $HOME/amber25/miniconda
```
   Luego ejecuta CMake deshabilitando la descarga automática:
```bash
cmake ../ -DCMAKE_INSTALL_PREFIX=$HOME/amber25 \
   -DCOMPILER=GNU \
   -DBUILD_SHARED_LIBS=TRUE \
   -DMPI=TRUE \
   -DCMAKE_BUILD_TYPE=Release \
   -DDOWNLOAD_MINICONDA=FALSE
```

#### 4. **Configura un proxy si es necesario** (si estás detrás de un proxy institucional):
```bash
export http_proxy=http://proxy.frre.utn.edu.ar:3128  # Reemplaza con tu proxy real
export https_proxy=http://proxy.frre.utn.edu.ar:3128
```
   (Consulta con el departamento de TI de tu institución la configuración exacta del proxy).

#### 5. **Alternativa: Usa Python del sistema** (si tienes Python 3.9+ instalado):
```bash
cmake ../ -DUSE_CONDA=FALSE  # Omite la instalación de Miniforge
```

