# Samsung UN32J4290 — matriz de compatibilidad v2

Investigación: 21/10/2026. Esta es la referencia actual para el producto v2; los informes anteriores se conservan como historial. No se contactó ni modificó la TV en esta fase.

**Etiquetas:** confirmado = documentación del fabricante o proveedor; comunitario = experiencia o herramienta independiente; decisión = elección de ingeniería de este proyecto; pendiente = no comprobado en la unidad real. Una documentación de API demuestra su existencia, no que este firmware la ejecute correctamente.

## Modelo, variantes y plataforma

| Punto | Evidencia y límite | Consecuencia |
|---|---|---|
| Plataforma | **Confirmado:** Samsung lista `UJ4290` en `15TV_ENTRY`, Samsung Legacy Platform, TV SDK 5.1. [Samsung Model Groups](https://developer.samsung.com/smarttv/develop/specifications/tv-model-groups.html). | Widget HTML/CSS/JS Legacy, conocido comunitariamente como Orsay. No APK/ADB ni widget Tizen moderno. |
| Año | La tabla técnica agrupa la plataforma como 2015; la ficha peruana comercializa el J4290 AG como Smart TV 2018. [Samsung Perú](https://www.samsung.com/pe/support/model/UN32J4290AGXPE/). | No deducir plataforma a partir del año de venta ni la letra J. |
| Argentina | **Confirmado:** `UN32J4290AGXUG`, firmware publicado T-N14MNUABC 1030.0, 22/06/2019. [Soporte AR](https://www.samsung.com/ar/support/model/UN32J4290AGXUG/). | Dato de catálogo, no lectura del firmware instalado. |
| México | **Confirmado:** `UN32J4290AFXZX`, familia T-N14MNAKUC, versión publicada 1020.1. [Soporte MX](https://www.samsung.com/mx/support/model/UN32J4290AFXZX/). | No intercambiar firmware AF/AG ni inferir sufijo del usuario. |
| Perú / Paraguay | Existen fichas `UN32J4290AGXPE` y `UN32J4290AGCZB`. [PE](https://www.samsung.com/pe/support/model/UN32J4290AGXPE/), [PY](https://www.samsung.com/py/support/model/UN32J4290AGCZB/). | La tienda regional, los menús y las apps pueden variar. |
| Unidad del usuario | Modelo parcial UN32J4290 aportado. Sufijo y versión de software **pendientes**. | Requisito para preparar una futura prueba física, no impedimento para revisar el diseño. |
| Pantalla física | **Confirmado en el manual original Samsung**, BN68-09154A-00_J4290_J5290_ZS_L02_180425.1.pdf, p. 22 (44 en español): **1366×768**, diagonal medida 31,5", audio 2×5 W. Recuperado de la descarga enlazada en [Samsung Perú](https://www.samsung.com/pe/support/model/UN32J4290AGXPE/); tabla renderizada e inspeccionada. | Especificación de familia UN32J4290; no medición de esta unidad. No equivale al tamaño del widget. |
| CPU / RAM | **No confirmados** con fuente exacta de fabricante para este modelo/variante. | No inventar núcleos, MHz ni MB. Medir consumo y respuesta más adelante. |
| Smart Hub | Legacy / Application Manager; generación exacta y opciones del firmware regional **pendientes**. | Aplicación independiente dentro de Smart Hub, sin sustitución del sistema. |

## SDK, motor y decisiones de implementación

La [descripción oficial de Legacy](https://developer.samsung.com/smarttv/legacy/overview.html) enumera tamaños de aplicación 960×540, 1280×720 y 1920×1080. También describe WebKit para generaciones históricas, pero no fija una versión exacta para el J4290. Las tablas modernas de Chromium/Tizen no se aplican a esta TV.

| Capacidad | Estado | Producto v2 |
|---|---|---|
| HTML/CSS/JS | Base documentada de widgets Legacy; versión exacta del motor desconocida. | JS escrito con sintaxis conservadora ES5/ES3; sin módulos, fetch, Promises ni frameworks en ejecución. Node/Chrome lo ejecutan; no hay certificación del motor real. |
| Lienzo | **Decisión:** 1280×720, uno de los tamaños documentados. | `config.xml` y geometría coinciden. Escalado único al viewport con prefijo WebKit; fallback zoom si existe. Tamaño real del runtime pendiente. |
| CSS | No se encontró una matriz completa por firmware J4290. | Posiciones y colores sólidos; sin flex/grid/calc/variables/blur. Borde de foco sin transformaciones. `rgba`, opacidad, radio y escalado requieren prueba física; la geometría nativa no depende de una animación. |
| Recursos | JPEG y PNG de base; no reproductor multimedia. | JPEG baseline 1280×720 y PNG locales. Sin WebP, vídeo, fuentes remotas ni TLS para cargar la interfaz. No se afirma soporte de todos los códecs de la TV. |
| Mando | TVKeyValue, eventos de teclado y ciclo Widget documentados. | Se comparan `KEY_UP/DOWN/LEFT/RIGHT/ENTER/RETURN/EXIT` del SDK. No se usan números de Return/Exit de Tizen. En PC: flechas, Enter, Escape/Backspace y X para Exit. |
| Return / Exit | `blockNavigation(event)`, `sendReturnEvent()` y `sendExitEvent()` documentados. | Back cierra primero la capa actual. En inicio pregunta antes de volver a Smart Hub; Exit pregunta antes de salir a TV. Pruebas con dobles de SDK, no mando físico. |
| Persistencia | **Confirmado:** API Legacy FileSystem común. El ejemplo de `localStorage` en la guía de migración corresponde al destino Tizen. | FileSystem por `curWidget.id`, lectura/escritura/cierre y comprobación posterior; localStorage opcional con try/catch. Si falla, la sesión sigue usable y avisa. Persistencia física tras apagado pendiente. |
| Abrir apps | `runSearchWidget(appID, extraInfo)` existe; exige ID conocido, sin confirmación de apertura exitosa. | Todos los IDs vacíos y `verified:false`. Solo una verificación futura por unidad puede habilitar el adaptador. Ninguna ficha actual promete reproducción. |
| Descubrir apps | No se verificó una API pública para enumerar arbitrariamente apps instaladas en esta unidad. | Sin detección automática ni falsos estados de instalación. |
| Home / autoinicio | Sin procedimiento reproducible encontrado para J4290; las pruebas de otros Tizen no son transferibles. | Descartados del alcance actual: remap Home, sustituir Smart Hub, root, eliminar paquetes, autoinicio. Ausencia de evidencia no prueba imposibilidad universal. |
| Red / sistema | No se han habilitado plugins nativos ni consultas al dispositivo. | Hora local del entorno, viewport y detección de API reales. Internet, CPU y RAM figuran sin medición. |

Referencias de SDK: [índice oficial de API Legacy](https://developer.samsung.com/smarttv/legacy/api-references.html), [migración oficial y ejemplo FileSystem anterior](https://developer.samsung.com/smarttv/develop/migrating-applications/migrating-legacy-platform-to-tizen.html), [guía Samsung v1.20 en espejo, §§5.4 y 8.2](https://freethetv2011.s3.amazonaws.com/App_Development_Guide_for_Samsung_Smart_TV%5BV1.20%5D%5B1%5D.pdf). En FileSystem, `isValidCommonPath` devuelve 1 cuando el directorio es válido; no se interpreta 2 como verdadero válido.

### Formatos y navegador: comprobación adicional del eManual original

El eManual `ENG_NMISDBN-1.133_180502.1.pdf` asociado por [Samsung al modelo AG](https://www.samsung.com/pe/support/model/UN32J4290AGXPE/) documenta, p. impresa 115 (PDF 120), JPEG hasta 15360×8640 y PNG/BMP hasta 4096×4096, además de MPO. La tabla fue renderizada e inspeccionada. **Son capacidades del reproductor multimedia; no certifican el decodificador de imágenes HTML del widget.** Nuestros recursos 1280×720 quedan deliberadamente por debajo.

En p. 116 enumera H.264, MPEG-1/2/4, VC-1, Motion JPEG y VP8, condicionados al contenedor y modelo. Incluye audio MP3/AAC/FLAC/Vorbis/WMA/WAV y subtítulos de archivo como SRT/SSA/ASS. Esto no habilita DRM, streaming moderno ni sincronización Stremio. Las pp. 111–112 advierten limitaciones de fuentes, latencia de mando durante cargas y vídeos web; el navegador solo declara MP3 como audio. No se conoce su versión WebKit exacta.

El manual de usuario, pp. 15–16, documenta Wi-Fi 802.11a/b/g/n y conexión Ethernet. Esta capacidad confirma que mezclar PC cableada y TV inalámbrica no requiere otra notebook; la conectividad entre clientes depende del router.

## Servicios solicitados

Las cuentas, suscripciones, historial, DRM y subtítulos pertenecen a cada cliente de servicio. Una portada no los sincroniza. Las fichas del producto explican esto y permiten favoritos; no se disfrazan de botones de reproducción.

| Servicio | Evidencia oficial | Estado honesto para J4290 |
|---|---|---|
| Netflix | [Dispositivos compatibles](https://help.netflix.com/en/node/14361); disponibilidad variable según aparato. The initially reported unit had Netflix installed and running slowly. | Presente según usuario, sesión y lanzamiento desde widget sin comprobar. No se reutiliza el antiguo ID comunitario. |
| YouTube | [Dispositivos admitidos](https://support.google.com/youtube/answer/7582560?hl=en-IN) enumera Samsung, sin certificar J4290. | Pendiente revisar la tienda y app real de esta unidad. |
| Stremio | [Anuncio oficial Samsung](https://blog.stremio.com/stremio-is-now-available-in-the-samsung-tv-app-store/): modelos desde 2019. | Sin cliente Orsay verificado. Cliente compatible externo para cuenta/subtítulos. |
| MUBI | [Dispositivos MUBI](https://mubi.com/en/devices): Samsung desde 2017. | Fuera de Legacy documentado; equipo compatible externo. |
| Max / HBO Max | [Ayuda Max](https://help.max.com/sg-en/Answer/Detail/000002506): Samsung desde 2016 en esa región. | No demuestra cliente Orsay. Comprobar disponibilidad regional en el dispositivo externo. |
| Disney+ | [Ayuda Disney+](https://help.disneyplus.com/en-GB/article/disneyplus-en-ba-samsung-tv): excluye Orsay y navegador integrado. | Incompatible con esta plataforma según proveedor. |
| Flow | [Personal Flow](https://www.personal.com.ar/flow), sección dispositivos: Samsung Tizen 5.0+. | No corresponde a Legacy; cliente externo compatible. |
| Brave | [Requisitos de Brave](https://support.brave.com/hc/en-us/articles/360021357112-What-are-the-system-requirements-to-install-Brave): Windows/macOS/Linux/Android/iOS, sin cliente Samsung Legacy. | No hay integración ni Sync verificable en esta TV. Computadora compatible por HDMI como alternativa para perfil completo. |

## Repositorios y transferencia futura

- [Samsung/cordova-sectv-orsay](https://github.com/Samsung/cordova-sectv-orsay): referencia oficial histórica de estructura y paquete. Se conserva el formato widget ZIP con `config.xml` en raíz; no se instala Cordova.
- [CleanYourTV](https://github.com/Gathin23/cleanyourtv), [tv-makeover](https://github.com/paisanos-io/tv-makeover): Android/Google TV; aportan ideas de foco y simplicidad, no APIs para J4290.
- CleanFrame, arn0-tv y AeroTizen, analizados en `github-j4290.md`: otros modelos Tizen. No se trasladan sus comandos Home ni root.
- [Yandex RTV](https://github.com/yandex/rtv): herramientas históricas Orsay, evidencia del flujo diferente de widgets, no certificación de este hardware.
- [Jellyfin Orsay Installer](https://github.com/Apps2Samsung/Jellyfin-Orsay-Installer): flujo comunitario HTTP/widgetlist, archivado. Apps2Samsung moderno excluye Orsay. [Caso comunitario específico J4290](https://emby.media/community/topic/111104-samsung-smart-tv-j4290/) concuerda con plataforma Orsay.

Futura instalación: PC y TV necesitan conectividad IP local; Ethernet en la PC y Wi-Fi en la TV son compatibles si el router no aísla los clientes. El flujo comunitario usa catálogo HTTP en puerto 80, cuenta `develop` y sincronización iniciada desde la TV. Menús/aceptación regional todavía no probados. No necesita abrir puertos de Internet. Un fallo al abrir una URL **no prueba por sí solo** que sea el firewall: también pueden intervenir servidor detenido, IP distinta o aislamiento de red.

La documentación oficial Legacy limita su antigua vía USB de desarrollo a plataforma 2012; la receta comunitaria `userwidget` no está certificada para J4290. No se recomienda instalar el ZIP USB histórico como si estuviera validado. No se ejecutó transferencia, reinicio, reset, firewall ni configuración de router en esta fase.


