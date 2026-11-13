---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas GitHub Copilot en modo agente, trabaja de forma autónoma: explora la base de código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que se producen. Puedes revisar cada diff integrado en el chat, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas en los archivos.
- orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha hecho una petición adicional: también quiere un control para el modo claro, pensado para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de iniciar otra conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en el chat:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos que ha modificado el agente durante la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará un aviso **Allow**. Aprueba solo los comandos con los que estés de acuerdo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Verás un mensaje final que resume los cambios realizados.

## Pide al agente que añada el modo claro

El modo de alto contraste ya está implementado. Ahora ampliaremos la misma conversación para añadir un control de modo claro. Como continuarás en el mismo chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde la entrada de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde el punto en que se quedó, explorará los cambios realizados y propondrá las adiciones para el modo claro.
3. Revisa cada nueva edición a medida que aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar los controles de alternancia de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto a la entrada del chat y enviar un prompt de seguimiento para aclararlo.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa sus diffs. Confirma que el agente personalizado ha aplicado patrones de accesibilidad, como atributos ARIA, navegación con el teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición al producirse, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos individuales.
- has orientado una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de cambios y revisarás los resultados del agente personalizado local que has utilizado en este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
