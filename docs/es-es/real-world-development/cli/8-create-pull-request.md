---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la skill, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos conjuntamente y abrir una solicitud de cambios. Fusionaste tú mismo la solicitud de cambios (PR) de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la skill y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de la revisión y confirmarás que la PR se ha fusionado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR —corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y actualiza la rama mediante un rebase cuando es necesario— y, después, habilita la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y asegúrate de que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la PR

Ya has creado todo el código. Revisémoslo conjuntamente, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la fusionará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar las solicitudes de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has habilitado Agent Merge solo después de la revisión y confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a cloud agent.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
