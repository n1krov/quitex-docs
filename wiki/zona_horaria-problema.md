
>[!NOTE]
>El servidor no tiene salida a los servidores ntp, no se por que...
>Puede ser un firewall quizas.

### **1. Configurar la hora manualmente (solución inmediata)**
Si no hay conexión a Internet, ajusta la hora manualmente:

#### **Opción A: Usar `timedatectl` (recomendado)**
```bash
sudo timedatectl set-time "2025-07-08 15:30:00"
```
Reemplaza `15:30:00` con la hora actual.  
Verifica:
```bash
timedatectl
```

#### **Opción B: Usar `date` (alternativa)**
```bash
sudo date -s "15:30:00 08 Jul 2025"
```

#### **Opción C: Sincronizar con el reloj de hardware (RTC)**
Para evitar que la hora se pierda al reiniciar:
```bash
sudo hwclock --systohc  # Guarda la hora del sistema en el RTC
sudo hwclock --hctosys  # Opcional: Carga la hora del RTC al sistema
```

---

### **2. Desactivar NTP (para evitar intentos fallidos)**
Si no hay Internet, desactiva el servicio NTP para que no consuma recursos:
```bash
sudo timedatectl set-ntp off
```
Verifica:
```bash
timedatectl
```
Debe mostrar `NTP service: inactive`.

---

### **3. Solución para redes aisladas (sin Internet pero con LAN)**
Si tienes otros equipos en la red local, configura uno como servidor NTP local:

#### **En el equipo con hora correcta (servidor NTP):**
1. Instala `chrony`:
   ```bash
   sudo apt install chrony
   ```
2. Edita `/etc/chrony/chrony.conf` y añade:
   ```ini
   allow 192.168.1.0/24  # Ajusta según tu red
   local stratum 10
   ```
3. Reinicia:
   ```bash
   sudo systemctl restart chrony
   ```

#### **En tu servidor (cliente NTP):**
1. Configura `/etc/chrony/chrony.conf` para apuntar al servidor local:
   ```ini
   server 192.168.1.100 iburst  # Reemplaza con la IP del servidor NTP local
   ```
2. Reinicia:
   ```bash
   sudo systemctl restart chrony
   ```

---

### **4. Verificar el estado final**
```bash
timedatectl
```
- **Si configuraste manualmente**:  
  - `System clock synchronized: no` (normal, sin NTP).  
  - La hora debe ser la que estableciste.  
- **Si usas un servidor NTP local**:  
  - `System clock synchronized: yes`.  

---

### **¿Por qué ocurre esto?**
- **Sin Internet**: Chrony no puede alcanzar servidores NTP públicos.  
- **RTC incorrecto**: Si el reloj de hardware (BIOS) está mal, la hora se pierde al reiniciar.  
- **Firewall/DNS**: Bloquean conexiones NTP (aunque en tu caso es por falta de Internet).  

### **Conclusión**
1. **Ajusta la hora manualmente** con `timedatectl` o `date`.  
2. **Desactiva NTP** si no hay Internet (`sudo timedatectl set-ntp off`).  
3. **Configura un servidor NTP local** si tienes otros equipos en la red.  
