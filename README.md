# **script-altahost**

Script para dar alta a UBUNTU/DEBIAN/PVE/PBS en base a necesidad de Lunix SRL, **pero se puede usar de forma generica para cualquier cliente**

**Verificado en:**

```bash
Cloud/Debian9+
Cloud/Ubuntu20.04+
Ubuntu+Docker
PVE v6+
PBS v1+
```

## **Instalación** 🔧

* Primero: Instalar las dependencias necesarias

```bash
apt-get update; apt-get install -y git screen
```

* Segundo: Ejecutar screen para correr en segundo plano

```bash
screen
```

* Tercero: Ejecutar script

```bash
git clone https://github.com/avillalba96/script-altahost && cd "$(basename "$_" .git)" && cd install && ./alta.lunixstart.sh
```

## **Pasos Opcionales**

### **Actualización del banner**

Cuando se actualizan los paquetes de PVE/PBS estos sobreescriben nuestro banner de bienvenida, por lo tanto en caso de mantenerlo es necesario volver a bajarlo:

```bash
### BANNER LUNIX
# PVE
wget https://raw.githubusercontent.com/avillalba96/script-altahost/main/install/systemd/pvebanner-service -O /usr/bin/pvebanner && chmod +x /usr/bin/pvebanner
sed -i "s/.data.status.toLowerCase() !==/.data.status.toLowerCase() ==/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
sed -i "s/www.proxmox.com/www.lunix.com.ar/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
wget https://github.com/avillalba96/script-altahost/raw/main/install/images/proxmox_logo.png -O /usr/share/pve-manager/images/proxmox_logo.png
systemctl restart pvebanner.service
systemctl restart pveproxy.service
# PBS
wget https://raw.githubusercontent.com/avillalba96/script-altahost/main/install/systemd/pbsbanner-service -O /usr/lib/x86_64-linux-gnu/proxmox-backup/proxmox-backup-banner && chmod +x /usr/lib/x86_64-linux-gnu/proxmox-backup/proxmox-backup-banner
sed -i "s/.data.status.toLowerCase() !==/.data.status.toLowerCase() ==/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
sed -i "s/www.proxmox.com/www.lunix.com.ar/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
wget https://github.com/avillalba96/script-altahost/raw/main/install/images/proxmox_logo.png -O /usr/share/javascript/proxmox-backup/images/proxmox_logo.png
systemctl restart proxmox-backup-banner.service
systemctl restart proxmox-backup-proxy.service


### BANNER GENERICO
# PVE
wget https://raw.githubusercontent.com/avillalba96/script-altahost/main/install/systemd/pvebanner-service_example -O /usr/bin/pvebanner && chmod +x /usr/bin/pvebanner && systemctl restart pvebanner.service
# PBS
wget https://raw.githubusercontent.com/avillalba96/script-altahost/main/install/systemd/pbsbanner-service_example -O /usr/lib/x86_64-linux-gnu/proxmox-backup/proxmox-backup-banner && chmod +x /usr/lib/x86_64-linux-gnu/proxmox-backup/proxmox-backup-banner && systemctl restart proxmox-backup-banner.service
```

### **RAID ZFS *(configuracion basica, machete)***

Se deja a mano los comandos aplicados sobre local-zfs

```bash
zfs set reservation=20G rpool/ROOT
zfs set dedup=off rpool
zfs set compression=lz4 rpool
zfs set sync=disabled rpool
pvesm set local-zfs --blocksize 128k
pvesm set local-zfs --sparse 1
```

### **Autores** ✒️

* **Maxi** - [Maximiliano Baez](https://github.com/MaximilianoBz)
* **Franco** - [Franco Grismado](https://github.com/fgrismado)
* **Pablito** - [Pablo Ramos](https://github.com/avillalba96)
* **Alejandro** - [Alejandro Villalba](https://github.com/avillalba96)

### **TO-DO** 📦

* Agregar alta con wireguard y dialog <https://github.com/avillalba96/mkt-wireguard_init>
* Verficiar porque no se genera /var/log/syslog *(proxmox sabemos que no se genera)*
* Eliminar total referencia de lunix y/o similares, volverlo lo mas generico posible, usar usuario o demas como "altahost"
* Acomodar el puerto que define SSH, que reemplaze cualquier existente, actualmente a veces agrega 2 lineas a sshd_config
