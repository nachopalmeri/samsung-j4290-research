# Samsung UN32J4290: investigación de home, apps y rendimiento

Fecha de consulta: 21 de octubre de 2026.

## Resultado

No encontré un procedimiento reproducible y validado para reemplazar permanentemente Home en una UN32J4290, ni un perfil de debloat para su firmware. Sí hay una vía documentada para instalar aplicaciones comunitarias en Samsung Orsay. Instalar un widget, reemplazar Home, conseguir root y acelerar Netflix son cuatro objetivos distintos: la evidencia de uno no demuestra los otros.

La evidencia disponible apunta a Orsay para esta familia, pero falta el sufijo completo del modelo y la cadena de software del equipo del usuario para cerrar la identificación. No deducir el sistema solo de la letra J, el año de compra o una ficha de MercadoLibre.

## 1. Qué sabemos del modelo

- Samsung Argentina tiene soporte para UN32J4290AGXUG. La búsqueda de su ficha devuelve el paquete T-N14MNUABC, versión 1030.0, fechado 22/06/2019. Es una descarga publicada para esa variante, no una comprobación del firmware instalado ni de todas las variantes regionales. [Samsung AR](https://www.samsung.com/ar/support/model/UN32J4290AGXUG/).
- En un hilo específico J4290, un colaborador del foro oficial de Emby explica que J4200 usa Orsay y que por eso no instala la app Tizen. Es evidencia técnica de soporte comunitario para esta familia; no es una certificación Samsung del ejemplar del usuario. [Emby, agosto 2022](https://emby.media/community/topic/111104-samsung-smart-tv-j4290/).
- Un propietario de UN32J4290 describe demoras en Smart Hub, apertura de apps y respuesta al control, incluso después de reinicios y asistencia remota. Es un reporte individual de octubre 2021; no diagnostica la causa de nuestra TV. [Samsung Community](https://eu.community.samsung.com/t5/tvs/incidencias-con-el-sistema/m-p/4189877/highlight/true).

## 2. Lo de los posts y los repositorios

| Proyecto o caso | Evidencia y límites | Resultado para esta TV |
|---|---|---|
| [CleanYourTV upstream](https://github.com/Gathin23/cleanyourtv) | Android TV/Google TV, ADB y launcher Android. El enlace Pendia citado anteriormente es un fork. | No hay ruta para instalar ese launcher Android en Orsay. |
| [paisanos-io/tv-makeover](https://github.com/paisanos-io/tv-makeover) | Se basa en CleanYourTV y requiere Android TV/Google TV; excluye Samsung Tizen. Sus plantillas y ajustes extremos no cambian la plataforma requerida. | No compatible como solución directa para J4290. |
| [CleanFrame](https://github.com/nthnclrk/CleanFrame) | Referencia The Frame LS03D, Tizen 9, firmware 2130.0. Home y arranque emplean helpers no documentados; no promete compatibilidad general. | No hay validación J4290/Orsay. |
| [Arn0](https://github.com/NelsonEU/arn0-tv) | Referencia Q70B 2022, Tizen 6.5, firmware 1720.7. Home usa preferencias de plataforma sin root; arranque en frío no totalmente validado. | No copiar sus cambios nativos a esta TV. |
| [AeroTizen](https://github.com/helalaou/AeroTizen) | Referencia DU7200 2024 con Tizen 9; Home tiene retraso y limitaciones según app. | Tampoco prueba soporte para J4290. |
| [Apps2Samsung actual](https://github.com/Apps2Samsung/Apps2Samsung/blob/beta/docs/index.html) | Herramienta para Tizen; excluye Orsay explícitamente y remite al instalador legado sin mantenimiento. | No usar su flujo Tizen suponiendo que toda Samsung es compatible. |

El README de tv-makeover aporta un límite relevante: en su Hitachi Android 11, quitar bloat por sí solo no aceleró las apps; atribuye mejoras a los cambios de launcher, asistente y animaciones. Es una medición del autor en ese equipo, no un benchmark de J4290 ni de Netflix.

El modo desarrollador no implica root. Samsung dice que SDB no concede root en su flujo de desarrollo. Eso no prueba que nunca haya exploits, pero invalida presentar modo desarrollador = acceso root como instrucción universal. [Samsung Developer](https://developer.samsung.com/smarttv/develop/faq/application-testing.html).

## 3. La vía Orsay que sí existe

[Jellyfin-Orsay-Installer](https://github.com/Apps2Samsung/Jellyfin-Orsay-Installer) documenta el siguiente mecanismo general:

1. Ejecutar en la PC un servidor local que publique el widget y widgetlist.xml.
2. Entrar a Smart Hub con la cuenta de desarrollo develop.
3. Abrir el menú de desarrollo y configurar la IP de la PC.
4. Sincronizar aplicaciones de usuario para descargar el widget.

El README muestra variantes de teclas y cuenta, y reconoce que los menús varían con el firmware. Este flujo no equivale al 12345/SDB de los modelos Tizen.

El instalador y [la app Orsay](https://github.com/Apps2Samsung/Jellyfin-Orsay-App) están archivados desde el 6 de junio de 2026. La app señala que el sideload puede seguir siendo posible, pero carece de soporte. No hay prueba específica de J4290 en lo revisado.

Esto permitiría investigar una aplicación sencilla para Orsay si el equipo conserva ese menú y acepta widgets. Desarrollar una pantalla de accesos sería un experimento razonable; que pueda abrir todas las apps, apropiarse de Home o iniciar al encender debe comprobarse aparte. No hay una solución lista y verificada que pueda prometerse aquí. Jellyfin es un cliente de un servidor multimedia; no sustituye Netflix.

## 4. Root y SamyGO

La [wiki J de SamyGO](https://wiki.samygo.tv/index.php?title=The_J_Series_Wiki) documenta métodos históricos por familia de firmware, no por la letra J en general. La [wiki de root H](https://wiki.samygo.tv/index.php?title=Rooting_H_series_TV) lista rangos de versiones y deja algunos límites de NT14M/NT14MJ sin resolver.

T-NT14MJ y T-N14MNUABC no son cadenas idénticas. La documentación revisada no permite afirmar que el método funcione con la versión del usuario. No encontré una recuperación y procedimiento exactos para ese equipo. No recomendaría downgrade, menú de servicio o scripts root genéricos como ruta práctica para arreglar Netflix. Antes de considerar actualizaciones en una investigación de root hay que registrar el estado actual, porque una actualización puede cambiar las posibilidades de modificación.

## 5. Qué encontré en Reddit y Twitter/X

### Reddit

No se localizaron casos exactos J4290/J4200 que acrediten Home permanente con firmware identificado. Esto significa que no encontramos evidencia, no que sea imposible.

En [este hilo de Orsay](https://www.reddit.com/r/revancedapp/comments/15nkel4/is_there_a_youtube_revanced_client_for_samsung/), agosto de 2023, FoxReis se identifica como desarrollador de TizenTube y relata que rooteó un Orsay, pero falló al modificar YouTube por certificados y falta de depuración remota. No informa modelo ni firmware. Es una experiencia directa útil para distinguir root de compatibilidad real de apps; no valida un exploit para J4290.

[Este caso de Samsung 2012](https://www.reddit.com/r/4kTV/comments/1cztg1l) relata uso satisfactorio de Chromecast después de perder Netflix. No corresponde a J4290. [Este hilo de Netflix](https://www.reddit.com/r/netflix/comments/1bi3km8/netflix_wont_load_on_samsung_smart_tv/) muestra resultados variados con reinicios y reinstalación, por lo que no hay un arreglo universal deducible de testimonios.

### Twitter/X

Se buscaron UN32J4290, J4290, Orsay, gathint y tiendademedusas mediante indexación pública. Los accesos directos al perfil de Rafa y al post enlazado desde CleanYourTV no devolvieron contenido utilizable. No se obtuvo una búsqueda interna completa de X ni todas las respuestas del post.

El sitio del autor [cleanyour.tv](https://cleanyour.tv/) reproduce el post de Gathin y confirma que el producto es para Android/Google TV. Para el caso Rafa solo contamos aquí con el texto pegado por el usuario: afirma Tizen 9, instalación de una app firmada y reasignación Home; no incluye modelo completo, comandos ni recuperación. Por eso no lo trato como procedimiento reproducible para J4290. No encontré un post verificable de X que demuestre esa modificación en UN32J4290.

## 6. Qué hacer según el objetivo

| Objetivo | Ruta razonable | Límite |
|---|---|---|
| Mejorar Netflix sin comprar | Reinicio en frío; distinguir navegación lenta de buffering; comprobar firmware y red. | No garantiza superar limitaciones de app/sistema. |
| Investigar sin comprar una home propia | Confirmar Orsay y disponibilidad del menú develop; probar primero un widget mínimo compatible. | No prometer reemplazo Home/arranque ni mejora de Netflix. |
| Tener apps modernas y home personalizable | Usar un dispositivo HDMI compatible con Netflix; si la personalización es central, evaluar Android TV/Google TV compatible con los proyectos. | Requiere hardware adicional; el sistema Samsung permanece instalado. |
| Root para eliminar procesos | Solo considerar con soporte exacto de modelo y firmware, y recuperación comprobada. | Actualmente no hay evidencia suficiente encontrada. |

Samsung recomienda reinicio en frío para problemas de apps: desconectar de la corriente 30 segundos y volver a probar. [Guía oficial](https://www.samsung.com/ar/support/tv-audio-video/how-to-troubleshoot-apps-that-are-not-working-on-the-samsung-smart-tv/). Para diagnosticar, medir por separado tiempo de respuesta del menú, apertura de Netflix y reproducción. Si la lentitud solo ocurre al cargar video, comparar Ethernet o una conexión alternativa ayuda a separar red de interfaz; es una prueba propuesta, no realizada.

No borraría ni resetearía Smart Hub como primer paso en una TV con apps antiguas, sin comprobar antes si todavía pueden descargarse y activarse. No se verificó que la tienda del equipo funcione hoy.

La alternativa HDMI separa el panel del software de streaming: la app corre en el dispositivo externo. Netflix publica una [lista de familias de dispositivos compatibles](https://help.netflix.com/es/node/14361); hay que validar el modelo concreto y soporte vigente antes de comprar. Si ya hay PC o consola compatible, se puede ensayar esa ruta sin comprar hardware.

## Datos pendientes y alcance

Faltan el modelo completo (sufijo regional), cadena completa de software y descripción de qué parte de Netflix falla. Se obtienen normalmente desde Menú > Soporte > Contacto Samsung. No hace falta compartir serie, MAC ni credenciales.

Se hizo investigación de fuentes públicas y lectura de documentación, con informes independientes de GitHub y Reddit revisados por el agente principal. No se conectó al televisor, no se escaneó la red, no se instaló software y no se alteraron apps o configuraciones. La ausencia de un procedimiento encontrado no demuestra imposibilidad técnica.

## Actualización: X consultado desde el navegador del usuario

Se accedió a X en Brave con la sesión existente del usuario, mediante navegación de solo lectura. Esta sección actualiza la limitación de acceso de la sección 5: ahora sí se leyeron resultados de búsqueda internos, el post original de Rafa y las respuestas visibles. No se publicaron mensajes, likes o marcadores.

### Búsqueda exacta UN32J4290

La pestaña Latest mostró una publicación del 23/11/2020 que menciona expresamente UN32J4290 y su falta de compatibilidad con Disney+. Se abrieron las dos imágenes: son capturas de un chat atribuido a soporte Samsung sobre la falta de disponibilidad de la app. No contienen instrucciones de modificación, firmware ni una identificación explícita Orsay del equipo. Otro resultado visible era un anuncio comercial de 2019. No se encontró allí una home modificada para este modelo.

Fuente: https://x.com/Evellyn_Gomes_A/status/1331034807460040709

### Rafa: fuentes directas y cronología

- Post original 20/10/2026: afirma home nueva, mejora de fluidez y root mediante modo desarrollador. El hilo no aporta comandos exactos ni modelo completo. https://x.com/tiendademedusas/status/2112486419692495057
- Mensaje anterior del mismo 20/10: había creado una app para sustituir la home, sin eliminar el resto, y la TV seguía muy lenta. El ID del post es anterior al anuncio de mejora; no debe presentarse como una retractación posterior. https://x.com/tiendademedusas/status/2112474213533815166
- Explicación 21/10: confirma Tizen 9 y PC Mac en la misma red. https://x.com/tiendademedusas/status/2112825325516136746
- Detalla desarrollo de app, Apps2Samsung y firma propia; llama ADB al puente. https://x.com/tiendademedusas/status/2112825328619888653
- Afirma que la asignación Home persiste al reiniciar. No publica el comando. https://x.com/tiendademedusas/status/2112825331711139944
- Buswe corrige a SDB/26101; Rafa confirma esa aclaración y dice que no se atreve a eliminar procesos en su televisor de 55 pulgadas. https://x.com/buswe_com/status/2112548940058935572 y https://x.com/tiendademedusas/status/2112551831918309511

Conclusión: se confirma el relato de un cambio en Tizen 9, no su reproducibilidad en J4290 ni que exista un debloat probado. La afirmación de root sigue sin demostración técnica en el hilo. No inferir una mejora de Netflix a partir de fluidez de la home.

### Orsay: otras fuentes dentro de X

- Samsung Brasil, 04/04/2022: explica que la compatibilidad de apps con Orsay depende de sus desarrolladores; distingue Orsay de Tizen. No es una respuesta sobre la UN32J4290 exacta. https://x.com/SamsungBrasil/status/1511066919796977676
- Samsung Brasil, 15/08/2021: indica que Disney+ no es compatible con Orsay. Es una declaración histórica específica de Disney+, no una prueba de imposibilidad de instalar cualquier widget. https://x.com/SamsungBrasil/status/1426979545375199232
- Simba, 20/10/2026: anuncia free-my-tv para Android 9+ y trabajo de adaptación para Samsung Tizen/LG webOS. https://x.com/SimbasJungles/status/2112525805364367515
- Un usuario pregunta por soporte para Orsay H2014; al abrir esa conversación no había una respuesta visible que confirmara soporte. No convertir el anuncio de Tizen en soporte Orsay. https://x.com/brunopcason/status/2112605383759986803

Consultas UI: UN32J4290 en Latest; from:tiendademedusas (Samsung OR Tizen OR codex); Orsay Samsung en Top y Latest. Se leyeron los artículos visibles y se abrieron las fuentes relevantes. No se afirma cobertura exhaustiva de todo X.

## Segundo material pegado: Free My TV comprobado en GitHub

El README actual de https://github.com/simbasjungles/free-my-tv ya incluye carpeta tizen y una versión Samsung. Su requisito explícito es Samsung 2017+ con Tizen 3+. Instala una app .wgt mediante TizenBrew; mantiene la home Samsung y propone Autorun Last App para iniciar en la app. No es soporte Orsay ni reemplazo nativo universal de Home. La sección Any TV remite a un streamer Google TV por HDMI para las otras teles. Este dato actualiza el anuncio de adaptación futura visto en X: la versión Tizen está documentada ahora, pero no habilita J4290/Orsay.

Los mensajes HBO de 2021 pegados por el usuario son evidencia histórica de retiro del servicio HBO España en Orsay; no prueban el estado de Netflix. El comentario de Grok sobre un navegador de UE40ES5505 es una respuesta de otro modelo de IA sobre otro televisor: no es prueba técnica para UN32J4290. El testimonio de Globoplay sobre apps light corresponde a ese proveedor, no una actualización general de Orsay.

## Conclusión operativa (21/10/2026)

Para una UN32J4290 no recomiendo intentar replicar el video de Rafa. Esa TV pertenece a la generación Orsay, mientras que el video usa Tizen 9 y herramientas SDB/Apps2Samsung. Samsung publica para la variante J4290AG firmware T-N14MNUABC 1030.0 (la ficha no demuestra qué versión está instalada). En el hilo técnico de Emby, el administrador responde al usuario de una J4290 que la familia J4200 corre Orsay, por lo que una app Tizen no se instala. Evidencia directa de familia/modelo, aunque no es una declaración oficial de Samsung sobre todos los sufijos regionales. [Samsung Perú: UN32J4290AG](https://www.samsung.com/pe/support/model/UN32J4290AGXPE/) · [Emby: Samsung Smart TV J4290](https://emby.media/community/topic/111104-samsung-smart-tv-j4290/).

### Respuesta a “¿puedo hacer esto?”

- **Sí se puede experimentar con widgets Orsay:** Apps2Samsung conserva un instalador legado para Orsay 2011–2015. Describe servidor local, cuenta `develop`, IP del servidor y sincronización desde Smart Hub. El repositorio quedó archivado y marcado EOL el 6/6/2026. Esto es sideload de widgets; no concede root ni ofrece una opción documentada para sustituir Home o arrancar automáticamente. [Instalador Orsay archivado](https://github.com/Apps2Samsung/Jellyfin-Orsay-Installer).
- **No hay una receta probada para una home nueva persistente en J4290.** No hallé repo, firmware ni testimonio reproducible que muestre remapeo de Home/autoarranque en ese modelo. El soporte para widgets tampoco prueba esa capacidad.
- **No instales CleanYourTV, tv-makeover, Free My TV Tizen, Apps2Samsung Tizen, CleanFrame ni arn0 en esta TV:** los primeros Android TV; los demás Tizen; ninguno soporta Orsay/J4290.
- **No sigas “activar developer = root”.** En Orsay existe un menú developer comunitario para sincronizar widgets, pero no equivale al modo developer de Tizen 9 ni a privilegios root.

### Recomendación para Netflix

Primero distinguí si se traba el menú, si Netflix queda cargando o si el video se corta. La guía actual de Netflix para Smart TVs Samsung recomienda desenchufar el TV y dejarlo sin corriente 3 minutos; luego probar de nuevo. Si solo falla Netflix, ofrece cerrar sesión y entrar otra vez. Si sigue, indica revisar DNS/red con el proveedor y pedir a Samsung asistencia para firmware/Smart Hub. [Netflix: congelamiento o carga infinita en TV Samsung](https://help.netflix.com/es/node/100242). No haría reset de fábrica de entrada en este TV viejo: puede dejarte sin una tienda que no verificamos que todavía permita volver a descargar apps.

Si el problema es que **la app o reproducción es demasiado lenta**, cambiar la portada no va a cambiar el decodificador, DRM, memoria ni versión de Netflix. La salida más fiable es correr Netflix desde un equipo HDMI compatible que ya tengas (PC/consola/streamer), dejando la TV como pantalla. Comprar un equipo nuevo es decisión posterior; validar modelo y soporte vigente de Netflix antes.

### Datos que cierran la variante, sin compartir información sensible

En Menú > Soporte > Contacto Samsung anotá solo **Model Code** y **Software Version**; no compartas número de serie ni MAC. Si aparece UN32J4290AGXUG/AGXPE/AFXZX u otro sufijo, permite cotejar la ficha regional correcta. No flashees el archivo 1030.0 encontrado en la ficha peruana sin que coincidan modelo completo y región.

### Fuentes adicionales recién verificadas

- La ficha oficial Samsung Perú llama al producto “J4290” y publica archivo `T-N14MNUABC.zip`, versión 1030.0, fecha 22/06/2019. [Samsung Perú](https://www.samsung.com/pe/support/model/UN32J4290AGXPE/).
- El hilo Emby relata que la app Tizen no instala en el J4290; su administrador identifica J4200 como Orsay y deriva a la app Orsay comunitaria o equipo externo. [Hilo de soporte](https://emby.media/community/topic/111104-samsung-smart-tv-j4290/).
- Netflix publica diagnóstico específico de carga congelada para Samsung, incluido el ciclo de desconexión, cierre de sesión y escalado de red/fabricante. [Centro de ayuda Netflix](https://help.netflix.com/es/node/100242).
- La ficha Samsung permite ver que el firmware publicado es de 2019, no afirma que 1030.0 esté instalado en la unidad del usuario ni garantiza futuras actualizaciones.

## Alternativas reales encontradas tras ampliar la búsqueda (21/10/2026)

Corrijo la conclusión demasiado tajante anterior: sí hay formas de personalizar la experiencia en el J4290, aunque ninguna fuente demuestra que se pueda reemplazar el Smart Hub nativo igual que en Tizen 9.

### A. Hacer que la pantalla actual sea más simple (opción segura)

El manual de usuario de J4290 describe la pantalla **First Screen** de Smart Hub, donde se muestran elementos favoritos, apps y fuentes. Para subir Netflix (o cualquier item compatible) a Favoritos: abrir Smart Hub, enfocarlo, mantener presionado el botón izquierdo del mando y elegir **Add to Favorites**. El favorito pasa al principio de la lista. [Manual UN32J4290](https://www.notice-facile.com/en/manual/1043191/samsung%2Bun32j4290ah).

Samsung documenta que modelos 2015 tienen opciones Auto-start de Smart Hub (el menú exacto varía por región/firmware). Eso automatiza abrir Smart Hub, no instalar ni arrancar cualquier widget como launcher. [Samsung: opciones de inicio automático](https://www.samsung.com/au/support/tv-audio-video/use-autorun-start-screen-options-on-your-samsung-tv/).

Esta es la mejor forma de lograr ya una pantalla más directa: dejar Netflix arriba en Favoritos y usar la pantalla inicial de Smart Hub. No elimina el sistema Samsung ni cambia el comportamiento del botón Home.

### B. Instalar un widget Orsay con interfaz propia (experimento posible)

Orsay admite instalar widgets de usuario por USB en `userwidget` o por sincronización contra un servidor local desde Developer Settings. En guías comunitarias, después de instalarlo aparece en **Mis aplicaciones** y luego en **Recientes** de Smart Hub. Un widget hecho para el TV podría presentar una página visual limpia a pantalla completa. Fuentes: [guía de instalación comunitaria Orsay de Emby](https://emby.media/community/topic/9869-samsung-orsay-smarttv-2011-2015-community-app-install-instructions/) y [instalador Orsay archivado](https://github.com/Apps2Samsung/Jellyfin-Orsay-Installer).

Límite importante: no hallé evidencia específica de que un widget de usuario pueda interceptar Home, ejecutarse al encender en el J4290, o invocar directamente cualquier app instalada. Por eso esto sería una **app propia que se abre desde Smart Hub**, no un reemplazo confirmado del launcher. Se puede construir una pantalla como tablero con Netflix como acceso destacado, pero validar el lanzamiento directo de Netflix requiere probar en el equipo.

### C. Root SamyGO: una posibilidad técnica sin compatibilidad confirmada

Apareció un matiz que no estaba bien expresado en la primera investigación. SamyGO documenta un método de root “Network” para la familia H y dice que contempla plataformas `NT14M` con un límite de firmware aún desconocido (`NT14M < ???`). El firmware publicado para variantes J4290 es `T-N14MNUABC-1030.0`, nombre que parece pertenecer a esa rama NT14M, pero no encontré ningún caso J4290/1030.0 confirmado como root. Las instrucciones J-root para `T-HK*` (hasta 1480) y `T-NT14MJ*` son ramas diferentes; no se deben aplicar por semejanza de nombre. [SamyGO H Network root](https://forum.samygo.tv/viewtopic.php?t=9622) · [Wiki SamyGO J](https://wiki.samygo.tv/index.php?title=The_J_Series_Wiki).

Ese método antiguo requiere Wi-Fi, USB, ajustes de servicio y bloquear actualizaciones; SamyGO advierte sobre cuelgues y riesgo de brick con procedimientos incorrectos. Aunque se confirmara root, no encontré una técnica documentada para reasignar el botón Home de Orsay a una app, como se reportó en Tizen 9. Así que root no cierra la brecha por sí solo y no recomiendo probar esos pasos con el dato actual.

## Verificación adicional y adaptación local (21/10/2026)

La tabla vigente de Samsung Developer resuelve parte de la duda del sistema: su grupo de modelo **UJ4290** aparece bajo “Samsung Legacy Platform”, SDK 5.1. No es la grafía completa `UN32J4290...` ni identifica la variante regional, pero coincide con la familia informada. Samsung indica que 2015 tuvo excepciones a la regla Tizen; la J4290 no debe clasificarse por el año solamente. Junto al hilo específico de Emby (J4290/J4200 y Orsay), esto refuerza que hay que usar compatibilidad Legacy/Orsay, con la salvedad de confirmar Model Code y Software Version en el propio aparato. [Samsung model groups](https://developer.samsung.com/smarttv/develop/specifications/tv-model-groups.html) · [Samsung platform Q&A](https://developer.samsung.com/smarttv/develop/faq/specifications-and-feature-support.html) · [Emby J4290](https://emby.media/community/topic/111104-samsung-smart-tv-j4290/).

Hay una forma documentada de pedir a un widget Orsay que ejecute otra app: `Common.API.Widget.runSearchWidget(appID, extraInfo)`, desde Samsung Smart TV 2.265. Eso corrige la afirmación previa de que Orsay no ofrecía ninguna llamada de lanzamiento. Su limitación es que exige conocer el ID; la API no descubre qué aplicación está instalada ni garantiza que el ID global funcione en cada región/firmware. Una lista comunitaria registra el Netflix Orsay clásico `11101200001`, pero no hallé una prueba de esa llamada en `UN32J4290` con el firmware del usuario. [Samsung Application Development Guide, §5.4.8](https://freethetv2011.s3.amazonaws.com/App_Development_Guide_for_Samsung_Smart_TV%5BV1.20%5D%5B1%5D.pdf) · [tabla comunitaria de app IDs](https://docs.thecloudportal.com/en/screens/nativeapps).

Preparé [`Clean Home Orsay`](cleanyourtv-orsay/README.md), una app widget nueva con interfaz remota y un intento de abrir Netflix por `runSearchWidget`; si el API o el ID no aplican, ofrece abrirlo desde Smart Hub. El paquete local está en [`CleanHome-Orsay.zip`](CleanHome-Orsay.zip), y `Prepare-Server.ps1` arma el `widgetlist.xml`/directorio para servir en red local. No deriva código Android del CleanYourTV. El formato se basa en las plantillas históricas de Samsung y el instalador comunitario Orsay archivado. No está probado en TV real. El ZIP se instala como una aplicación aparte y no reemplaza Home, no remapea la tecla Home y no acelera Netflix.

También encontré que **Auto Ticker** pudo iniciar widgets en algunas generaciones/regiones Orsay, y se usó en foros para series F con condiciones adicionales. No hay caso para J4290. El widget conserva `<ticker>n</ticker>` y no modifica arranque. [respuesta técnica Auto Ticker, 2015](https://stackoverflow.com/questions/12780239/background-application-on-smart-tv) · [caso F/SamyGO](https://forum.samygo.tv/viewtopic.php?t=8760).

La pantalla del UN32J4290 es HD 1366×768 en la ficha del modelo; el widget usa un único escalado al viewport (se quitó una regla CSS que podía reducirlo una segunda vez). Eso valida la corrección lógica, no el render en el televisor. [Samsung UN32J4290AF](https://www.samsung.com/mx/support/model.UN32J4290AFXZX/).

### Recomendación final revisada

1. Reordenar Smart Hub y agregar Netflix a Favoritos (sin instalar nada).
2. Si querés una pantalla visualmente propia, desarrollar un widget Orsay y agregarlo como acceso de Smart Hub; su instalación es experimental y no promete remapear Home.
3. Si el objetivo principal es Netflix lento, una app corriendo en hardware HDMI es la solución más efectiva, porque el launcher no puede actualizar el motor de streaming ni la memoria de la TV.

No encontré en X, Reddit, GitHub ni SamyGO a nadie que haya probado una sustitución persistente de Home en el UN32J4290/T-N14MNUABC. Esa afirmación es sobre la evidencia pública revisada, no sobre imposibilidad matemática.

## Corrección operativa: el menú de instalación no es igual en todos los Orsay

La instrucción anterior de iniciar sesión con el botón rojo y abrir un menú de desarrollo con el azul corresponde a una variante documentada para D-series; fue demasiado general para recomendarla como el camino del J4290. En una guía comunitaria de **algunos** modelos H y J, el acceso se hace desde Smart Hub iniciando sesión como `develop`, luego se mantiene presionado Enter sobre una app en la lista de apps para abrir opciones de IP; se vuelve a mantener presionado Enter y se elige **Start User app sync**. Esa guía enumera J4300, J5200, J5300 y J6200; **no enumera J4290**, así que no demuestra que esos menús aparezcan en esta TV. [Emby: instrucciones J-series seleccionadas](https://emby.media/community/topic/9869-samsung-orsay-smarttv-2011-2015-community-app-install-instructions/).

Otra guía Orsay describe instalar por USB: crear `userwidget` en la raíz, poner ahí el ZIP del widget, insertar el USB y revisar Samsung Apps/Mis Apps. Advierte que no sirve en todos los modelos. Eso respalda una prueba de bajo impacto, pero tampoco es una certificación del J4290. [Guía comunitaria Orsay USB/Sync](https://mi-device-configuration-manuals.readthedocs.io/en/latest/samsung.html).

La ficha oficial disponible para una variante UN32J4290AH identifica ese modelo y publica el firmware `T-N14MNUABC` 1030.0; no documenta una opción Developer/Sync en su manual público. La interfaz exacta debe tratarse como firmware/variante-dependiente. [Samsung Support UN32J4290AH](https://www.samsung.com/latin_en/support/model/UN32J4290AHXZE/).

El widget está pensado como app separada; no cambia el botón Home ni las cuentas/servicios instalados. No se conectó la TV ni se comprobó que aceptara el paquete.


