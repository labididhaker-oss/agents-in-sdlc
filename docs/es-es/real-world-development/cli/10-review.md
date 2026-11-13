---
title: "Lección 10 - Recapitulación y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial.
- añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera solicitud de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- guiado al agente con instrucciones personalizadas, adaptado la skill existente `quality-checks` y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado el navegador real para explorar el filtrado.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la skill y las pruebas en el navegador.
- revisado el cambio completo de filtrado y autorizado Agent Merge para la PR correspondiente.
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a cloud agent.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones por estrellas:** mostrar el valor existente de `starRating` y un estado explícito sin valoración en las tarjetas de juegos.
2. **Flujo de filtrado y calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, adaptar el informe `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos combinado ese trabajo en una PR para agilizar el taller.

## Distintas formas de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La skill `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha incluido los resultados en el nuevo formato. Antes de la PR, QA reunió esos resultados con una revisión de los requisitos y la cobertura de pruebas.

Las pruebas añadidas deben cubrir carencias reales; puede ser correcto que una ejecución de QA no necesite pruebas nuevas. Revisa el código y las pruebas antes de autorizar la fusión, y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, adaptado una skill, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades de tu equipo. Las instrucciones establecen estándares, las skills describen tareas repetibles, los agentes personalizados definen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios y resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar el enfoque antes de empezar a construir, **Interactive** para mantenerte al tanto de los cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para las ediciones rutinarias y otro más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia sustancialmente el resultado.

## Más cosas por explorar

Ya has recorrido el flujo de trabajo principal. Estas son algunas funciones más de la CLI que merece la pena explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para analizar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
