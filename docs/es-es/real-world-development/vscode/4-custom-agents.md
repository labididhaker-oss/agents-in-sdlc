---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de trabajo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de codificación y conocimientos del dominio para que realice mejor ciertos tipos de trabajo. Los equipos pueden codificar sus conocimientos en agentes reutilizables: un agente de accesibilidad que aplique el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con al menos los campos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las skills de agente

Los agentes personalizados y las [skills de agente][agent-skills-concept] tienen algunos aspectos en común. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que el **agente personalizado** es el trabajador y las **skills** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar skills —e incluso otros agentes— mientras realizan su trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; para ello, podría recurrir a skills como una skill para el flujo de revisión de pull requests o una que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier función de IA, prueba y repite hasta encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes han indicado que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que quieres implementar esta función cuanto antes. Para generarla, utilizarás un agente personalizado.

En este ejercicio aprenderás a:

- revisar un agente personalizado de accesibilidad existente.
- utilizar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y abre un terminal. Crea una rama nueva a partir de `main` para el trabajo de accesibilidad, así mantendrás separada la PR de filtrado del Ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que destacan:
   - Responsabilidades principales al generar código para un sitio web accesible.
   - Prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Errores y problemas habituales.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en la lista de agentes situada en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso la indicará cuando sea relevante.

1. Si no está seleccionado, elige **Agent** en la lista de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona la lista de agentes de la parte inferior de la vista de chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará la base de código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que podrás revisar.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot está haciendo cambios reales en el repositorio: editará archivos existentes, como componentes Astro, CSS y pruebas relacionadas.

Revisarás y orientarás este trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes especializados para determinadas tareas y áreas. Con ellos, puedes codificar los conocimientos y estándares del equipo en agentes reutilizables que orientan a Copilot para que realice mejor ciertos tipos de trabajo.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo utilizar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios mientras se realizan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
