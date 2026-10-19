# Validación 0.14.6

Código del instalador: `e153b103762a0ddb960507ff548e17460f7de76a`. Versión instalada y publicada: 0.14.6, edición Equipos IA.

## Conversación con el Lead

El composer acepta mensajes con tareas activas o bloqueadas. Las preguntas habituales de estado se responden con datos guardados, sin inferencia; no crean ejecuciones, intentos ni permisos. Las demás consultas usan una sesión del Lead sin herramientas; una petición explícita de trabajo se deriva al scheduler. Las indicaciones se incorporan a los siguientes pasos, sin interrumpir una llamada en curso. Reintentar conserva las tareas terminadas y utiliza los controles de reconciliación existentes. Los mensajes y sus sesiones se guardan antes de enviar; una respuesta interrumpida al iniciar se marca como tal sin reenviarse.

Se conservan los comandos declarados por cada agente y se comparan con los resultados observados. El aviso identifica responsable y tarea; los comandos y resultados completos quedan en los detalles. No se relaja la verificación para ocultar un bloqueo.

## Pruebas

- 56 pruebas unitarias de frontend y 77 pruebas de interfaz aprobadas. Tras los últimos ajustes de presentación se repitieron las 15 de equipos y cuentas, todas correctas.
- Rust con `desktop-e2e`: 121 aprobadas, 0 fallidas y 7 ignoradas por requerir harness o capacidades específicas.
- Harness nativo con OpenCode real e inferencia determinista local: una explicación al Lead no inicia trabajo y no ofrece herramientas; el pedido siguiente inicia un equipo automático con dos Backend y un Frontend en paralelo. Una consulta de estado no duplica la ejecución. Se verifican escrituras, comandos observados, mensajes y roster persistidos.
- Recuperación con terminación real del proceso: solicitud al Lead y sesiones paralelas conservadas; misma sesión pendiente retomada, efecto aplicado una sola vez y entrega reabierta.
- Build de escritorio y PWA, lint sin errores y coherencia de versiones correctos. Permanecen advertencias existentes de lint y compilador.

## Actualización instalada y prueba real

Firma Minisign verificada con la clave pública del updater. SHA256 del instalador: `1a0eb4f47154656468d69264a2f37631b04fa7ed32295f9b72ddcb58ca48e69b`. NSIS terminó con código 0. El hash canónico del ejecutable instalado coincide con el compilado.

Se guardó un backup privado consistente antes de instalar. Después se compararon completos ambos documentos de equipos, permitiendo únicamente los campos nuevos vacíos: los siete chats de cada perfil quedaron conservados. También se compararon exactamente las 46 sesiones, 844 mensajes y 3082 partes existentes del motor; todas las filas quedaron iguales y SQLite íntegro. Esta comparación se realizó antes de las nuevas conversaciones de validación.

Con Computer Use, en la aplicación instalada se preguntó «¿Qué falló y qué falta?» sobre el bloqueo real de QA por cuota. La respuesta identificó responsable, tarea y causa, y quedó guardada sin crear una sesión de inferencia ni modificar los intentos. Después se pidió «Corregilo y reintentá»: se conservó una sola ejecución y las tres tareas terminadas; solo QA recibió otro intento. QA ejecutó `node --test tests/http.test.js`, registró salida 0, pasó la revisión individual y el cierre global. La entrega quedó en **Pendiente de QA manual**, sin atribuir al usuario una aprobación.

Otra pregunta sobre el papel de cada integrante recibió una respuesta breve normal del modelo de producción, sin JSON visible ni tareas nuevas. Tras cerrar y relanzar la aplicación se comparó el documento completo: quedó exactamente igual, con seis mensajes conversacionales, una ejecución y los mismos intentos. La conversación y la entrega se volvieron a abrir desde la interfaz.

La demo se verificó también directamente: 12 pruebas HTTP aprobadas, incluidos CORS, OPTIONS, errores 404/405, conflictos de puertos y limpieza tras fallos controlados. En el navegador real se abrió el frontend y se pulsó Volver a consultar; se vieron Hola mundo y ok, de ambos servicios. Los servidores de validación propios se cerraron después. Las capturas reales anteriores y posteriores quedan privadas en la PC, fuera de GitHub. La captura externa del verificador no se presenta como si la hubiera realizado el runner ni sustituye el QA manual del usuario.

## OpenCode Zen

Se verificó en la app instalada su sección independiente con campo enmascarado. Las pruebas de interfaz conectan únicamente Zen, seleccionan sus modelos gratuitos o de pago y conectan Go después sin sobrescribir su clave. El catálogo nativo acepta Zen sin Go. No se proporcionó una clave real de Zen: no se declara validada una generación ni un saldo de pago.

Los cuatro artefactos firmados se publicaron con el workflow [Publicar GuilleCode 0.14.6](https://github.com/Kaitach/guillecode/actions/runs/39287774263), que terminó correctamente y verificó manifiesto, firma declarada y SHA256 descargado. La compilación y las demás pruebas anteriores se ejecutaron localmente; no se atribuyen a una preparación de release en CI.
