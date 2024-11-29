# Idioma de Teclado

### **Paso 1: Instalar las configuraciones de localización (si es necesario)**
Si el idioma español no está instalado, instálalo con:
```bash
sudo apt install language-pack-es
```

---

### **Paso 2: Configurar el teclado a LATAM**
1. **Editar la configuración del teclado**  
   Usa el comando `dpkg-reconfigure` para seleccionar el teclado:
   ```bash
   sudo dpkg-reconfigure keyboard-configuration
   ```
   - Selecciona el modelo del teclado (generalmente, "Generic 105-key").
   - En la sección de disposición del teclado (layout), selecciona:
     - **Español** (Spanish).
     - Subopción: **Latin American**.

2. **Aplica los cambios**  
   Después de configurar el teclado, reinicia el servicio de entrada:
   ```bash
   sudo systemctl restart keyboard-setup
   ```

---

### **Paso 3: Configurar la variable `XKB` (opcional)**
Si `dpkg-reconfigure` no aplica el cambio, puedes editar manualmente el archivo de configuración:
1. Abre el archivo `/etc/default/keyboard`:
   ```bash
   sudo nano /etc/default/keyboard
   ```
2. Asegúrate de que las líneas estén configuradas así:
   ```
   XKBMODEL="pc105"
   XKBLAYOUT="latam"
   XKBVARIANT=""
   XKBOPTIONS=""
   ```
3. Guarda los cambios y aplica:
   ```bash
   sudo setupcon
   ```

---

### **Paso 4: Verifica la configuración**
Para verificar que el teclado está configurado como **LATAM**, abre la consola y prueba caracteres especiales como `ñ` o `á`. Si funcionan correctamente, ¡el teclado está listo!

---

Si necesitas más ayuda con la configuración o tienes problemas, avísame. 😊
