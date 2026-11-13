---
title: "Ejercicio 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones —si es que hay alguna— no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidad que llevan tiempo en la lista de tareas pendientes porque no ha habido tiempo para implementarlas. Copilot cloud agent está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una PR preliminar para que la revise un desarrollador. Así, puede encargarse de tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en cuestiones más importantes.

En este ejercicio explorarás cómo Copilot cloud agent:

- personaliza el entorno para generar código.
- garantiza que las operaciones se realicen de forma segura.
- trabaja con incidencias cuyo alcance está bien definido.
- recibe incidencias asignadas.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: en realidad, no hay documentación. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que la gente pueda seguir explorando. De momento no necesita una implementación refinada; solo quiere algo que pueda utilizar para probar la experiencia. Este trabajo está bloqueado, pero ahora mismo hay otras incidencias más prioritarias.

Estos son ejemplos de tareas que pueden perder prioridad con facilidad y que conviene asignar a Copilot cloud agent. Cloud agent puede trabajar en ellas de forma asíncrona y permitirte centrarte en otras tareas antes de volver para revisar su trabajo y comprobar que todo está como esperabas.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. También puedes iniciar este trabajo de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una tarea, Copilot crea una solicitud de cambios preliminar para registrar el progreso, configura un entorno y empieza a trabajar. Puedes explorar la sesión de Copilot mientras se ejecuta o después de que termine. Cuando esté lista la solución propuesta, te mencionará en la PR.

## La importancia de unas instrucciones con un alcance claro

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las operaciones que parecen sencillas pueden ser complejas cuando se examinan de cerca, así que [define cuidadosamente el alcance de las tareas que asignas a Copilot cloud agent][cloud-agent-best-practices]. Trátalo como a un programador colaborativo de IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar la IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo a Copilot cloud agent, inspecciona en modo de solo lectura los archivos de instrucciones que ya incluye este repositorio. Así podrás observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub del repositorio o en un Codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas relativas a las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes a cloud agent la incidencia `Code lacks documentation` en la sección siguiente, busca en la PR comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas pautas provienen de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo de Copilot cloud agent

La creación de código suele requerir un entorno específico y algunos scripts de configuración para garantizar que todo esté listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

Cloud agent utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. El archivo ya está configurado para facilitar el taller y permitirte explorar esta función. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Todo el mundo entiende la importancia de la documentación, pero la mayoría de los proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta mantener el código o incorporar nuevos desarrolladores al equipo. Copilot destaca a la hora de crear documentación, así que es una incidencia ideal para asignar a Copilot cloud agent. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio futuro volverás a revisar su trabajo.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crearla.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios (PR) que se acaba de generar; el título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece ninguna PR, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está configurando su entorno —ejecutando el flujo de trabajo anterior—, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan adecuado, y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales de Copilot cloud agent es realizar tareas asíncronas y permitirnos centrarnos en otras. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, nos centraremos en desarrollar una funcionalidad para la aplicación.

## Añade una sección de juegos relacionados a los detalles de cada juego

Como ya hemos indicado, una de las grandes ventajas de GitHub Copilot cloud agent es que permite repartir el trabajo para que tú puedas centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. ¡Asignémosela a Copilot cloud agent!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás varios 👀 en el primer comentario de la incidencia. Significa que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Copilot cloud agent funciona como un ingeniero de software: no necesitas supervisarlo activamente; basta con revisar su trabajo al terminar. Ahora veremos cómo crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignar incidencias a cloud agent para que las resuelva de forma asíncrona. Copilot puede encargarse de deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno para generar código.
- garantizar que las operaciones se realicen de forma segura.
- definir claramente el alcance de las incidencias.
- asignar incidencias a Copilot.

Mientras Copilot cloud agent trabaja en segundo plano, puedes crear y utilizar agentes personalizados. [Copilot cloud agent también puede utilizar servidores MCP][cloud-agent-mcp] y puede acceder a instrucciones personalizadas, que hemos explorado en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
