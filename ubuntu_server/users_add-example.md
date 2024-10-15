
# Añadir Usuarios con Permisos **sudo** en Ubuntu 22.04 LTS

Para configurar tu servidor Ubuntu 22.04 LTS con dos usuarios que tengan permisos de **sudo** (en este caso, `lautaro` y `nicolai`), debes seguir los pasos para crear los usuarios y otorgarles permisos de **sudo**.

### Pasos para configurar dos usuarios con permisos **sudo**

#### 1. **Crear los usuarios**

Primero, debes crear los usuarios `lautaro` y `nicolai`:

##### Crear usuario `lautaro`

1. Ejecuta el siguiente comando para crear el usuario `lautaro`:

   ```bash
   sudo adduser lautaro
   ```

2. Sigue las instrucciones para definir una contraseña para `lautaro` y completa la información adicional (opcional).

##### Crear usuario `nicolai`

1. Ejecuta el siguiente comando para crear el usuario `nicolai`:

   ```bash
   sudo adduser nicolai
   ```

2. Asigna una contraseña para `nicolai` y completa la información adicional (opcional).

#### 2. **Añadir los usuarios al grupo `sudo`**

Una vez que los usuarios han sido creados, debes agregarlos al grupo `sudo` para que puedan ejecutar comandos como superusuarios.

##### Añadir `lautaro` al grupo `sudo`

1. Usa el siguiente comando para agregar el usuario `lautaro` al grupo `sudo`:

   ```bash
   sudo usermod -aG sudo lautaro
   ```

##### Añadir `nicolai` al grupo `sudo`

1. Usa el siguiente comando para agregar el usuario `nicolai` al grupo `sudo`:

   ```bash
   sudo usermod -aG sudo nicolai
   ```

#### 3. **Verificar que los usuarios tienen permisos de sudo**

Para verificar que ambos usuarios tienen permisos de **sudo**, inicia sesión como cada uno de ellos y prueba un comando que requiere privilegios administrativos, como `sudo apt update`.

##### Verificación para `lautaro`

1. Cambia al usuario `lautaro`:

   ```bash
   su - lautaro
   ```

2. Verifica los permisos de **sudo**:

   ```bash
   sudo apt update
   ```

3. Si se te pide la contraseña y el comando se ejecuta correctamente, entonces `lautaro` tiene permisos de **sudo**.

##### Verificación para `nicolai`

1. Cambia al usuario `nicolai`:

   ```bash
   su - nicolai
   ```

2. Verifica los permisos de **sudo**:

   ```bash
   sudo apt update
   ```

3. Si se te pide la contraseña y el comando se ejecuta correctamente, entonces `nicolai` tiene permisos de **sudo**.

#### 4. **(Opcional) Configurar autenticación SSH para cada usuario**

Si ambos usuarios van a acceder al servidor a través de **SSH**, puedes configurar claves SSH para cada uno.

1. Genera una clave SSH en las máquinas desde las que te conectarás (si no tienes una):

   ```bash
   ssh-keygen -t rsa -b 4096
   ```

2. Copia la clave pública de cada usuario a su respectiva cuenta en el servidor:

   Para `lautaro`:

   ```bash
   ssh-copy-id lautaro@direccion-ip-del-servidor
   ```

   Para `nicolai`:

   ```bash
   ssh-copy-id nicolai@direccion-ip-del-servidor
   ```

---

