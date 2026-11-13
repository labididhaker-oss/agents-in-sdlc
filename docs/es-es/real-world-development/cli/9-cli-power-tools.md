---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente a cloud agent."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos otros que pueden ser útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- utilizarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas a cloud agent.

## Escenario

Ya has terminado el flujo de trabajo principal de la CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente a [Copilot cloud agent][about-cloud-agent].

## Explora el contexto de Copilot CLI

En las tareas extensas o complejas puedes alcanzar el tamaño máximo de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y puedes inspeccionar o compactar el contexto tú mismo mediante comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Puede que no haya cambiado mucho si la conversación ya era breve.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` si quieres decidir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea no relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Cada modelo tiene sus puntos fuertes y cada desarrollador sus preferencias. Copilot CLI permite mostrar y seleccionar el modelo que quieres utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar en equipo y compartir lo aprendido ayuda a todo el mundo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir esta sesión, elige el destino adecuado, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

La publicación de un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

Has terminado el taller principal. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una solicitud de cambios.

Estos comandos son opcionales porque pueden crear otros árboles de trabajo o iniciar tareas remotas. Empieza por una tarea nueva y bien definida, y revisa el resultado con tu flujo habitual. Si quieres profundizar en el trabajo asíncrono de los agentes, continúa con el [taller de cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y ver lo que ocurre internamente. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has utilizado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas a cloud agent.

¡Copilot CLI ofrece muchos más comandos de barra! Cerraremos el recorrido [repasando lo aprendido][next-lesson] y algunos pasos para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
