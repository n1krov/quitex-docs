# Instalación y configuración de WebMO 25.2 en Ubuntu 24.04
**Servidor:** quitex_server | **IP:** 190.114.201.4 | **Puerto SSH:** 11127

*Fecha: 1 de abril de 2026*

---

## Documentación oficial

* [WebMO Documentation](https://www.webmo.net/support/documentation.html)
* [Installing WebMO on Linux](https://www.webmo.net/link/help/InstallingWebMO-Linux.html)
* [WebMO Tutorials](https://www.webmo.net/link/help/)

> En el último links se pueden encontrar los primeros pasos y tutoriales de uso de la herramienta, además de videos y otros documentos de ayúda práctica.

---

## Información de licencia de WebMO

| Campo | Valor |
|---|---|
| License number | `2604-0118-9565` |
| Password | `60OxjTy2aXIiE` |

---

## Requisitos previos

Antes de instalar WebMO, asegurarse de tener instalado:

```bash
sudo apt-get install apache2 perl libcgi-pm-perl
```

Habilitar el módulo CGI en Apache:

```bash
sudo a2enmod cgi
sudo systemctl restart apache2
```

---

## Preparación de directorios

El instalador corre como usuario `lautaro` (no root), por lo que los directorios globales deben crearse manualmente con los permisos correctos antes de ejecutar el instalador:

```bash
sudo mkdir -p /var/www/html/webmo
sudo mkdir -p /usr/lib/cgi-bin/webmo
sudo mkdir -p /var/lib/webmo
sudo chmod 755 /var/www/html/webmo
sudo chmod 755 /usr/lib/cgi-bin/webmo
sudo chmod 777 /var/lib/webmo
sudo chown lautaro:lautaro /var/www/html/webmo
sudo chown lautaro:lautaro /usr/lib/cgi-bin/webmo
sudo chown lautaro:lautaro /var/lib/webmo
```

> El `chown` es necesario porque el instalador Perl necesita escribir en esos directorios como usuario `lautaro`. Puede revertirse a `root:root` tras la instalación si se desea mayor seguridad.

---

## Instalación

Descomprimir el archivo descargado y ejecutar el instalador **sin sudo**:

```bash
cd ~
tar xzf WebMO.25.2.004.tar.gz
cd WebMO.install
perl setup.pl
```

> **Importante:** Correr `perl setup.pl` sin `sudo`. El instalador detecta si se corre como root y no permite continuar. Debe correrse como usuario normal (`lautaro`).

Cuando el script lo solicite, ingresar los siguientes valores:

| Pregunta | Valor |
|---|---|
| License number | `2604-0118-9565` |
| Path to Perl binary | `/usr/bin/perl` *(Enter, default)* |
| Webserver name | `quitex_server` |
| HTML directory | `/var/www/html/webmo` |
| HTML URL | `/webmo` |
| CGI script directory | `/usr/lib/cgi-bin/webmo` |
| CGI script URL | `/cgi-bin/webmo` |
| User files directory | `/var/lib/webmo` |

---

## Modificaciones realizadas respecto al tutorial oficial

### 1. Instalación global en lugar de por usuario

El tutorial oficial instala WebMO en el home de un usuario específico (`~/public_html/webmo`). En el servidor se realizó una instalación global personalizada, para que todos los usuarios puedan acceder, usando los directorios estándar de Apache.

### 2. Nombre del servidor con guión bajo

El nombre de host `quitex-server` (con guión medio) era rechazado por la validación interna de WebMO. La expresión regular en `/usr/lib/cgi-bin/webmo/parse.cgi` (línea 101) solo permite letras, números, puntos y guiones bajos:

```perl
server => '^[a-zA-Z0-9_.]*$',
```

**Solución:** usar `quitex_server` (con guión bajo) como nombre de servidor en toda la configuración.

---

# Acceso remoto mediante tunnel SSH

El servidor está en la red de la UTN y no es accesible directamente desde internet en el puerto 80. El acceso se realiza mediante un tunnel SSH:

## 1º Paso: Abrir una terminal y ejecutar

```bash
ssh -L 8080:localhost:80 -p 11127 lautaro@190.114.201.4
```
> `lautaro` u otro usuario.

## 2º Paso: Acceder a la URL

Luego acceder desde el navegador a `http://localhost:8080/cgi-bin/webmo/login.cgi`.

---

## Configuración post-instalación (interfaz web)

Acceder a WebMO via tunnel SSH y entrar a:
```
http://localhost:8080/cgi-bin/webmo/login.cgi
```

### 1. Configurar cuenta admin

- Iniciar sesión como `admin` con contraseña en blanco
- Cambiar la contraseña → `quitex`

### 2. Configurar GAMESS (desde Interface Manager)

- Ir a **Interface Manager** → *Gamess* → **Enable interface**
- Ir a **Edit interface** y verificar:

| Campo | Valor |
|---|---|
| GAMESS Version | 2024 |
| GAMESS directory | `/usr/local/gamess` |
| GAMESS binary | `gamess.00.x` |
| Ddikick binary | `ddikick.x` |

- Si se requiere, hacer cambios necesarios (en este caso no se requirió).
- Click **Submit**

### 3. Crear usuario investigador (User Manager)

- Ir a **User Manager** → **New User**
- Username: `investigador`
- Password: `investigador`
- Click **Submit**

---

# Credenciales del sistema

| Rol | Usuario | Contraseña |
|---|---|---|
| Administrador WebMO | `admin` | `quitex` |
| Usuario WebMO | `investigador` | `investigador` |

---

## Verificación

Para confirmar que Apache y WebMO funcionan correctamente desde el servidor:

```bash
systemctl status apache2 | head -5
apache2ctl -M | grep cgi
```

La segunda línea debe mostrar `cgid_module (shared)`.