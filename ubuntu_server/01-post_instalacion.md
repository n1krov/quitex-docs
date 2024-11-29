
### **Configuraciones básicas post-instalación**
1. **Actualizar el sistema**  
   Asegúrate de tener todo actualizado:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Crear los usuarios y asignarles privilegios sudo**  
   Ejecuta estos comandos para agregar los usuarios:
   ```bash
   sudo adduser lautaro
   sudo usermod -aG sudo lautaro

   sudo adduser nicolai
   sudo usermod -aG sudo nicolai
   ```
   - Te pedirá una contraseña al crear cada usuario.
   - Los usuarios tendrán acceso a privilegios de administrador mediante `sudo`.

3. **Habilitar SSH para acceso remoto**
   Si no está instalado, instala y habilita el servicio SSH:
   ```bash
   sudo apt install openssh-server -y
   sudo systemctl enable ssh
   sudo systemctl start ssh
   ```
   Para verificar que SSH está corriendo:
   ```bash
   sudo systemctl status ssh
   ```

4. **Configurar el acceso por SSH**
   - Edita el archivo de configuración SSH:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Asegúrate de tener estas configuraciones:
     ```
     PermitRootLogin no
     PasswordAuthentication yes
     ```
     Esto deshabilita el acceso SSH para el usuario root y permite contraseñas para usuarios normales.
   - Aplica los cambios:
     ```bash
     sudo systemctl restart ssh
     ```

5. **Obtener la dirección IP del servidor**  
   Usa este comando para saber la IP:
   ```bash
   ip addr show
   ```
   Anota la dirección bajo `inet` (normalmente en `eth0` o `ens33`).

   Ahora, los usuarios pueden conectarse usando:
   ```bash
   ssh usuario@<IP_del_Servidor>
   ```

---

### **Configurar el entorno para Amber**
1. **Instalar las dependencias para Amber**  
   Amber necesita varias herramientas como **gcc**, **make**, y **Python**:
   ```bash
   sudo apt install build-essential cmake gfortran python3 python3-pip -y
   ```

2. **Configurar Amber en el servidor**
   - Descarga Amber desde el sitio oficial si tienes acceso.
   - Descomprime el archivo en un directorio adecuado, por ejemplo:
     ```bash
     mkdir ~/amber && tar -xvf ambertools22.tar.bz2 -C ~/amber
     ```
   - Sigue las instrucciones de instalación de Amber en su manual.

3. **Configurar variables de entorno**  
   Agrega las variables necesarias al archivo `~/.bashrc` de cada usuario (`lautaro` y `nicolai`):
   ```bash
   echo "export AMBERHOME=~/amber" >> ~/.bashrc
   echo "export PATH=\$AMBERHOME/bin:\$PATH" >> ~/.bashrc
   source ~/.bashrc
   ```

4. **Pruebas de instalación de Amber**  
   - Asegúrate de que `amber` y sus herramientas se ejecutan correctamente:
     ```bash
     pmemd --help
     ```
   - Si necesitas ejecutar simulaciones, verifica el rendimiento y asegúrate de que las dependencias funcionan.

---

### **Seguridad adicional del servidor**
1. **Firewall con UFW**
   Habilita el firewall y permite solo SSH:
   ```bash
   sudo ufw allow OpenSSH
   sudo ufw enable
   ```
   Verifica el estado:
   ```bash
   sudo ufw status
   ```

2. **Habilitar acceso SSH con claves públicas (opcional)**
   - Genera claves SSH en los clientes (`lautaro` y `nicolai`):
     ```bash
     ssh-keygen
     ```
   - Copia la clave pública al servidor:
     ```bash
     ssh-copy-id usuario@<IP_del_Servidor>
     ```

3. **Monitorización del servidor**
   Instala herramientas útiles como:
   - `htop`: Para monitorizar el uso de CPU y RAM.
     ```bash
     sudo apt install htop
     ```
   - `nmon`: Para monitorización de rendimiento más detallada.
     ```bash
     sudo apt install nmon
     ```


