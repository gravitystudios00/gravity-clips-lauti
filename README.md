# Gravity Clips

Clips para Instagram a partir de podcasts y videos largos, en tu propia PC.
Transcribe, elige los mejores momentos con IA, corta, reencuadra a 9:16, pone
subtítulos y música.

## Requisitos

- Windows 10 u 11.
- Placa de video **NVIDIA** con el driver actualizado ([nvidia.com/drivers](https://www.nvidia.com/drivers)).
- **20 GB libres** para instalar, y más espacio para tus videos (30 GB recomendados).
- Una clave de la API de Anthropic (Claude) con saldo: [console.anthropic.com](https://console.anthropic.com).

## Instalar

1. Entrá a **Releases** (a la derecha) y abrí la última versión.
2. Descargá **todos** los archivos `GravityClips.zip.001`, `.002`, … y también
   `INSTALAR.bat`. Guardalos todos en la **misma carpeta**.
3. Doble clic en `INSTALAR.bat`. Une los pedazos y descomprime en
   `C:\GravityClips`. Tarda varios minutos.
4. Abrí `C:\GravityClips\LEEME.txt` y después **`Gravity Clips.bat`**.
5. La primera vez se abre en **Ajustes**: pegá tu clave de Anthropic y tocá
   "Probar y guardar". En la misma pantalla, el diagnóstico tiene que dar todo OK.

Windows puede avisar "editor desconocido" al abrir los `.bat`: tocá
**Más información → Ejecutar de todas formas**.

Si algún pedazo se bajó mal, `SHA256.txt` tiene la huella de cada uno. En
PowerShell: `Get-FileHash GravityClips.zip.001` y compará.

## Actualizar

Cuando haya una versión nueva vas a recibir un `GravityClips-app-X.Y.Z.zip`
chico. Cerrá Gravity Clips, copiá `C:\GravityClips\app\config.local.json` (tu
clave) a otro lado, borrá la carpeta `app`, descomprimí el zip en su lugar y
volvé a poner el `config.local.json`. `motor` y `datos` no se tocan.

## Si algo falla

En **Ajustes → Descargar para soporte** se arma un archivo con el diagnóstico y
el registro del programa. Mandáselo a Gravity. No incluye tu clave ni tus videos.
