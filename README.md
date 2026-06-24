# winturbo

![Última release](https://img.shields.io/github/v/release/unjordi/winturbo?label=%C3%BAltima%20release&color=2563eb)
![Descargas](https://img.shields.io/github/downloads/unjordi/winturbo/total?label=descargas&color=22c55e)
![Windows 11](https://img.shields.io/badge/Windows%2011-0078D6?logo=windows&logoColor=white)
![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?logo=powershell&logoColor=white)

Optimizador, *debloat* y endurecimiento de privacidad para **Windows 11**: menú interactivo por secciones (**aplicar · desaplicar · no tocar**), reporte por corrida y un modo diagnóstico de **solo lectura**. Pensado para correrse en cada instalación nueva de Windows.

> **Filosofía: limpia, no rompe.** Todo lo reversible, con respaldo, y guardias que evitan dejarte fuera de tu sesión.

## Qué hace

Cada sección se elige por separado en el menú. A grandes rasgos:

- **Privacidad y telemetría** — corta el rastreo (DiagTrack, CEIP, autologgers ETW, ~20 tareas), permisos de apps, geolocalización, Copilot/Recall.
- **Cámara y micrófono** — control de acceso de apps (reversible; no bloquea apps de videollamada por sorpresa).
- **Bloatware** — desinstala apps de relleno preinstaladas. **Conserva** Teams, Paint, Bloc de notas y Recortes.
- **Apps de empresa (opt-in)** — desinstala Teams y Skype Empresarial **solo si lo eliges** (apagado por default).
- **Servicios, búsqueda indexada, componentes opcionales (DISM)** — apaga lo innecesario.
- **Tareas programadas** — elimina tareas de bloatware; opcionalmente instala un **guardián** que las mantiene borradas tras cada Windows Update.
- **UI / Modo oscuro / OneDrive** — limpia el Explorer y la barra de tareas, tema oscuro, deja OneDrive inactivo.

## Uso sin descarga manual

Siempre la última versión, sin guardar nada — PowerShell **como administrador**:

```powershell
irm https://github.com/unjordi/winturbo/releases/latest/download/winturbo.ps1 | iex
```

No requiere tocar la política de ejecución.

## Uso en PowerShell offline (con auto-actualización si hay red)

Baja `winturbo.ps1` de la [última release](https://github.com/unjordi/winturbo/releases/latest) para tenerlo en disco.

Primero, **una sola vez**, permite ejecutar scripts:

```powershell
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```

(Alternativa por corrida: lanzar con `-ExecutionPolicy Bypass`.)

Luego córrelo:

```powershell
.\winturbo.ps1
```

Funciona sin conexión; **si hay red**, al arrancar revisa si hay una versión más nueva y **te ofrece actualizarse** solo (la baja y se relanza).

## Modos y opciones

```powershell
.\winturbo.ps1                     # menú interactivo (grid por secciones)
.\winturbo.ps1 -Mode Report        # diagnóstico de SOLO LECTURA (no cambia nada)
.\winturbo.ps1 -Mode Apply         # aplica todo, sin menú
.\winturbo.ps1 -Mode Undo          # revierte todo
.\winturbo.ps1 -Preset VM          # preajuste para máquina virtual
.\winturbo.ps1 -Preset Hardware    # preajuste para Windows instalado
.\winturbo.ps1 -NoUpdate           # omite el chequeo de actualización
.\winturbo.ps1 -InstallTaskGuardian  # (no interactivo) instala el guardián de tareas
```

## Reversible

Todo lo que aplica se puede **desaplicar**: en el menú, marca la columna **Desaplicar**, o corre con `-Mode Undo`. Además, antes de aplicar respalda en un `.reg` las claves de registro que toca.

## Seguridad — "limpia, no rompe"

- **`-Mode Report`** no modifica nada: solo reporta el estado actual de cada parámetro que el script puede mover (a un archivo propio con timestamp).
- **Nunca te deja fuera de tu sesión.** La sección de biometría solo se aplica si detecta una **contraseña** de respaldo; si la cuenta solo tiene PIN, **no toca nada** (Windows Hello es monolítico y desactivarlo se llevaría el PIN).
- **Guardián de tareas opt-in.** No se instala sin preguntar; corre con **tus permisos** (no como SYSTEM) y con el código incrustado en la tarea (sin dejar archivos sueltos).
- **Confirmación deliberada.** Antes de aplicar sale un diálogo de confirmación; las acciones consecuentes piden un gesto explícito (interruptor + botón).
- **Respaldo.** Antes de aplicar, exporta a `.reg` las claves que modifica. Cada corrida deja un reporte en `%USERPROFILE%`.
