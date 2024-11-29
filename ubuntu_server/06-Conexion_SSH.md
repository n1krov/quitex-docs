
Para conectarte al **Ubuntu Server** desde otro lugar como el usuario `lautaro`, puedes hacerlo usando **SSH** (Secure Shell). A continuación, te detallo los pasos necesarios:

---

### **Paso 1: Asegúrate de que SSH está habilitado en el servidor**
- Verifica que el servicio **SSH** esté ejecutándose en el servidor:
  ```bash
  sudo systemctl status ssh
  ```
  Si no está en ejecución, inícialo:
  ```bash
  sudo systemctl start ssh
  ```

- Si no estás seguro de la IP pública o privada del servidor, en el servidor ejecuta:
  ```bash
  ip addr show
  ```

  - **IP privada**: Si el cliente está en la misma red local.
  - **IP pública**: Si el cliente está fuera de la red local.

  Para verificar la IP pública, puedes usar:
  ```bash
  curl ifconfig.me
  ```

---

### **Paso 2: Configura el acceso al servidor**
Si estás accediendo desde fuera de la red local:
1. **Reenvío de puertos en el router**  
   Configura el router para redirigir el puerto 22 (SSH) de la IP privada del servidor hacia Internet.
   - **IP del servidor interno**: Por ejemplo, `192.168.1.100`.
   - **Puerto redirigido**: 22.

2. **Opcional: Cambiar el puerto SSH**  
   Por razones de seguridad, puedes cambiar el puerto SSH. Edita el archivo de configuración:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
   Cambia la línea:
   ```
   Port 22
   ```
   Por otro puerto (por ejemplo, `2222`), y reinicia el servicio:
   ```bash
   sudo systemctl restart ssh
   ```
   Asegúrate de abrir el puerto en el firewall:
   ```bash
   sudo ufw allow 2222/tcp
   ```

---

### **Paso 3: Conéctate al servidor desde tu máquina**
1. Abre una terminal en tu máquina local.
2. Usa el comando `ssh` para conectarte al servidor. Supongamos que la IP del servidor es `203.0.113.10` y el puerto es `22`:
   ```bash
   ssh lautaro@203.0.113.10
   ```

   Si cambiaste el puerto SSH, especifica el puerto con `-p`:
   ```bash
   ssh lautaro@203.0.113.10 -p 2222
   ```

3. Ingresa tu contraseña cuando se te solicite.

---

### **Paso 4: Acceso con claves SSH (opcional, pero recomendado)**
Para mayor seguridad, usa claves SSH en lugar de contraseñas.

1. **Genera una clave SSH en tu máquina local**  
   En tu computadora, ejecuta:
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
   Esto creará dos archivos:
   - `~/.ssh/id_rsa` (clave privada, no la compartas).
   - `~/.ssh/id_rsa.pub` (clave pública).

2. **Copia la clave pública al servidor**  
   Usa el comando:
   ```bash
   ssh-copy-id lautaro@203.0.113.10
   ```
   Si usaste un puerto diferente:
   ```bash
   ssh-copy-id -p 2222 lautaro@203.0.113.10
   ```

3. **Verifica el acceso con claves SSH**  
   Ahora podrás conectarte sin ingresar contraseña:
   ```bash
   ssh lautaro@203.0.113.10
   ```

---

### **Paso 5: Solución de problemas comunes**
1. **No puedes acceder desde fuera de la red local**  
   - Verifica que el reenvío de puertos esté configurado correctamente en el router.
   - Asegúrate de que el puerto SSH esté permitido en el firewall del servidor:
     ```bash
     sudo ufw allow 22/tcp
     ```

2. **IP dinámica del servidor**  
   Si la IP pública del servidor cambia, considera usar un servicio de DNS dinámico como [No-IP](https://www.noip.com/) o [DuckDNS](https://www.duckdns.org/).


