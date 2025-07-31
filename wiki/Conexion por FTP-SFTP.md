# 📡 Conexión por FTP/SFTP

## 🌐 Desde el explorador de archivos (Thunar)

1. Abre **Thunar** (Explorador de archivos)
2. En la barra lateral, haz clic en **"Otras ubicaciones"**
3. En la barra de dirección, escribe:
   ```
   sftp://[IP]:[PUERTO]
   ```
   Ejemplo:
   ```
   sftp://192.168.1.100:22
   ```


![[Pasted image 20250731084103.png]]

**Notas:**
- Reemplaza `[IP]` por la dirección del servidor
- Reemplaza `[PUERTO]` por el puerto SSH
- Se te pedirán **credenciales** de **acceso**

---

## 💻 Desde terminal (Linux/macOS)

### 🔹 Conexión básica con FTP

```bash
ftp -p usuario@servidor
```

Ejemplo práctico:
```sh
ftp -p juan@ftp.miempresa.com
```

**Opciones:**
- `-p`: Habilita modo pasivo (recomendado)
- Para conexión anónima usa:
  ```sh
  ftp -p anonymous@servidor
  ```

### 🔹 Comandos esenciales FTP

| Comando       | Descripción                     | Ejemplo                  |
|--------------|--------------------------------|--------------------------|
| `ls`         | Lista archivos remotos          | `ls /documentos`         |
| `cd`         | Cambia directorio remoto        | `cd /descargas`          |
| `lcd`        | Cambia directorio local         | `lcd ~/subidas`          |
| `get`        | Descarga un archivo             | `get informe.pdf`        |
| `put`        | Sube un archivo                 | `put proyecto.zip`       |
| `binary`     | Cambia a modo binario           | `binary`                 |
| `ascii`      | Cambia a modo texto             | `ascii`                  |
| `help`       | Muestra ayuda                   | `help put`               |
| `quit`       | Cierra la conexión              | `quit`                   |

---

## 🛡️ Conexión segura (recomendada)

### 🔐 Usando SFTP (SSH File Transfer)

```bash
sftp usuario@servidor -P puerto
```

Ejemplo:
```sh
sftp admin@10.0.0.5 -P 2222
```

**Ventajas:**
- Conexión encriptada
- Usa el puerto SSH (normalmente 22)
- Mismos comandos básicos que FTP

---

## 🔧 Solución de problemas

### ⚠️ Errores comunes

1. **"Connection refused"**
   - Verifica que el servidor FTP esté activo
   - Confirma la **IP** y **puerto** correctos
   - Revisa reglas del firewall

2. **Problemas de autenticación**
   - Confirma usuario/contraseña
   - Verifica permisos de cuenta