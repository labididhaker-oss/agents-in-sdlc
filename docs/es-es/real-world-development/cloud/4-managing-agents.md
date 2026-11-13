---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido a Copilot cloud agent que se encargara de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque cloud agent está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar tareas asignadas a cloud agent, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de cambios (PR) abiertas. También puedes explorar las operaciones realizadas y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas de cloud agent.
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de alternancia, parecía lógico incluir esta función. Orientarás el trabajo del agente para que añada tanto el modo claro como el de alto contraste.

## Revisa las tareas de Copilot cloud agent

Veamos el estado actual de las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - Actualizar la documentación de la base de código.
    - Generar API para modificar juegos.
    - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que Copilot ha realizado, entre ellas:
    - Obtener el código del repositorio.
    - Crear el entorno de trabajo de Copilot.
    - Configurar servidores MCP.
    - Realizar varias operaciones para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de PR que aparece a la derecha. Permite consultar la solicitud y los archivos modificados para seguir el trabajo.

## Orienta cloud agent

Ahora que has visto las tareas activas, pidamos a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya asignado y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión unos minutos, hasta que indique que ha terminado la configuración y ha empezado el trabajo. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea de cloud agent en la página de agentes, con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot reconoce el prompt y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso que estaba ejecutando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar para reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y orientar las tareas de GitHub Copilot cloud agent. Con este centro de control puedes seguir las tareas activas y completadas, revisar el trabajo que se está realizando y redirigir tareas en curso para ajustar su alcance o dar indicaciones adicionales.

Has explorado estos conceptos:

- utilizar mission control y la página de agentes para supervisar las tareas de cloud agent.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
