# Acceso al Servidor

Esta guía reúne las formas de acceso al servidor según el origen de la conexión. Se indican los datos necesarios para transferencia de archivos por SFTP y acceso por terminal mediante SSH.

---

## Desde fuera de la red de la facu

> A red de la facultad nos referimos cuando hablamos de cualquier WiFi que sea de la facultad o del anexo

### Por SFTP, para transferencia de archivos

- Explorador de archivos: `sftp://190.114.201.4:11127`
- Terminal: `sftp -P 11127 usuario@190.114.201.4`

### Por SSH, para interfaz de terminal

- Terminal: `ssh -p 11127 usuario@190.114.201.4`

---

## Desde la red de la facu

> [!WARNING]
> Si el acceso falla, puede deberse a una clave SSH guardada previamente. En ese caso, ejecuta primero:

```bash
ssh-keygen -R 10.13.0.127
```

### Por SFTP, para transferencia de archivos

- Explorador de archivos: `sftp://10.13.0.127`
- Terminal: `sftp -P 11127 usuario@10.13.0.127`

### Por SSH, para interfaz de terminal

```bash
ssh usuario@10.13.0.127
```

> [!IMPORTANT]
> En este caso no se usa puerto.

---

## Credenciales de administradores

- Usuario de administración del servidor: `lautaro`
- Contraseña: `esqwer`

> Otros usuarios (investigadores y directores) deben acceder con sus usuarios.

---