# Instrucciones para Claude: instalar Gravity Clips

Estas instrucciones son para **Claude Code** corriendo en la PC de la persona que
va a usar Gravity Clips. Seguilas en orden y contale a la persona qué estás
haciendo en cada paso, en español y sin tecnicismos.

Gravity Clips es un programa local para Windows: transcribe videos con la placa
NVIDIA y usa Claude para elegir clips, decidir recortes y escribir títulos. Se
instala en `C:\GravityClips` y se usa desde el navegador.

## Reglas

- **Nunca pidas ni escribas contraseñas, claves de API ni tokens.** El inicio de
  sesión en Claude lo hace la persona en su navegador.
- **No modifiques archivos dentro de `C:\GravityClips\app` ni `motor`.** Si algo
  falla, diagnosticá y explicá; no parchees el programa.
- **Antes de descargar, avisá el tamaño** (unos 6 GB) y esperá el OK.
- No borres nada de `C:\GravityClips\datos`: son los videos y clips de la persona.
- Los comandos de abajo son para PowerShell. Si tu terminal es bash, adaptalos.

## Paso 1: revisar la PC

1. Windows 10 u 11.
2. Placa NVIDIA y driver: `nvidia-smi`. Si no responde o no hay placa NVIDIA,
   frená: el programa necesita una. Si el driver es viejo, indicá
   https://www.nvidia.com/drivers.
3. Espacio en C: `[math]::Round((Get-PSDrive C).Free/1GB)`. Hacen falta **30 GB
   libres** como mínimo (6 GB de descarga + 9 GB instalado + lugar para videos).
4. ¿Ya está instalado? `Test-Path C:\GravityClips\app`.
   - Si **no**: seguí con el Paso 2 (instalación nueva).
   - Si **sí**: andá al Paso 2b (actualizar). No reinstales de cero.

## Paso 2: instalación nueva

```powershell
$d = "$env:USERPROFILE\Downloads\GravityClips-instalador"
New-Item -ItemType Directory -Force $d | Out-Null
$base = "https://github.com/gravitystudios00/gravity-clips-lauti/releases/latest/download"
curl.exe -L --fail -o "$d\SHA256.txt" "$base/SHA256.txt"
curl.exe -L --fail -o "$d\INSTALAR.bat" "$base/INSTALAR.bat"
# SHA256.txt lista todos los archivos de la version: se bajan los pedazos del zip
Get-Content "$d\SHA256.txt" | ForEach-Object { ($_ -split '\s+')[1] } |
  Where-Object { $_ -like "GravityClips.zip.*" } |
  ForEach-Object { curl.exe -L --fail -o "$d\$_" "$base/$_" }
```

**Verificar** que cada pedazo coincida con `SHA256.txt`:

```powershell
Get-Content "$d\SHA256.txt" | ForEach-Object {
  $h, $n = $_ -split '\s+'
  if (Test-Path "$d\$n") { "{0}  {1}" -f ((Get-FileHash "$d\$n").Hash.ToLower() -eq $h), $n }
}
```

Todos tienen que dar `True`. Si alguno da `False`, volvé a bajar ese pedazo.

**Instalar** (une los pedazos y descomprime en `C:\GravityClips`, tarda 5 a 10
minutos; corrélo en segundo plano y esperá):

```powershell
Start-Process cmd.exe -ArgumentList "/c `"$d\INSTALAR.bat`" < nul" -WorkingDirectory $d -Wait -WindowStyle Hidden
Test-Path C:\GravityClips\motor\python\python.exe
```

Tiene que dar `True`. Después preguntale a la persona si querés borrar la carpeta
de descarga (6 GB que ya no hacen falta).

Seguí con el Paso 3.

## Paso 2b: actualizar una instalación existente

```powershell
$d = "$env:USERPROFILE\Downloads\GravityClips-actualizacion"
New-Item -ItemType Directory -Force $d | Out-Null
$base = "https://github.com/gravitystudios00/gravity-clips-lauti/releases/latest/download"
curl.exe -L --fail -o "$d\SHA256.txt" "$base/SHA256.txt"
curl.exe -L --fail -o "$d\ACTUALIZAR.bat" "$base/ACTUALIZAR.bat"
$zip = Get-Content "$d\SHA256.txt" | ForEach-Object { ($_ -split '\s+')[1] } |
  Where-Object { $_ -like "GravityClips-app-*.zip" }
curl.exe -L --fail -o "$d\$zip" "$base/$zip"
```

Verificá la huella del zip igual que en el Paso 2. Pedile a la persona que
**cierre Gravity Clips** (la ventana negra) y después:

```powershell
Start-Process cmd.exe -ArgumentList "/c `"$d\ACTUALIZAR.bat`" < nul" -WorkingDirectory $d -Wait -WindowStyle Hidden
Get-Content C:\GravityClips\app\CHANGELOG.md -TotalCount 25
```

La actualización conserva la conexión con la IA. Contale a la persona qué trae
la versión nueva (el CHANGELOG) y seguí con el Paso 4.

## Paso 3: acceso directo

```powershell
Start-Process cmd.exe -ArgumentList "/c `"C:\GravityClips\Crear acceso directo.bat`" < nul" -Wait -WindowStyle Hidden
```

Queda un ícono "Gravity Clips" en el escritorio.

## Paso 4: Claude Code instalado y con sesión

Gravity Clips usa la **suscripción de Claude** de la persona a través del
comando `claude`. Necesita un plan pago (Pro o Max).

1. `claude --version`. Si no existe el comando (por ejemplo, si estás corriendo
   dentro de la app de escritorio y no hay CLI instalada), instalala con el
   instalador oficial:

   ```powershell
   irm https://claude.ai/install.ps1 | iex
   ```

   Queda en `%USERPROFILE%\.local\bin\claude.exe`.
2. `claude auth status --json`. Tiene que decir `"loggedIn": true` y
   `"authMethod": "claude.ai"`.
   - Si `loggedIn` es `false`: la persona tiene que iniciar sesión. Lo más simple
     es desde el panel (Paso 5, botón **Iniciar sesión**), o que corra
     `claude auth login` en una terminal: se abre el navegador, entra con su
     cuenta de Claude y acepta.
   - Si `authMethod` es `console`, esa sesión cobra como API: pedile que inicie
     sesión de nuevo con su cuenta de Claude (`claude auth login --claudeai`).

## Paso 5: abrir el programa y conectar la IA

```powershell
Start-Process "C:\GravityClips\Gravity Clips.bat"
```

Se abre una ventana negra (tiene que quedar abierta mientras se usa) y el panel
en el navegador, normalmente en http://127.0.0.1:8766 (si ese puerto está
ocupado, la ventana negra dice cuál usó). Esperá a que responda:

```powershell
Invoke-RestMethod http://127.0.0.1:8766/api/edicion
```

**Conectar la IA.** En el panel: **Ajustes → Conexión con la IA →
"Mi suscripción de Claude"**. Los pasos "Instalar Claude Code" e "Iniciar sesión"
tienen que estar en verde; después **Probar y usar**. Si preferís hacerlo vos
por la API del panel (hace la misma consulta mínima y guarda):

```powershell
Invoke-RestMethod -Method Post http://127.0.0.1:8766/api/ajustes/clave `
  -ContentType "application/json" -Body '{"proveedor":"claude_code","api_key":""}'
```

Tiene que responder `ok: True`.

## Paso 6: diagnóstico

```powershell
(Invoke-RestMethod http://127.0.0.1:8766/api/diagnostico).chequeos |
  Select-Object estado, nombre, detalle, ayuda | Format-Table -Wrap
```

Todo tiene que dar `ok`. "Música de fondo" puede dar `aviso` si no hay pistas.
Si algo da `falla`, la columna `ayuda` dice qué hacer (ver también la tabla de
abajo).

## Paso 7: contarle a la persona cómo seguir

- Los videos largos van en `C:\GravityClips\datos\_videos` (o se suben desde
  "Nuevo corte").
- Primero tiene que completar un **contexto** en "Contextos": quién habla, a quién
  le habla y qué tipo de clips busca. El de ejemplo no se puede usar hasta
  completarlo (y borrar la línea "SIN CONFIGURAR").
- "Nuevo corte" saca clips de un podcast; "Edición normal" limpia un video;
  "Multiplicar" hace variantes de un clip; "Multicam" arma un episodio con varias
  cámaras.
- Cada video procesado cuenta contra los límites de uso de su plan de Claude.

## Si algo falla

| Síntoma | Qué hacer |
|---|---|
| `nvidia-smi` no responde | Instalar o actualizar el driver de NVIDIA y reiniciar. |
| Windows avisa "editor desconocido" al abrir un `.bat` | **Más información → Ejecutar de todas formas.** |
| `INSTALAR.bat` falla | Revisar que estén todos los pedazos y 20 GB libres en C:. |
| "Ya hay una instalación en C:\GravityClips" | Es una actualización: usar el Paso 2b. |
| Diagnóstico: "Placa de video" falla | Driver de NVIDIA; reiniciar la PC. |
| Diagnóstico: "ffmpeg" falla | El antivirus puede estar bloqueándolo: agregar `C:\GravityClips` a las exclusiones. |
| "Claude Code: Not logged in" o "La sesión de Claude está cerrada" | Iniciar sesión (Paso 4). |
| La Cola dice que se llegó al límite de uso | Es el límite del plan de Claude: esperar a que se libere. |
| El panel no abre | Mirar el error en la ventana negra; volver a abrir `Gravity Clips.bat`. |

Si no se resuelve: en el panel, **Ajustes → Descargar para soporte** arma un zip
con el diagnóstico y el registro (sin claves ni videos). La persona se lo manda
a Gravity.
