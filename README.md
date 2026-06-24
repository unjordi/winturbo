# winturbo

Distribución pública de **`windows_turbo_script.ps1`** — optimizador / debloat / privacidad para **Windows 11**: menú interactivo por secciones (aplicar · desaplicar · no tocar), reporte por corrida y un modo diagnóstico de **solo lectura**.

> El código fuente se mantiene en un repo privado. Aquí solo se publica el script ya listo, como *release asset*, para poder arrancarlo y autoactualizarlo. La versión sigue el formato de fecha `aa.mm.dd`.

## Uso recomendado: archivo local con auto-actualización

1. Descarga el `.ps1` de la **[última release](https://github.com/unjordi/winturbo/releases/latest)**.
2. Córrelo en **PowerShell como administrador** (el script se autoeleva si hace falta):

   ```powershell
   .\windows_turbo_script.ps1
   ```

Al arrancar revisa si hay una versión más nueva publicada aquí y **te ofrece actualizarse** solo (baja la nueva y se relanza).

## Arranque "siempre la última" (sin guardar nada)

```powershell
irm https://github.com/unjordi/winturbo/releases/latest/download/windows_turbo_script.ps1 | iex
```

## Modos

```powershell
.\windows_turbo_script.ps1                # menú interactivo (grid por secciones)
.\windows_turbo_script.ps1 -Mode Report   # diagnóstico de SOLO LECTURA (no cambia nada)
.\windows_turbo_script.ps1 -NoUpdate      # omite el chequeo de actualización
```

## Seguridad

- `-Mode Report` no modifica nada: solo reporta el estado actual de los parámetros que el script puede mover.
- Antes de aplicar, respalda en un `.reg` las claves de registro que toca.
- Cada corrida deja un reporte en `%USERPROFILE%`.
