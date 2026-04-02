# Guía de uso de GAMESS a través de WebMO
**Servidor:** quitex_server | **IP:** 190.114.201.4 | **Puerto SSH:** 11127

*Fecha: 1 de abril de 2026*

---

## Documentación oficial

* [WebMO Documentation](https://www.webmo.net/support/documentation.html)
* [Installing WebMO on Linux](https://www.webmo.net/link/help/InstallingWebMO-Linux.html)
* [WebMO Tutorials](https://www.webmo.net/link/help/)

> En el último links se pueden encontrar los primeros pasos y tutoriales de uso de la herramienta, además de videos y otros documentos de ayúda práctica.

---

## 1. Acceso al servidor

WebMO está instalado en un servidor de la UTN que no es accesible directamente desde internet. Para acceder desde cualquier red externa es necesario establecer un **tunnel SSH**.

### Abrir el tunnel SSH

Desde una terminal, ejecutar:

```bash
ssh -L 8080:localhost:80 -p 11127 lautaro@190.114.201.4
```
> `lautaro` u otro usuario.

> [!IMPORTANT]
> Dejar esta terminal **abierta** durante toda la sesión de trabajo. Es normal que quede sin actividad visible — eso significa que el tunnel está funcionando.
> ***Si se cierra esta terminal, WebMO dejará de ser accesible.***

### Acceder a WebMO en el navegador

Con el tunnel activo, abrir el navegador e ingresar:

```
http://localhost:8080/cgi-bin/webmo/login.cgi
```

---

## 2. Inicio de sesión

En la pantalla de login ingresar las credenciales:

| Rol | Usuario | Contraseña |
|---|---|---|
| Administrador WebMO | `admin` | `quitex` |
| Usuario WebMO | `investigador` | `investigador` |

> Si necesitás acceso de administrador (para configurar interfaces, crear usuarios, etc.), usar `admin`.
> Si necesitás utilizar herramientas y realizar cálculos, usar `investigador`.

---

## 3. Ejecutar un cálculo con GAMESS

### Crear un nuevo trabajo

1. En el **Job Manager**, hacer click en **New Job** → **Create New Job**
2. Seleccionar **GAMESS** como motor de cálculo
3. Construir la molécula usando el editor gráfico integrado, o pegar directamente las coordenadas

### Configurar el cálculo

En la pantalla de configuración elegir:

- **Calculation:** tipo de cálculo (Energy, Optimize, Frequencies, etc.)
- **Theory:** método de cálculo (HF, DFT, MP2, etc.)
- **Basis Set:** base de funciones (STO-3G, 6-31G*, etc.)
- **Charge** y **Multiplicity** según la molécula

### Enviar el trabajo

Hacer click en **Submit Job**. El trabajo aparecerá en el Job Manager con estado **Running**.

---

## 4. Ver resultados

Una vez que el trabajo cambia a estado **Completed**:

1. Hacer click en el nombre del trabajo
2. WebMO mostrará los resultados principales: energía, geometría optimizada, frecuencias, etc.
3. Para ver el archivo de salida completo de GAMESS, hacer click en **Raw Output**

Si el estado es **Error**, hacer click en el trabajo y revisar el **Raw Output** para identificar el problema.

---

## 5. Tipos de cálculo disponibles

| Calculation | Descripción |
|---|---|
| Energy | Energía de un punto para la geometría dada |
| Optimize | Optimización de geometría (minimización de energía) |
| Frequencies | Frecuencias vibracionales (requiere geometría optimizada) |
| Optimize+Frequencies | Optimización seguida de cálculo de frecuencias |

---

## 6. Cerrar la sesión

Al terminar de trabajar:

1. Hacer click en **Logout** en WebMO
2. Cerrar la terminal con el tunnel SSH (o presionar `Ctrl+C`)

---

# Resumen rápido de pasos

```
1. Abrir terminal y ejecutar:   ssh -L 8080:localhost:80 -p 11127 lautaro@190.114.201.4
2. Abrir URL en el navegador:  http://localhost:8080/cgi-bin/webmo/login.cgi
3. Loguearse
```