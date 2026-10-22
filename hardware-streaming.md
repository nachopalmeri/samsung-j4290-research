# Samsung UN32J4290: hardware, reproducción y autonomía

Fecha de revisión: 21/10/2026. Este informe conserva la evidencia reunida durante la auditoría de v2 y la implementación del adaptador de reproducción v3. No hubo conexión con la TV, consultas a su red local, instalación, actualización de firmware ni cambios en Smart Hub. No se hizo una nueva investigación extensa de repositorios.

## Qué se conoce de esta unidad

The investigated television model is **UN32J4290**; the initial case description reported Netflix installed and running slowly. Eso es información del usuario, no una inspección técnica. Siguen pendientes el sufijo regional completo, el firmware instalado y la ejecución física del widget.

**No hay pruebas físicas realizadas en este J4290.** La ejecución en navegador de PC, las pruebas con dobles del SDK y las comprobaciones del paquete no certifican el motor, los códecs, el mando, la persistencia ni el reproductor nativo de la unidad.

| Dato | Evidencia | Límite |
|---|---|---|
| Plataforma | Samsung ubica `UJ4290` en `15TV_ENTRY`: **Samsung Legacy Platform, TV SDK 5.1**. | Es una clasificación oficial de modelos; no una lectura del software instalado. No corresponde aplicar las especificaciones de Tizen por el año o la letra J. |
| Argentina | La ficha de `UN32J4290AGXUG` publica **T-N14MNUABC 1030.0**, 22/06/2019. | Es firmware de catálogo. No se descargó para instalar ni se comprobó que esta unidad tenga ese sufijo o versión. |
| México | La ficha de `UN32J4290AFXZX` publica **T-N14MNAKUC 1020.1**, 07/08/2019. | No se debe intercambiar con la familia AG ni usarla para deducir la variante del usuario. |
| Pantalla | Manual Samsung `BN68-09154A-00_J4290_J5290_ZS_L02_180425.1.pdf`, p. 22 en inglés / p. 44 en español: **1366×768**, diagonal medida 31,5 pulgadas, audio 2×5 W. | Especificación de familia documentada; no medición física. No determina por sí sola la resolución del widget. |
| Conectividad | El mismo manual documenta Ethernet y Wi-Fi IEEE 802.11a/b/g/n, pp. 15–16 en inglés. | No acredita señal, velocidad ni comunicación entre clientes del router del usuario. |
| CPU, RAM y motor web | Los documentos revisados no identifican con precisión CPU, núcleos, MHz, cantidad de RAM ni versión WebKit del modelo/variante. | Permanecen **desconocidos**. No extrapolar de otros J-series, fichas comerciales genéricas o tablas Chromium de Tizen. |

Fuentes originales: [grupos de modelos Samsung](https://developer.samsung.com/smarttv/develop/specifications/tv-model-groups.html), [soporte Argentina](https://www.samsung.com/ar/support/model/UN32J4290AGXUG/), [soporte México](https://www.samsung.com/mx/support/model/UN32J4290AFXZX/), [soporte Perú y descargas del manual AG](https://www.samsung.com/pe/support/model/UN32J4290AGXPE/), [soporte Paraguay AG](https://www.samsung.com/py/support/model/UN32J4290AGCZB/).

Las especificaciones citadas proceden del manual original Samsung enlazado en el informe; las copias locales de trabajo no se publican.

## Cómo obtener los datos pendientes sin modificar el sistema

El eManual Samsung citado en el informe describe **MENU > Support > Contact Samsung** para ver el modelo, la versión de software y la información de Smart Hub.

Esta es una ruta documentada para consultar información, no una ruta de instalación. Su traducción y presencia exactas siguen pendientes en el firmware regional del usuario. No se necesita abrir un menú de servicio para esta comprobación.

## Reproducción: ruta Legacy documentada

La [guía oficial de migración Legacy a Tizen](https://developer.samsung.com/smarttv/develop/migrating-applications/migrating-legacy-platform-to-tizen.html) conserva ejemplos del reproductor anterior. Documenta un objeto `pluginPlayer` con `InitPlayer(url)`, `StartPlayback(seconds)`, `Play(url)`, `Pause()`, `Resume()`, `Stop()`, `JumpForward(seconds)`, `JumpBackward(seconds)` y `GetDuration()`.

También enumera callbacks de tiempo, metadatos, buffering, fin y errores: `OnCurrentPlayTime`, `OnStreamInfoReady`, `OnBufferingStart`, `OnBufferingComplete`, `OnRenderingComplete`, `OnRenderError`, `OnStreamNotFound`, `OnConnectionFailed` y `OnNetworkDisconnected`. El ejemplo usa nombres de funciones globales como cadenas. La duración y el tiempo Legacy se convierten de milisegundos a segundos en el adaptador del proyecto.

La ruta implementada en v3 prepara el reproductor con **InitPlayer → SetDisplayArea → StartPlayback**. No se mezcla esa secuencia con una segunda llamada a `Play`. Su existencia en documentación del SDK justifica una implementación comprobable; no demuestra que todas las fuentes o métodos funcionen en el J4290 real.

El adaptador exige presencia de métodos nativos y captura errores al invocarlos. Una petición enviada no equivale a reproducción exitosa: la pantalla informa estados de carga, reproducción, pausa, fin o error, con espera acotada. La prueba definitiva requiere callbacks y audio/video reales de la unidad.

### Alternativas del SDK

La [guía oficial Legacy a TOAST](https://developer.samsung.com/smarttv/develop/migrating-applications/migrating-legacy-platform-to-toast.html) muestra tres rutas históricas: plugin directo, SEF y `webapis.avplay.getAVPlay`. Para SEF documenta `Open('Player','1.0','Player')` y comandos `Execute` para inicializar y reproducir. Son alternativas de API, no reproductores adicionales certificados para esta unidad.

El proyecto prioriza el plugin directo. SEF solo merece un segundo adaptador si una prueba concreta demuestra que el primero no está disponible y se valida la inicialización de SEF con la referencia completa. Incorporar TOAST, Cordova o una API Tizen moderna no soluciona por sí mismo la compatibilidad Legacy.

Hay una discrepancia entre los ejemplos oficiales sobre las etiquetas `top,left` y `left,top` de `SetDisplayArea`. El rectángulo actual empieza en 0,0; una prueba física con desplazamiento debe comprobar los ejes y el sistema de coordenadas. El tamaño de pantalla física no debe usarse como prueba del sistema de coordenadas del plugin.

Referencia general: [archivo oficial de API Legacy](https://developer.samsung.com/smarttv/legacy/api-references.html). La [descripción de Legacy](https://developer.samsung.com/smarttv/legacy/overview.html) enumera resoluciones de aplicación históricas, sin fijar una versión exacta del motor para J4290.

## Códecs: evidencia y límites

El eManual original asociado por Samsung al modelo AG incluye tablas de formatos en pp. impresas 115–117. La tabla de video admite, con restricciones por archivo/modelo, H.264, MPEG-1/2/4, VC-1, Motion JPEG y VP8. Incluye audio MP3, AAC y otros formatos. Indica **H.264 hasta Level 4.1**, sin FMO/ASO/RS, y advierte que videos de 10 Mbps o más pueden ralentizar los menús.

Estos datos describen capacidades del **reproductor multimedia de la TV**. No certifican automáticamente el elemento HTML `video`, todos los contenedores, un stream HTTP concreto ni cada función expuesta al widget. El eManual incluso limita el audio del navegador integrado a MP3; no debe confundirse navegador con Player nativo.

Consecuencias prácticas:

- La primera validación física debe usar un MP4 progresivo H.264/AAC corto y de bitrate moderado, con fuente conocida y permiso de uso.
- MOV H.264/MP3, MP3 de radio y HLS requieren pruebas separadas. Un contenedor admitido en una tabla no garantiza la combinación exacta de pistas, URL, encabezados o transporte.
- HLS es una ruta experimental del backend nativo; no se anuncia soporte universal ni se agrega MSE/hls.js al motor antiguo.
- El backend HTML5 de PC necesita métodos reales `play`, `pause` y `load`; la mera creación de un elemento `video` en Legacy no prueba soporte. `canPlayType` es una pista, no una garantía.
- Los medios MP4 incluidos en el paquete pueden servir para diagnóstico local. La reproducción nativa de su URL `file://` queda pendiente; se prefiere HTML5 si declara soporte y dispone de métodos reales.
- No se publican en este informe endpoints nuevos ni se afirma que una URL externa siga disponible. La disponibilidad, TLS, redirecciones, geobloqueo y formato se comprueban por fuente y dispositivo.

## Subtítulos y DRM

El proyecto puede leer SRT empaquetados, convertirlos en cues acotados y dibujar texto sincronizado a partir de los callbacks de tiempo. Es una función propia del widget. No supone que el SDK acepte un método de subtítulos adivinado ni que una cuenta externa entregue automáticamente sus pistas.

El manual menciona subtítulos de archivos del reproductor multimedia, pero esa lista no prueba integración de subtítulos con todos los streams del plugin. El adaptador usa texto plano y no ejecuta HTML de los subtítulos.

Legacy tuvo APIs históricas de DRM. Su existencia no concede credenciales, licencias, certificados ni compatibilidad con Netflix, Disney+, Max, MUBI, Flow o Stremio. Una URL pública MP4/HLS sin DRM y una sesión de un servicio comercial son problemas distintos. No se evaden DRM ni se reutilizan sesiones de apps instaladas. La comparación de servicios documentada en `compatibility-v2.md` sigue siendo investigación de disponibilidad, no una lista de integraciones activas.

## Autonomía real y dependencia de servidores

Una vez instalado el widget y sus recursos, funciones escritas en HTML/JS con datos locales —reloj, calendario, temporizador, cronómetro, contador, fondos y subtítulos empaquetados— pueden funcionar sin una PC encendida. Esa autonomía describe la arquitectura; su instalación y ejecución en la unidad todavía deben verificarse.

Un reproductor con una URL HTTP pública puede ejecutarse directamente en la TV sin servidor personal intermediario, si el transporte, el formato y el firmware son compatibles. Sigue dependiendo de Internet y del servidor de esa fuente. Una radio online no es una función offline. Un servidor usado para transferir inicialmente el ZIP no tiene por qué ser una dependencia permanente de la interfaz instalada.

No se verificó enumeración arbitraria de archivos USB, biblioteca DLNA ni catálogo del usuario desde este widget. No se promete un explorador local a partir de que la TV tenga USB o un menú multimedia propio.

### Qué aportan los repositorios Orsay revisados anteriormente

La evidencia anterior identifica [Samsung/cordova-sectv-orsay](https://github.com/Samsung/cordova-sectv-orsay) como referencia oficial histórica de estructura y empaquetado; no fue instalado ni convierte una app moderna en un cliente multimedia certificado.

[Jellyfin Orsay Installer](https://github.com/Apps2Samsung/Jellyfin-Orsay-Installer) aporta evidencia comunitaria de transferencia HTTP/widgetlist para Orsay. Es un instalador archivado: **no es una certificación J4290 ni un reproductor offline sin servidor**. El papel de un instalador debe separarse del cliente que instala y del servidor multimedia del que ese cliente depende.

El [caso comunitario sobre J4290 en Emby](https://emby.media/community/topic/111104-samsung-smart-tv-j4290/) es coherente con la clasificación Orsay. No sustituye documentación de Samsung ni una prueba en esta unidad. No se auditó aquí un cliente Emby completo ni se demostró que opere sin servidor Emby.

[Yandex RTV](https://github.com/yandex/rtv) sirve como referencia histórica de herramientas Orsay. No se verificó en este trabajo un reproductor OTT/IPTV específico, una lista de canales ni un endpoint activo a partir de ese repositorio.

Por tanto, estos antecedentes ayudan a razonar sobre empaquetado e instalación, pero **no justifican afirmar compatibilidad de un reproductor OTT/IPTV/Emby ni autonomía sin servidor en J4290**. La ruta más pequeña comprobable sigue siendo un widget local con utilidades reales y un adaptador Player para medios de prueba explícitos. No se amplió esta fase a una búsqueda indefinida de clientes abandonados.

## Instalación futura: hipótesis operativas, no pasos ya validados

El flujo comunitario anterior propone una PC que sirva por HTTP un catálogo `widgetlist` y un ZIP, y una sincronización iniciada desde la TV mediante cuenta `develop`. La PC puede usar Ethernet y la TV Wi-Fi si el router permite conectividad IP entre ambos clientes. No necesita publicar el servidor en Internet.

**No se estableció el menú exacto de instalación para el sufijo y firmware regional del usuario.** Cuenta, aceptación, puertos, formato del catálogo y opciones de sincronización deben validarse antes de ofrecer instrucciones como reproducibles. No se ejecutó ese procedimiento.

La vía USB oficial antigua citada en la investigación está limitada a determinadas generaciones; la receta comunitaria `userwidget` no está certificada aquí para J4290. Tampoco se trasladan códigos de developer mode, comandos Tizen, root, remapeo Home, autoinicio o sustitución de Smart Hub desde otros modelos.

Un fallo de acceso no identifica por sí solo al firewall: también pueden intervenir el servidor detenido, dirección incorrecta o aislamiento de clientes. No se cambiaron firewall, router ni permisos para investigar esas posibilidades.

## Verificación que falta en la TV

1. Leer modelo completo y software desde información de soporte; registrar solo datos necesarios, sin números de serie ni identificadores personales.
2. Confirmar una vía de instalación regional reproducible y autorizada.
3. Ejecutar interfaz, mando y persistencia; cerrar y volver a abrir, y comprobar tras un ciclo de energía.
4. Probar MP4 progresivo y callbacks del Player; comprobar coordenadas, tiempo, duración, pausa, búsqueda y fin.
5. Probar por separado MOV, radio MP3 y HLS, incluyendo fallos de red y espera acotada.
6. Confirmar que Return, Exit y salida de pantalla detienen el medio y que callbacks tardíos no reactivan una reproducción anterior.

Hasta completar esas comprobaciones, la descripción correcta es **implementación con pruebas de escritorio y dobles de API; compatibilidad física pendiente**.


