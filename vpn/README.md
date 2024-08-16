# Inicalizacion

Para conectarte al servidor de la facultad necesitas conectarte por la vpn con openvpn y el .ovpn que te proporciona la facultad.

Para conectarte a la vpn con openvpn, primero debes instalar openvpn en tu sistema operativo. Luego, debes ejecutar el siguiente comando en la terminal:

```bash
sudo openvpn anpetelski-config.ovpn
```

Donde `anpetelski-config.ovpn` es el archivo que permite la conexion a la vpn.

Te pedira un usuario y una contraseña.

Una vez que te conectes a la vpn, puedes conectarte al servidor de la facultad con el siguiente comando:

```bash
ssh nicolai@10.13.0.127
```

Luego la contraseña.
> Nota: Las credenciales debes tenerlas a mano para poder iniciar sesion.

Listo, estaras conectado al servidor de la facultad.
