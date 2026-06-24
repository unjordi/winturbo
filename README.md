# winturbo

![Última release](https://img.shields.io/github/v/release/unjordi/winturbo?label=%C3%BAltima%20release&color=2563eb)
![Descargas](https://img.shields.io/github/downloads/unjordi/winturbo/total?label=descargas&color=22c55e)
![Windows 11](https://img.shields.io/badge/Windows%2011-0078D6?logo=windows&logoColor=white)
![PowerShell](https://img.shields.io/badge/PowerShell-5391FE?logo=powershell&logoColor=white)

Optimizador, *debloat* y endurecimiento de privacidad para **Windows 11**: menú interactivo por secciones (**aplicar · desaplicar · no tocar**), reporte por corrida y un modo diagnóstico de **solo lectura**. Pensado para correrse en cada instalación nueva de Windows.

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

## Modos

```powershell
.\winturbo.ps1                # menú interactivo (grid por secciones)
.\winturbo.ps1 -Mode Report   # diagnóstico de SOLO LECTURA (no cambia nada)
.\winturbo.ps1 -NoUpdate      # omite el chequeo de actualización
```

## Seguridad

- `-Mode Report` no modifica nada: solo reporta el estado actual de los parámetros que el script puede mover.
- Antes de aplicar, respalda en un `.reg` las claves de registro que toca.
- Cada corrida deja un reporte en `%USERPROFILE%`.
