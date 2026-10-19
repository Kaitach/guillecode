# Validación 0.14.7

Código del instalador publicado: `1ccb82483bc12817d385415f8ef8df5e80632755`. Versión instalada: 0.14.7, edición Equipos IA.

## Adjuntos y configuración

El composer acepta imágenes por selector, pegado y arrastre, aun sin texto. Las referencias originales quedan en `team-images`, con metadatos, tamaño y SHA256 en SQLite. Solo se recuperan imágenes registradas mediante su ID; se comprueba ruta, tamaño y hash. Se admiten PNG, JPEG, WebP y GIF, hasta 5 imágenes y 20 MB en total, con 10 MB por archivo. La validación de entrada comprueba firma y MIME; el motor decodifica la imagen. No se declara una validación completa del formato mediante un decodificador local.

El Lead recibe partes de archivo reales en su consulta; las imágenes pasan al scheduler cuando la conversación se convierte en trabajo y llegan a sus pasos de planificación, especialistas y revisión. Las imágenes del usuario se tratan como referencias y no como capturas de la entrega.

Configuración muestra proveedor, ID/nombre de modelo, razonamiento y soporte de imagen de cada integrante de la ejecución elegida. Conserva el modelo histórico de cada intento. Es una vista de consulta. Al abrirla se vuelve al principio para ver primero al Lead.

## Comprobaciones automáticas

- 56 unitarias de frontend aprobadas; Rust con `desktop-e2e`: 123 aprobadas, 0 fallidas y 7 ignoradas porque requieren un harness o capacidades específicas.
- Harness nativo con OpenCode real y respuestas deterministas locales aprobado: imágenes reales en conversación, planificación, sesiones paralelas, revisión y cierre. Se verifican herencia al trabajo posterior, referencias persistidas, lectura por ID, rechazo de traversal y archivo alterado.
- 12 E2E de equipos aprobadas, incluidas adjuntar/quitar, pegar, enviar sin texto, recuperar preview y mostrar modelos/variantes históricos. Repetidas también sobre la rama del PR original. Después del ajuste de desplazamiento se repitieron las tres de imágenes/configuración y la de configuración sobre la rama upstream; todas aprobadas.
- Suite completa: 79 aprobadas directamente y una aprobada al reintentar. El test existente `bubble-context` del cursor transitorio de streaming también mostró esa inestabilidad en una repetición aislada. No se atribuye una corrida totalmente limpia ni una corrección de ese problema ajeno a este cambio.
- Builds desktop y PWA y coherencia de versiones correctos; lint sin errores, con advertencias existentes.

## Instalación y datos conservados

Firma Minisign del instalador final verificada con la clave pública del updater. SHA256 del instalador: `ba29448a044425cfa786eef6f4dc0155b2d2b8134d4dbcb184a19f59442cb16b`. Instalación NSIS terminada con código 0. Hash canónico del ejecutable instalado: `85b0569ca33e284c3e40d337f2bad2e7f8ad68736a978d77d389598eada763e5`, igual al compilado.

Se creó un backup privado consistente. La actualización desde 0.14.6 conservó completos los nueve chats de ambos perfiles, permitiendo únicamente los campos nuevos vacíos de imágenes. Las 52 sesiones, 863 mensajes y 3142 partes existentes del motor quedaron exactamente iguales, con SQLite íntegro. La comparación se realizó antes de las conversaciones nuevas de validación.

## Prueba real con Computer Use

En la app instalada se abrió Configuración y se verificaron Lead, Frontend 1, Backend 1 y QA 1 con su modelo real `openai/gpt-6.1-sol-fast`, razonamiento automático y soporte de imágenes. El selector global del chat normal usaba otro modelo; la vista muestra correctamente los modelos propios del equipo.

Una captura de prueba antigua tenía extensión PNG aunque sus bytes eran JPEG. El guard nativo la rechazó con un mensaje legible sin borrar el borrador. Se usó una copia idéntica con extensión JPEG correcta y se adjuntó mediante el diálogo nativo de Windows. El Lead de producción respondió en una frase describiendo correctamente el fondo celeste, Hola mundo, Saludo/Estado y Volver a consultar. Se mantuvo una ejecución y los mismos intentos; la consulta no inició trabajo ni aprobó QA manual.

Antes de instalar/reabrir la compilación final se hizo otro backup que incluye `team-images`. Tras el reinicio se compararon los documentos completos de ambos perfiles, sin excepciones: exactamente iguales, con diez mensajes en la conversación probada y la imagen idéntica en bytes y SHA256. También quedaron iguales las 53 sesiones, 865 mensajes y 3147 partes del motor. Se reabrió el team chat desde la interfaz y se vieron miniatura, nombre y respuesta; Configuración abrió desde arriba mostrando primero al Lead.

Las capturas reales anteriores y posteriores están en la carpeta privada de evidencias 0.14.7 de la PC; no se subieron imágenes de perfiles ni credenciales a GitHub.

## Publicación

[Release 0.14.7](https://github.com/Kaitach/guillecode/releases/tag/v0.14.7) publicada con sus cuatro artefactos. [Workflow de publicación](https://github.com/Kaitach/guillecode/actions/runs/39297002209) terminado correctamente: comprueba los artefactos descargados, el manifiesto y el SHA256. La verificación criptográfica del instalador y las pruebas de código se realizaron localmente; no se atribuyen a ese workflow.

La primera build 0.14.7 se publicó antes de fijar el canal al repositorio original. La instalación local fue reconstruida y reinstalada desde `1ccb824` con el manifiesto y la clave oficial de `GuillermoPastoriniRivas/guillecode`. El workflow de publicación fue cerrado en este repositorio para impedir nuevas releases del fork. Las instalaciones antiguas que no recibieron esta build conservan la configuración que tenían hasta que instalen una versión nueva.

[PR #8 al repositorio original](https://github.com/GuillermoPastoriniRivas/guillecode/pull/8), basado en su main posterior al merge del PR #7. Incluye las funciones, pruebas y documentación de uso, conservando versión, clave y canal de actualizaciones originales. Al consultar el PR quedó abierto y sin conflictos; no había checks de CI registrados.
