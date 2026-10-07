# Gravity Clips

Clips para Instagram a partir de podcasts y videos largos, en tu propia PC.
Transcribe, elige los mejores momentos con IA, corta, reencuadra a 9:16, pone
subtítulos y música. También edita videos (saca tomas falladas y silencios),
multiplica un clip en variantes con títulos distintos y arma el multicam de un
podcast.

## Requisitos

- Windows 10 u 11.
- Placa de video **NVIDIA** con el driver actualizado ([nvidia.com/drivers](https://www.nvidia.com/drivers)).
- **20 GB libres** para instalar, y más espacio para tus videos (30 GB recomendados).
- Para la IA, una de estas dos:
  - **Recomendado:** un plan pago de Claude (Pro o Max). Usa tu suscripción, sin
    pagar API ni pegar claves.
  - Una clave de la API de Anthropic con saldo ([console.anthropic.com](https://console.anthropic.com)).

## Instalar con tu Claude (lo más fácil)

Si usás Claude Code (en la terminal o en la app de escritorio de Claude), pedile:

> Instalá Gravity Clips en esta PC siguiendo las instrucciones de
> https://github.com/gravitystudios00/gravity-clips-lauti/blob/master/INSTALAR_CON_CLAUDE.md

Tu Claude revisa la PC, baja la última versión, la verifica, la instala, conecta
la IA y te guía en los pasos que tenés que hacer vos (iniciar sesión con tu
cuenta de Claude).

## Instalar a mano

1. Entrá a **[Releases](https://github.com/gravitystudios00/gravity-clips-lauti/releases/latest)**
   y abrí la última versión.
2. Descargá **todos** los archivos `GravityClips.zip.001`, `.002`, … y también
   `INSTALAR.bat`. Guardalos todos en la **misma carpeta**.
3. Doble clic en `INSTALAR.bat`. Une los pedazos y descomprime en
   `C:\GravityClips`. Tarda varios minutos.
4. Abrí `C:\GravityClips\LEEME.txt` y después **`Gravity Clips.bat`**
   (`Crear acceso directo.bat` te deja un ícono en el escritorio).
5. La primera vez se abre en **Ajustes → Conexión con la IA**. Con
   **"Mi suscripción de Claude"**:
   1. **Instalar**: se abre una ventana que instala Claude Code (el programa
      oficial de Anthropic). Esperá a que diga "Listo" y cerrala.
   2. **Iniciar sesión**: se abre el navegador; entrá con tu cuenta de Claude y
      aceptá. Se hace una sola vez.
   3. **Probar y usar**.

   En la misma pantalla, el diagnóstico tiene que dar todo OK.

Windows puede avisar "editor desconocido" al abrir los `.bat`: tocá
**Más información → Ejecutar de todas formas**.

Si algún pedazo se bajó mal, `SHA256.txt` tiene la huella de cada uno. En
PowerShell: `Get-FileHash GravityClips.zip.001` y compará.

## Actualizar

Si ya lo tenés instalado, no hace falta bajar todo de nuevo: de la última
versión en Releases bajá solo `GravityClips-app-X.Y.Z.zip` y `ACTUALIZAR.bat`,
ponelos en la misma carpeta y hacé doble clic en `ACTUALIZAR.bat` (con Gravity
Clips cerrado). Cambia solo el programa y conserva tu conexión con la IA.

## Uso de la IA

Con la suscripción de Claude, cada video procesado cuenta contra los límites de
tu plan; un podcast largo consume bastante. Si llegás al límite, el trabajo se
frena con un aviso en la Cola hasta que se libere.

## Si algo falla

En **Ajustes → Descargar para soporte** se arma un archivo con el diagnóstico y
el registro del programa. Mandáselo a Gravity. No incluye claves ni videos.
