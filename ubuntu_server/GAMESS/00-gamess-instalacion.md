# Instalación de GAMESS
**Servidor:** quitex-server | **SO:** Ubuntu 24.04 | **CPU:** Intel Xeon E5-2623 v3 (16 cores) | **RAM:** 56 GB | **GPU:** NVIDIA GTX TITAN X

*Fecha: 1 de abril de 2026*

---

## Documentación oficial

* [WebMO Documentation](https://www.webmo.net/support/documentation.html)
* [Installing WebMO on Linux](https://www.webmo.net/link/help/InstallingWebMO-Linux.html)
* [GAMESS Installation Instructions for Linux](https://www.webmo.net/support/gamess_linux.html)
* [WebMO Tutorials](https://www.webmo.net/link/help/)

> En el último links se pueden encontrar los primeros pasos y tutoriales de uso de la herramienta, además de videos y otros documentos de ayúda práctica.

---

## 1. Dependencias

```bash
sudo apt-get install csh gfortran patch cmake tcsh
```

> **Importante:** Instalar `tcsh` explícitamente. En Ubuntu 24.04, el comando `csh` apunta por defecto a `bsd-csh`, que es incompatible con el script de GAMESS. Ver sección de modificaciones.

---

## 2. Descarga del código fuente

Solicitar acceso en la [página oficial de GAMESS](https://www.msg.chem.iastate.edu/GAMESS/download/source/). Se recibirá una contraseña por email. Luego ejecutar:

```bash
curl -k -O --user source:CONTRASEÑA https://www.msg.chem.iastate.edu/GAMESS/download/source/gamess-current.tar.gz
```

> La contraseña cambia cada semana, si se requiere otra, hay que volver a solicitar.

---

## 3. Descompresión e instalación

```bash
sudo su
cd /usr/local
tar xzf /home/lautaro/gamess-current.tar.gz
chown -R root:root gamess
chmod -R g-s gamess
```

---

## 4. Configuración

```bash
cd /usr/local/gamess
./config
```

Respuestas al asistente interactivo:

| Pregunta | Respuesta |
|---|---|
| Machine type | `linux64` |
| Installation / build directory | *(Enter — default)* |
| Version number | `00` |
| HPC system target | `none` |
| FORTRAN compiler | `gfortran` |
| Math libraries | `none` |
| Communication | `sockets` |
| LibXC, MDI, CCT3, LIBCCHEM, etc. | `no` |

---

## 5. Compilación de LAPACK

```bash
tools/lapack/download-lapack.csh
make -j lapack >& lapack.log
```

---

## 6. Compilación de DDI

```bash
cd ddi
./compddi >& compddi.log
mv ddikick.x ..
cd ..
```

---

## 7. Compilación de GAMESS (~20 min)

```bash
./compall >& compall.log
```

Monitorear progreso desde otra terminal:
```bash
tail -f /usr/local/gamess/compall.log
```

Si el proceso se detiene esperando confirmación (e.g. `cp: overwrite 'constants.F90'?`), responder `y` + Enter.

---

## 8. Linking

```bash
./lked gamess 00 >& lked.log
```

---

## 9. Configuración del script `rungms`

```bash
sudo vi /usr/local/gamess/rungms
```

Editar exactamente estas líneas:

```csh
set SCR=/tmp
set USERSCR=/tmp
set GMSPATH=/usr/local/gamess
```

---

## 10. Verificación de logs

```bash
grep "cannot stat" compall.log
tail -n 20 ddi/compddi.log
tail -n 20 lked.log
exit
```

Sin errores = compilación exitosa.

---

## 11. Test de funcionamiento

```bash
cd ~
mkdir gamess
cp -p /usr/local/gamess/tests/standard/exam01.inp ~/gamess
cd ~/gamess
tcsh /usr/local/gamess/rungms exam01 00 1 1 0 > exam01.log 2>&1
tail -n 20 exam01.log
```

Resultado esperado al final del log:
```
...
ddikick.x: exited gracefully.
...
```

> Para saber qué es cada parámetro del comando, ver [`01-gamess-uso.md`](./01-gamess-uso.md) para entender la estructura.

---

## Modificaciones realizadas respecto al tutorial oficial

### 1. Reemplazar `bsd-csh` por `tcsh`

El tutorial asume que `csh` es `tcsh`, pero en Ubuntu 24.04 el comando `csh` enlaza a `bsd-csh` (implementación mínima incompatible con la sintaxis del script `rungms`).

**Solución aplicada:**

```bash
sudo update-alternatives --config csh
# Seleccionar la opción que apunta a /bin/tcsh
```

Y ejecutar siempre GAMESS con `tcsh` explícito:

```bash
tcsh /usr/local/gamess/rungms exam01 00 1 1 0 > exam01.log 2>&1
```

### 2. Variable de entorno `CONDA_PROMPT_MODIFIER`

Conda exporta al entorno la variable `CONDA_PROMPT_MODIFIER=(base)`, cuyos paréntesis son ilegales para `csh`/`tcsh`. Esto causaba el error `Illegal variable name` al intentar correr el script.

La solución definitiva fue usar `tcsh` (punto 1) y pasar los argumentos completos al script, lo que evitó el conflicto.

---

## Notas adicionales

- Para correr GAMESS con múltiples núcleos, cambiar el argumento `NCPUS` (tercer parámetro de `rungms`).
- Si aparece el error `DDI Process 0: error code 911`, aumentar la memoria compartida del sistema:

```bash
sudo su
echo 17179869184 > /proc/sys/kernel/shmmax
echo "kernel.shmmax=17179869184" >> /etc/sysctl.conf
```