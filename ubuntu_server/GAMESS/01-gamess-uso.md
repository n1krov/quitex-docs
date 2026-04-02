# Guía de uso de GAMESS
**Servidor:** quitex_server | **Ruta de instalación:** `/usr/local/gamess`

*Fecha: 1 de abril de 2026*

GAMESS (General Atomic and Molecular Electronic Structure System) es un programa de química computacional que permite calcular estructuras electrónicas de moléculas mediante métodos ab initio, DFT, semi-empíricos, entre otros.

---

## Recursos

- [Página oficial de GAMESS](https://www.msg.chem.iastate.edu/gamess/)
- [Manual oficial de GAMESS](https://www.msg.chem.iastate.edu/gamess/documentation.html)
- [Ejemplos de inputs](https://www.msg.chem.iastate.edu/gamess/GAMESS_Manual/input.pdf)

---

## Cómo ejecutar un cálculo

### Estructura básica del comando

```bash
tcsh /usr/local/gamess/rungms NOMBRE VERNO NCPUS PPN LOGN > NOMBRE.log 2>&1
```

| Argumento | Descripción | Valor habitual |
|---|---|---|
| `NOMBRE` | Nombre del archivo de entrada (sin `.inp`) | e.g. `calculo01` |
| `VERNO` | Versión del ejecutable | `00` |
| `NCPUS` | Número de procesos de cómputo | `1` (un núcleo) |
| `PPN` | Núcleos por nodo físico | `1` |
| `LOGN` | Núcleos por nodo lógico | `0` (automático) |

### Ejemplo típico

```bash
cd ~/gamess
tcsh /usr/local/gamess/rungms mi_molecula 00 1 1 0 > mi_molecula.log 2>&1
```

### Ejemplo con múltiples núcleos (paralelismo)

```bash
tcsh /usr/local/gamess/rungms mi_molecula 00 8 8 0 > mi_molecula.log 2>&1
```

---

## Estructura de un archivo de entrada (`.inp`)

```
 $CONTRL SCFTYP=RHF RUNTYP=ENERGY $END
 $BASIS  GBASIS=STO NGAUSS=3 $END
 $DATA
Nombre de la molécula
C1
C   6.0   0.0  0.0  0.0
H   1.0   0.6  0.6  0.6
 $END
```

Los bloques principales son:

- **`$CONTRL`** — tipo de cálculo (`RUNTYP`) y método (`SCFTYP`)
- **`$BASIS`** — base de funciones a usar
- **`$DATA`** — geometría de la molécula (símbolo, número atómico, coordenadas X Y Z)

### Tipos de cálculo (`RUNTYP`) más comunes

| Valor | Descripción |
|---|---|
| `ENERGY` | Energía de un punto |
| `OPTIMIZE` | Optimización de geometría |
| `HESSIAN` | Frecuencias vibracionales |
| `GRADIENT` | Gradiente de energía |

### Métodos (`SCFTYP`) más comunes

| Valor | Descripción |
|---|---|
| `RHF` | Hartree-Fock capa cerrada |
| `UHF` | Hartree-Fock capa abierta |
| `ROHF` | Hartree-Fock capa abierta restringida |
| `MCSCF` | Multiconfiguracional |

---

## Archivos generados

Todos los archivos temporales se crean en `/tmp/` y son eliminados al finalizar el cálculo. Los archivos que persisten son:

| Archivo | Contenido |
|---|---|
| `NOMBRE.log` | Log completo del cálculo (resultado principal) |
| `/tmp/NOMBRE.dat` | Datos de punch (orbitales, gradientes, etc.) |

---

## Verificar si el cálculo terminó correctamente

```bash
tail -n 30 mi_molecula.log
```

Buscar al final del archivo:
- ✅ `ddikick.x: exited gracefully.` — cálculo exitoso
- ❌ `DDI Process 0: error code 911` — error de memoria o entrada incorrecta

---

## Si el cálculo falla y se quiere reintentar

Limpiar archivos temporales antes de volver a correr:

```bash
rm -f /tmp/mi_molecula*
rm -f ~/gamess/mi_molecula.log
```

---

## Archivos de ejemplo

GAMESS incluye casos de prueba listos para usar en:

```
/usr/local/gamess/tests/standard/
```

Para copiar y correr uno:

```bash
cp /usr/local/gamess/tests/standard/exam01.inp ~/gamess/
cd ~/gamess
tcsh /usr/local/gamess/rungms exam01 00 1 1 0 > exam01.log 2>&1
```