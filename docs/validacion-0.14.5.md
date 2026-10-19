# Validación local 0.14.5

Código: `270498b`. Instalador local firmado e instalado en la PC; esta validación no implica publicación de una release nueva en GitHub.

El botón **Modo equipo** abre un chat directamente, sin plantilla. El Lead puede agregar hasta ocho especialistas con el modelo y la variante del chat, incluidas varias personas del mismo rol. Los agentes y las tareas se validan juntos antes de guardarse; una incorporación inválida no reemplaza trabajo activo ni amplía carpetas o herramientas. Las plantillas manuales conservan su funcionamiento.

Las respuestas del Lead muestran **Qué hice**, **Qué probé** y **Qué falta**, con cada campo limitado en pantalla y el informe completo dentro de **Detalles técnicos**, cerrado inicialmente. Los avisos de criterios incumplidos y QA manual prevalecen sobre un resumen optimista. Los historiales anteriores conservan su texto original; los chats normales siguen mostrando su JSON sin transformaciones.

## Verificaciones

- 56 pruebas unitarias de frontend aprobadas.
- 75 pruebas de interfaz aprobadas. Tras el último ajuste del resumen histórico se repitieron las 24 de equipos y recuperación, todas correctas.
- Rust con `desktop-e2e`: 118 aprobadas, ninguna fallida, 7 ignoradas por requerir harness nativo o capacidades específicas.
- Build de escritorio y PWA, lint sin errores y coherencia de versiones aprobados.
- Harness con OpenCode real y scheduler Rust: el Lead propone dos Backend y un Frontend sin plantilla; las tres sesiones empiezan antes de que termine ninguna, escriben archivos propios y ejecutan comprobaciones observadas. Se conserva la carpeta sin Git y se guardan integrantes y resumen breve en SQLite. La inferencia es determinista local, sin utilizar una cuenta de producción.
- Recuperación automática con terminación real del proceso: integrantes elegidos, solicitud al Lead y sesiones de Backend y Frontend sobreviven al cierre. Reconciliar conserva las mismas sesiones y el efecto se aplica exactamente una vez; se reabre la entrega final con su resumen breve.
- El workflow de preparación incorpora ambos escenarios nuevos para las próximas releases.

La prueba con modelos de producción detectó que `terminal_run` podía ejecutar un test correctamente sin que el runner lo incluyera en sus comandos observados. Se agregó un recibo nativo al comienzo del resultado de `terminal_run` y `terminal_read`, emitido a partir del estado de finalización del shell. El runner conserva el comando original, vincula la lectura posterior con la misma terminal y el mismo comando ejecutado, y solo acepta un código numérico de salida satisfactorio. No acepta logs que digan PASS, resultados de otros servidores MCP, lecturas sin una ejecución previa, comandos reemplazados, esperas, códigos desconocidos ni fallos. Ambos harness de equipo automático y recuperación se repitieron tras la corrección.

## Instalación y datos

Se verificó la firma Minisign del instalador con la clave pública del updater. SHA256 del instalador final: `3bafde90701a102e44a39621b7dcf9fbafbaefdafe0b3bb5c0507b6df431ef9f`. La instalación NSIS terminó con código 0 y relanzó GuilleCode; el hash canónico del ejecutable instalado coincide con el compilado de `270498b`.

Antes de instalar se hizo un backup privado consistente de SQLite y archivos del perfil. Después se verificó la integridad y la conservación de los dos perfiles: seis chats y cuatro plantillas en cada uno; el motor mantuvo 27 sesiones, 725 mensajes y 2691 partes. Se comparó el documento completo ignorando únicamente los campos nuevos de presentación y modo automático. La comparación se realizó antes de crear el chat nuevo de prueba.

Con Computer Use se comprobó en la aplicación instalada que el mismo historial antiguo pasó de párrafos extensos a tres bloques breves y detalles plegados. Las capturas reales anteriores y posteriores se conservan localmente; no se publican datos del perfil en el repositorio.

En una carpeta nueva sin Git, desde la interfaz se abrió **Modo equipo** sin crear una plantilla y se pidió un frontend con dos servicios independientes. El Lead decidió incorporar Frontend, Backend y QA: eligió un Backend para ambos servicios, en lugar de un número fijo de personas. Los tres trabajos independientes arrancaron con 40 ms de diferencia; la integración quedó como tarea dependiente. Se observaron las sesiones reales y el equipo persistido tras reiniciar.

Antes de instalar la corrección de terminal se guardó otro backup consistente. Después se compararon exactamente ambos documentos de equipos y todas las filas de sesiones, mensajes y partes del motor: quedaron iguales. La instalación conserva también el chat y el trabajo parcial creados para esta prueba.

La repetición real conservó Backend y preparación de QA sin repetirlos. Frontend volvió a ejecutar la integración y quedó aprobado. La tarea final de QA se bloqueó por el límite de uso de la cuenta de producción; no se declara una entrega final aprobada ni una comprobación visual del navegador. Ese bloqueo sirve como caso real para comprobar el diagnóstico conversacional de 0.14.6.
