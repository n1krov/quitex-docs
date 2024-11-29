
Para configurar un servidor con Ubuntu 22.04 LTS para que el usuario **root** tenga acceso directo (y habilitar el inicio de sesión como **root**), debes seguir algunos pasos. Normalmente, en Ubuntu, el usuario **root** está deshabilitado por defecto por razones de seguridad, y en su lugar se utiliza el comando `sudo` para ejecutar tareas administrativas.

A continuación, te muestro cómo habilitar el acceso del usuario **root** y configurarlo para que pueda iniciar sesión en el servidor, ya sea localmente o por **SSH**.

---

### 1. **Establecer contraseña para el usuario root**
El usuario **root** tiene su contraseña deshabilitada por defecto. Para habilitar el acceso como root, primero necesitas asignarle una contraseña.

Ejecuta el siguiente comando para establecer la contraseña del usuario **root**:

```bash
sudo passwd root
```

Te pedirá que ingreses y confirmes la nueva contraseña para el usuario **root**.

### 2. **Habilitar el inicio de sesión como root por SSH**
Por razones de seguridad, el acceso SSH como root está deshabilitado por defecto en Ubuntu. Si necesitas habilitarlo, sigue estos pasos:

#### Paso 1: Edita el archivo de configuración de SSH

```bash
sudo nano /etc/ssh/sshd_config
```

#### Paso 2: Habilita el acceso de root

Dentro de este archivo, busca la línea siguiente (probablemente estará comentada):

```bash
#PermitRootLogin prohibit-password
```

Cambia esa línea a:

```bash
PermitRootLogin yes
```

Si prefieres una opción más segura, como permitir solo la autenticación con claves SSH para root, puedes usar:

```bash
PermitRootLogin prohibit-password
```

Esto permitirá que el usuario root se conecte solo con claves SSH (y no con contraseñas).

#### Paso 3: (Opcional) Cambiar el puerto SSH
Si no lo has hecho antes, puedes cambiar el puerto SSH para mejorar la seguridad, por ejemplo, a `2222` en lugar del puerto predeterminado `22`:

```bash
Port 2222
```

#### Paso 4: Reiniciar el servicio SSH

Después de hacer los cambios, guarda el archivo (`Ctrl + O`, luego `Enter` y `Ctrl + X` para salir) y reinicia el servicio **SSH** para aplicar los cambios:

```bash
sudo systemctl restart ssh
```

### 3. **Acceso remoto como root**
Ahora que el acceso de root está habilitado, puedes conectarte remotamente usando **SSH** como el usuario root.

Si cambiaste el puerto SSH a `2222`, asegúrate de conectarte especificando el puerto:

```bash
ssh root@direccion-ip-del-servidor -p 2222
```

Si mantuviste el puerto predeterminado (22), el comando sería:

```bash
ssh root@direccion-ip-del-servidor
```

### 4. **Consideraciones de Seguridad**
Habilitar el acceso de **root** directamente en el servidor conlleva ciertos riesgos de seguridad. Algunas recomendaciones adicionales:

- **Utiliza autenticación con claves SSH** en lugar de contraseñas para el usuario root.
  
  Genera una clave SSH en la máquina desde la que vas a conectarte (si no tienes una ya):

  ```bash
  ssh-keygen -t rsa -b 4096
  ```

  Copia la clave pública a tu servidor:

  ```bash
  ssh-copy-id root@direccion-ip-del-servidor
  ```

- **Configura el firewall** con **UFW** para permitir solo el puerto SSH y bloquear el resto, si aún no lo has hecho:

  ```bash
  sudo ufw allow 2222/tcp  # O el puerto que uses para SSH
  sudo ufw enable
  ```

- **Deshabilita el acceso root por SSH cuando no sea necesario**. Puedes usar el acceso **sudo** para tareas administrativas y habilitar el acceso root solo temporalmente si es estrictamente necesario.

---



