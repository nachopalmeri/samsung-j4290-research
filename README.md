# Samsung UN32J4290: compatibilidad, streaming e investigación

Esta carpeta reúne la investigación técnica y las pruebas realizadas para evaluar CleanYourTV en un Samsung UN32J4290. La evidencia combina documentación oficial de Samsung y proveedores, manuales de la familia, referencias históricas del SDK Legacy, proyectos comunitarios y pruebas reproducibles en Chrome/Node.

**Conclusión principal:** Samsung clasifica el grupo UJ4290 como Samsung Legacy Platform (SDK 5.1), también llamado Orsay en la comunidad. No es Android TV ni un Samsung Tizen moderno. El conjunto de informes no demuestra que Netflix, Disney+, Max, MUBI, Flow, Brave o Stremio puedan ejecutarse como servicios nativos en esta unidad. El prototipo de CleanYourTV implementa reproducción genérica para fuentes multimedia directas; el video HTML5, el plugin Samsung, codecs, red TLS, control remoto y persistencia todavía requieren validación en la TV física.

## Informes

- [Investigación principal del modelo, apps, X y rutas comunitarias](investigacion-principal.md)
- [Hardware, firmware, SDK y reproducción autónoma](hardware-streaming.md)
- [Matriz de compatibilidad de servicios y plataforma](matriz-servicios.md)
- [Síntesis previa de compatibilidad Samsung](compatibilidad-samsung.md)
- [Investigación de repositorios GitHub](github.md)
- [Experiencias de Reddit](reddit.md)
- [Prueba reproducible de OldTube](oldtube-prueba.md)
- [QA del reproductor](qa-reproductor.md)
- [QA de navegación e interfaz](qa-interfaz.md)

## Cómo interpretar los resultados

- **Confirmado por documentación:** el fabricante o el proveedor publica esa capacidad para la plataforma o modelo indicado.
- **Hallazgo comunitario:** un repositorio, foro o publicación describe un método; no certifica la variante ni el firmware de esta TV.
- **Probado en escritorio:** la función se ejercitó en Chrome/Node con el entorno indicado; eso no equivale a la ejecución en Orsay.
- **Pendiente en hardware:** requiere probarse en el Samsung físico, sin extrapolar resultados de otra generación.

Los informes señalan expresamente sus límites temporales y de evidencia. Las páginas externas citadas son las fuentes originales. No se publica aquí el prototipo, el paquete de instalación, credenciales, IPs locales ni archivos intermedios del entorno de desarrollo. No se instaló nada en la TV ni se modificó su firmware, Smart Hub o red durante estas pruebas.

Fecha de corte de la investigación: 22 de octubre de 2026.

