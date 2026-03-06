# **🐺 Proyecto Wolfdale: Laboratorio Híbrido & Diskless Cluster**

**Proyecto Wolfdale** es una arquitectura de "Homelab" diseñada para reciclar hardware antiguo de manera ultra-eficiente. Divide los servicios del hogar (IoT, NVR, Impresión 3D) y los experimentos retro (Impresión Matricial por Puerto Paralelo) en dos nodos especializados, operando bajo un modelo **Maestro / Esclavo Diskless**.

## **🏗️ Arquitectura del Sistema**

El sistema se compone de dos nodos físicos que interactúan a través de una red LAN dedicada.

### **🧠 Nodo A: Servidor Maestro (Producción 24/7)**

Actúa como cerebro de la red, servidor de almacenamiento, Gateway IoT y proveedor del sistema operativo (PXE/NFS) para el Nodo B.

* **Hardware:** Notebook Intel Core i5 desarmada (Flat Case), 8GB RAM.  
* **Energía:** Batería interna actuando como UPS nativa (\~10-15W de consumo).  
* **Servicios (Docker):**  
  * MotionEye: Grabación de cámaras de seguridad (NVR).  
  * Node-Red & Mosquitto: Lógica y broker MQTT para red IoT.  
  * OctoPrint: Gestión de Impresora 3D vía USB.  
  * Portainer: Gestión de contenedores.  
* **Servicios (Nativos):**  
  * CUPS: Servidor de impresión para Epson Color USB.  
  * Hostapd & Dnsmasq: Creación de red WiFi IoT (Access Point) y Gateway.  
  * NFS & TFTP: Servidor de arranque por red para el Nodo B.

### **🧟 Nodo B: Estación Retro "Wolfdale" (Diskless / Bajo Demanda)**

Un cliente "tonto" (Diskless) sin almacenamiento local. Su único propósito es exponer hardware *legacy* (Puerto Paralelo) a la red moderna.

* **Hardware:** Motherboard ASRock Wolfdale1333, CPU Core 2 Duo, 3GB RAM.  
* **Almacenamiento:** Ninguno (Boot por Red PXE).  
* **OS:** Debian 12 minbase (Carga diferida / Lazy Loading desde el Nodo A).  
* **Servicio:** netcat escuchando en puerto 9100, retransmitiendo en crudo (raw) al /dev/parport0 (Impresora de Matriz de Punto).

## **🚀 Plan de Implementación (Hoja de Ruta)**

### **FASE 1: Preparación del Servidor Maestro (Nodo A)**

1. **Instalación Base:**  
   * Instalar Debian 12 (Netinst) en la Notebook i5.  
   * Configurar /etc/systemd/logind.conf para ignorar el cierre de tapa (HandleLidSwitch=ignore).  
2. **Configuración de Red (Gateway):**  
   * Asignar IP estática a eth0 (Ej: 192.168.1.10).  
   * Configurar wlan0 con hostapd para crear la red WiFi Red\_IoT\_Lab.  
   * Habilitar IP Forwarding (NAT) mediante iptables.  
3. **Despliegue de Docker:**  
   * Instalar Docker y Docker Compose.  
   * Levantar el stack (docker-compose.yml) con MotionEye, Node-Red, Mosquitto y OctoPrint.  
4. **Impresión Convencional:**  
   * Instalar cups y configurar la impresora Epson Color por USB.

### **FASE 2: Creación del Sistema Diskless (El "Alma" del Wolfdale)**

El Nodo B no tiene disco, por lo que su sistema operativo vivirá dentro del Nodo A.

1. **Instalación de Servicios PXE:**  
   * Instalar nfs-kernel-server y dnsmasq en el Nodo A.  
2. **Construcción del OS (Debootstrap):**  
   * Crear el sistema de archivos raíz en /srv/nfs/wolfdale.  
   * Ejecutar: sudo debootstrap \--arch amd64 \--variant=minbase bookworm /srv/nfs/wolfdale  
3. **Configuración interna del Chroot:**  
   * Entrar al chroot, instalar el Kernel (linux-image-amd64), netcat-openbsd y configurar el fstab para montar la raíz vía NFS.  
   * Configurar un script en rc.local para iniciar la escucha del puerto LPT:  
     nc \-lk \-p 9100 \> /dev/parport0 &  
4. **Exportar y Servir:**  
   * Exportar /srv/nfs/wolfdale vía NFS.  
   * Copiar vmlinuz e initrd.img a /srv/tftp y configurar el menú de arranque pxelinux.0.

### **FASE 3: Modding Físico y Hardware**

1. **Notebook (Nodo A):** \- Desmontar pantalla y plásticos rotos.  
   * Montaje plano ("Flat Case") sobre acrílico/madera.  
2. **Wolfdale (Nodo B):**  
   * Retirar HDD/SSD, GPU Nvidia (para ahorrar 30W) y cables SATA.  
   * Modificación de la Fuente ATX: Puenteo del pin 16 (Verde) con GND (Negro) para arranque autónomo, o uso de PicoPSU si se agrupan placas en el futuro.  
   * Montaje de la placa base cerca de la impresora matricial.  
3. **BIOS Wolfdale:** \- Habilitar Onboard LAN Boot ROM (PXE).  
   * Configurar Puerto Paralelo en modo ECP+EPP.

### **FASE 4: Operación y Flujo de Trabajo**

* **Uso Diario:** El Nodo A opera 24/7 gestionando IoT, cámaras e impresión 3D de forma silenciosa y eficiente.  
* **Uso Retro:** Cuando se necesita imprimir en la Matriz de Punto o hacer pruebas LPT:  
  1. Se enciende físicamente el Wolfdale.  
  2. El Wolfdale arranca por red (PXE) en \~30 segundos.  
  3. Gracias a sus 3GB de RAM, los comandos se cachean localmente (Lazy Loading).  
  4. Se envían trabajos de impresión desde cualquier PC apuntando al puerto 9100 de la IP del Wolfdale.  
  5. Se apaga cortando la energía directamente (Sistema Stateless/Diskless robusto).

## **🛠️ Herramientas y Tecnologías Utilizadas**

* **OS:** Debian 12 (Bookworm)  
* **Red:** Hostapd, Dnsmasq, Iptables  
* **Boot Remoto:** PXE, TFTP, NFS (Network File System), Debootstrap  
* **Contenedores:** Docker, Portainer  
* **Hardware Hack:** ATX Pin bridging, Headless operations, Raw LPT port mapping.

*Documento generado para el Proyecto Wolfdale. Febrero 2026\.*