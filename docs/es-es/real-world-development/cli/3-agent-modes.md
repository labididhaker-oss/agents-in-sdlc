---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más grandes requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudarnos a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas, y finalmente la fusionarás en el proyecto.

> [!NOTE]
> Seguirás usando la misma conversación y rama durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían distintas ramas o PR para los diferentes tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador reenviado.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, adaptarás la skill `quality-checks` existente, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del trabajo pendiente describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en tu flujo de desarrollo no cambia los fundamentos. ¡Si acaso, los hace aún más importantes! La mayoría de los desarrolladores siguen un proceso similar a este:

1. Partir de una incidencia registrada que describe el trabajo necesario.
2. Crear un plan de lo que hay que desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Cuando se haya revisado el código y se haya completado correctamente la integración continua, fusionar el código.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen un patrón parecido al anterior.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de evaluación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: colaboras con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de valoración por estrellas está fusionada.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Fíjate en que el prompt ahora empieza por `#7` (o por un número parecido). El carácter `#` permite incluir en el contexto una incidencia o solicitud de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de encargársela a Copilot. Siempre es recomendable dedicar algo de tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empieza a preparar el plan. Primero explorará el proyecto y luego decidirá cuál es el mejor enfoque.

3. Durante el proceso, Copilot puede hacerte preguntas sobre cómo debe funcionar el filtrado. Respóndelas según tus preferencias. ¡No hay respuestas incorrectas!
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte del plan que no se ajuste a tus decisiones.

## Aprueba Autopilot

El plan ya está redactado y revisado. ¡Es hora de implementarlo! Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot repita el proceso hasta que considere que ha completado la tarea.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobarlo, la implementación puede empezar inmediatamente, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y verifica la implementación

Una vez generado el código, hay que revisarlo antes de fusionarlo, como cualquier otro código. Revisaremos el código y ejecutaremos el sitio para asegurarnos de que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, y las combinaciones acordadas en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado la funcionalidad manualmente en un navegador reenviado.

A continuación, veremos cómo se genera el código y cómo asegurarnos de que sigue las prácticas documentadas, [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
