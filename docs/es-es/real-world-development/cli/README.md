---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Desarrolla, verifica y entrega dos cambios de Tailspin Toys mientras aprendes a usar los modos de Copilot CLI, las personalizaciones, las herramientas MCP y la automatización de solicitudes de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un flujo de trabajo continuo de Tailspin Toys:

1. Prepararás el proyecto en GitHub Codespaces, instalarás Copilot CLI y te familiarizarás con él.
2. Harás un cambio específico para añadir valoraciones por estrellas, lo revisarás en el navegador y fusionarás manualmente tu primera solicitud de cambios (PR).
3. Partirás de la incidencia de filtrado, definirás el enfoque en el modo Plan, lo implementarás en el modo Autopilot y lo revisarás en el modo Interactive.
4. Actualizarás las instrucciones del repositorio y las aplicarás al trabajo de filtrado.
5. Adaptarás la skill `quality-checks` existente y la utilizarás para ejecutar las comprobaciones del proyecto.
6. Añadirás el servidor Model Context Protocol (MCP) de Playwright y lo utilizarás para explorar el filtrado en un navegador.
7. Crearás un agente personalizado de control de calidad (QA) para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisarás el cambio completo de filtrado y utilizarás Agent Merge para la PR correspondiente.
9. Explorarás comandos de barra útiles para el contexto, los modelos, el uso compartido y la delegación opcional a cloud agent.

Para centrar el taller, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado junto con las actualizaciones de instrucciones, la actualización de la skill, el perfil de QA y las pruebas. El trabajo de filtrado y calidad comparte una conversación y una rama para que puedas aprovechar lo que has hecho mientras exploras cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instalar Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI, y familiarízate con la herramienta |
| [2. Añadir valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el estado alternativo cuando no hay ninguna; después, fusiona tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad a partir de su incidencia, créala con Autopilot y revísala con Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y utilizar una skill de comprobaciones de calidad][ex5] | Comprobaciones repetibles | Explora la skill existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en la CLI e inspecciona el comportamiento del filtrado |
| [7. Crear y utilizar un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas finales de verificación |
| [8. Crear y fusionar la PR de la funcionalidad][ex8] | Revisión y fusión | Revisa el cambio completo, crea la PR y utiliza Agent Merge |
| [9. Comandos de barra en GitHub Copilot CLI][ex9] | Funciones de CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional a cloud agent |
| [10. Recapitulación y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: incorporar Foundry][foundry] | Agentes hospedados | Prepara un modelo, implementa un agente basado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permisos para crear un repositorio y un Codespace
- [ ] Conocimientos básicos del terminal o de la línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller.

> [!NOTE]
> Si utilizas Copilot Business o Copilot Enterprise, comprueba que tu administrador haya habilitado Copilot CLI.

## Primeros pasos

**[Empieza con los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
