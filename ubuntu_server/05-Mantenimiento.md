# Mantenimiento

## **Configuración para acceso gráfico remoto (Opcional)**
Si los usuarios necesitan interactuar con la interfaz gráfica de **CUDA** o **Amber**, puedes configurar acceso remoto gráfico con **X2Go** o **NoMachine**.

### X2Go:
1. Instala el servidor de X2Go en el servidor:

```bash
sudo apt install x2goserver x2goserver-xsession
```

2. En las máquinas cliente, descarga e instala **X2Go Client** desde [aquí](https://wiki.x2go.org/doku.php/download:start).

3. Conéctate al servidor con el cliente **X2Go** especificando la dirección IP y las credenciales de usuario.

### NoMachine:
1. Descarga e instala **NoMachine** en el servidor desde [aquí](https://www.nomachine.com/download).
2. Instala el cliente en las máquinas que se conectarán remotamente.

----

## **Mantenimiento y monitoreo**
### Monitoreo de hardware (NVIDIA)
Para monitorear la GPU en tiempo real:

```bash
watch -n 1 nvidia-smi
```

### Actualizaciones
Es importante mantener el servidor actualizado para recibir parches de seguridad y actualizaciones de los controladores:

```bash
sudo apt update && sudo apt upgrade -y
```
