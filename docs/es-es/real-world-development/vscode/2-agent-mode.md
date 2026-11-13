---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, actualizarlo... y así sucesivamente.

El modo agente de Copilot te ayuda con este proceso.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo puedes utilizar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y cloud agent ofrecen experiencias distintas. El modo agente trabaja en tu IDE y permite ciclos de respuesta rápidos e interactivos. Cloud agent trabaja de forma asíncrona como otro miembro del equipo e interactúa contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos relevantes y coordinar cambios.
- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de fusionarlos en la base de código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz de usuario del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito específico que utiliza las API DOM estándar.

### Inicia el sitio web

Para facilitar la ejecución del sitio web, hay una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Puedes ejecutarlo en GitHub Codespaces siguiendo estos pasos:

1. Vuelve a tu Codespace. Tras el ejercicio anterior, deberías estar en `main`.
2. Abre una ventana de terminal nueva en Codespaces pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Vuelve a `main`, actualízala y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con la URL, parecido al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace de una URL localhost del terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio en ejecución, puedes explorar sus funciones. Entre las principales se encuentran:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accedes a una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora la lista de tareas con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso la indicará cuando sea pertinente.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Empecemos por revisar la lista de tareas. Al crear el proyecto a partir de la plantilla, se generó una lista de incidencias automáticamente. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en la lista de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Utilizamos Claude Sonnet 4.5 para crear el taller y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea adecuada. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, es posible que las sugerencias sean distintas a las que se muestran en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía este prompt a Copilot para preguntarle por la lista de incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales para añadir funcionalidad. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a archivos de instrucciones específicos para distintos tipos de archivo, entre ellos:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar pautas, dividir conjuntos grandes en otros más pequeños y ofrecer ejemplos y plantillas a Copilot.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios distintos en la aplicación:

- Añadir o mejorar la lógica de filtrado en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento del filtrado.
- Actualizar la página con la lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de fusionar los cambios. El modo agente de Copilot puede realizar estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos asociados a la funcionalidad. Debería encontrar los helpers de la capa de datos, la interfaz de usuario y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y la base de código. Mientras tanto, puede que aparezcan los controles **Keep** y **Undo** en el editor. Cuando termine, tendrás un control **Keep** o **Undo** para cada cambio; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que se produzcan pausas o que fallen algunas pruebas durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión completada con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web. ¡Explora la nueva funcionalidad!
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funcionalidades al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- el modo agente puede explorar el proyecto, identificar archivos relevantes y coordinar cambios.
- revisar los cambios y las pruebas generados por el modo agente antes de fusionarlos en la base de código.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que la implemente en el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluido el uso de los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si este será fijo.
- al crear el prompt, proporciona a Copilot las instrucciones necesarias para obtener la implementación que quieres.
- es posible que tengas que iterar con GitHub Copilot y proporcionar contexto y solicitudes de cambio. Es el flujo normal cuando se trabaja con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
