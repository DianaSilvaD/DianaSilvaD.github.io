---
title: 1. Configuración del entorno | Infraestructura
date: 2025-05-19 00:00:00 -05:00
categories: [apt]
tags: [apt, virtual machine, wizard spider]
---

# 1. Configuración del entorno | Infraestructura

## Tabla de Contenidos
1. [Objetivos](#objetivos)
2. [Topología de Red](#topología-de-red)
3. [Requisitos Previos](#requisitos-previos)
4. [Configuración de Máquinas Virtuales](#configuración-de-máquinas-virtuales)
   - [Kali Linux 2019 - Attacker](#kali-linux-2019---attacker)
   - [Windows Server 2019 - Wizard (DC)](#windows-server-2019---wizard-dc)
   - [Windows 10 - Dorothy](#windows-10---dorothy)
   - [Windows 10 - Toto](#windows-10---toto)

---

## Objetivos

1. Crear un entorno aislado para emular TTPs del grupo Wizard Spider
2. Configurar dominio Active Directory con políticas de seguridad
3. Preparar estaciones de trabajo para simulación de ataques
4. Establecer conexión entre máquina atacante y red interna

---

## Topología de Red

| Componente         | IP Address     | Rol                   |
|--------------------|----------------|-----------------------|
| Kali Linux         | 192.168.0.4    | Máquina Atacante      |
| Windows Server     | 10.0.0.4       | Domain Controller     |
| Windows 10 (Dorothy)| 10.0.0.7      | Workstation 1 |
| Windows 10 (Toto)  | 10.0.0.8       | Workstation 2 |

---

## Requisitos previos
- Virtual Box
- Isos

    | SO                                | Rol                | Nombre |Enlace de descarga                                                                                       |
    |-----------------------------------|--------------------|--------|---------------------------------------------------------------------------------------------------------|
    | Kali Linux 2019                   | Atacante           |attacker|<https://old.kali.org/kali-images/kali-2019.1a/>                                                         |
    | Windows Server 2k19 - Build 17763 | Domain Controller  |wizard  |<https://archive.org/details/17763.3650.221105-1748.rs-5-release-svc-refresh-server-eval-x-64-fre-en-us> |
    | Windows 10 - Build 19042          | User machine 1     |dorothy |<https://archive.org/details/windows-10-insider-preview-client-x-64-en-us-19042>                         |
    | Windows 10 - Build 19042          | User machine 2     |toto    |<https://archive.org/details/windows-10-insider-preview-client-x-64-en-us-19042>                         |

---
## Configuración de Máquinas Virtuales
### **Kali Linux 2019 - Attacker**

#### Creación de maquina virtual en el Virtual Box

- Configuración de Hardware

    |Recurso       | Valor   |
    |--------------|---------|
    | Memoria base | 2048 MB |
    | Procesadores | 2 CPU   |
    | Disco        | 30 GB   |

- Acceder al botón crear en Virtula box
- Nombrar la VM y cargar la imagen ISO correspondiente
![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 15-54-32.png)

- Hardware asignado a la VM
![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 20-07-06.png)

- Disco duro
![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 20-07-10.png)

- Al terminar ir a configuración de la VM
    - Expert → **General** → Avanzado → Portapapales compartido : Bidirecional
    - Expert → **General** → Avanzado → Arrastrar y soltar : Bidirecional
    - Expert → **Sistema** → Placa base → Orden de arranque: [✓] Óptica [✓]  Disco duro [ ] Disquete [ ] Red 
    - Expert → **Sistema** → Aceleración → Interfaz de paravirtualización: Hyper-V

- Configuración de red
    - Expert → **Red** → Adapatdor 1 → Conectado a: Red NAT
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 21-51-47.png)

    - Expert → **Red** → Adapatdor 2 → Conectado a: Red interna
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 21-51-52.png)

#### Iniciar la VM para comenzar la instalación
- Seleccionar Graphical install (instalación grafica)
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 20-09-38.png)

- Nombrar a la VM - **attacker**
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 15-10-45.png)
    
- Partición de discos
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 15-12-16.png) 
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 15-12-30.png)
    
- Seleecionar el arranque GRUB 
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 16-13-19.png)    

- Comienza la instalación del disco
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 15-58-17.png)

- Termino de la intalación
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 16-13-50.png)


#### Verificar version de Kali linux    
```bash
    lsb_release -a
```
![figura](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-22 124140.png)
    
#### Hostname
```bash
    hostname
```
![figura](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 142812.png)

#### Asignar ip estatica **`192.168.0.4`** a eth1
- Verificar nombre de interfaz
```bash
ip a
```
- Editar la configuración:
```bash
sudo nano /etc/network/interfaces
```
- Modificar la IP
```bash
    auto lo
    iface lo inet loopback

    auto eth1
    iface eth1 inet static
        address 192.168.0.4
        netmask 255.255.255.0
```
![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 16-42-44.png)
            
#### Python
```bash
python3 --version
```
![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-22 163739.png)


#### Instalar Git
```bash
    sudo apt update
```
    
```bash
    sudo apt install git -y
```
    
```bash
    git --version
```
    
![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-22 125302.png)

#### Clonar Repositorio    
```bash
git clone https://github.com/center-for-threat-informed-defense/adversary_emulation_library.git
```

![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 23-42-19.png)


#### Acceder al repositorio clonado

- Ingresar a **`wizard_spider`** del repositorio clonado
        
    ```bash
        root@attacker:~/Descargas/adversary_emulation_library/wizard_spider#         
    ```
            
- Instalar **`pyminizip`** para descomprimir los archivos zip.
        
    ```bash
        pip3 install pyminizip
    ```
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 23-52-43.png)

--- 

### **Windows Server 2019 - Wizard (DC)**

#### Creación de maquina virtual en el Virtual Box

- Configuración de Hardware

    |Recurso       | Valor   |
    |--------------|---------|
    | Memoria base | 4096 MB |
    | Procesadores | 4 CPU   |
    | Disco        | 50 GB   |

    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 17-53-04.png)

#### Iniciar la VM para comenzar la instalación
- Seleccionar Windows Server 2019 Datacenter Evaluation (Desktop Experience) para poder tener acceso al servidor de manera grafica

    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 17-55-15.png)  


- Instalando Windows

    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 17-56-10.png)  


- Asignar contraseña para el administrador
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 17-58-28.png)  


- Ejecutar VBoxWindowsAdditions para mejorar la integración entre el host y VirtualBox
    ![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-02-05.png)  


#### Verificar version Windows Server 2k19   
```bash
    winver
```
![figura](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 185538.png)

#### Hostname
```bash
    hostname
```
![figura](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 172831.png)

#### Asignar ip estatica **`10.0.0.4`**

![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 205026.png)
            
#### Promover servidor a controlador de dominio
- Agregar roles y caracteristicas al servidor
- Proceder a la instalación
    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 212504.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 212511.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 212628.png)   
    
    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 212737.png)
    
    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 212818.png)
        
    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 212842.png)
        
    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 213214.png)
        
    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 213325.png)

- Promover el servidor a servidor de dominio
    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 213450.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 213500.png)

- Asistente de configuración de servicios de dominio de Active Directory

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 214311.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 214852.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 214915.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 214943.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 215032.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 215058.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 215121.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 215159.png)

    ![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 215228.png)


#### Consultar qué controlador de dominio tiene cada uno de los roles FSMO (Flexible Single Master Operations) en un entorno de Active Directory.

![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 233112.png)

#### Hacer ping desde el mismo servidor de dominio

![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 233455.png)

---

### **Windows 10 - Dorothy**

#### Creación de maquina virtual en Virtual Box

- Configuración de Hardware

    |Recurso       | Valor   |
    |--------------|---------|
    | Memoria base | 2048 MB |
    | Procesadores | 2 CPU   |
    | Disco        | 40 GB   |
    | Hostname     | dorothy |

- Asignar nombre e imagen ISO
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-12-05.png)

- Hardware
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-13-40.png)

- Disco duro
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-13-47.png)

- Maquina creada
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-14-06.png)


#### Iniciar la VM para comenzar la instalación
![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-18-50.png)

![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-20-50.png)

![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-21-39.png)

![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-21-57.png)


#### Verificar version de Windows 10 - Build 19042 
![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-07 01-32-58.png)

#### Hostname
![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 235327.png)

![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-07 01-36-30.png)

#### Asignar ip estatica **10.0.0.7**
- Ingresar al Panel de control
- Network and Internet
- Network an Sharing Center
- Change adapter settings
- Propiedades del Ethernet
- Internet Protocol Version 4 (TCP/IPv4)
- Asignar la ip del controlador de dominio

    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-07 01-39-47.png)

#### Asignar el dominio **oz.local** 

![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-23 235342.png)

- Comprobar la conexión con el controlador de dominio asignado
![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-24 001537.png)

--- 

### **Windows 10 - Toto**

#### Creación de maquina virtual en Virtual Box
- Configuración de Hardware

    |Recurso       | Valor   |
    |--------------|---------|
    | Memoria base | 2048 MB |
    | Procesadores | 2 CPU   |
    | Disco        | 40 GB   |
    | Hostname     | toto    |

- Asignar nombre e imagen ISO
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-24-04.png)

- Hardware
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-24-11.png)

- Disco duro
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-24-15.png)

- Maquina creada
    ![figura](/assets/images/2025-05-19-s05/Captura desde 2025-06-04 18-24-29.png)

#### Iniciar la VM para comenzar la instalación
#### Verificar version de Windows 10 - Build 19042 
#### Hostname

#### Asignar ip estatica **10.0.0.8**
- Ingresar al Panel de control
- Network and Internet
- Network an Sharing Center
- Change adapter settings
- Propiedades del Ethernet
- Internet Protocol Version 4 (TCP/IPv4)
- Asignar la ip del controlador de dominio
![image.png](/assets/images/2025-05-19-s05/Captura desde 2025-06-06 16-54-18.png)


#### Asignar el dominio **oz.local**
![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-24 001904.png)

![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-24 001949.png)

![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-24 002009.png)

![image.png](/assets/images/2025-05-19-s05/Captura de pantalla 2025-05-24 002302.png)

