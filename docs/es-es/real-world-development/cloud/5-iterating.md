---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Has pedido a Copilot que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad, incluidos controles para alternar entre los modos de alto contraste y claro. Revisemos los cambios de código y, si es necesario, demos comentarios a Copilot para mejorar su trabajo.

### Escenario

Como hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre queremos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con eso en mente, revisemos las sugerencias de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent realiza sus tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que el entorno siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- Cloud agent se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por explorar la primera solicitud de cambios (PR) generada por GitHub Copilot cloud agent: la que añade documentación al código. Para ello, utilizaremos la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que veas un aviso de que un firewall ha bloqueado GitHub Copilot. Es **lo esperado**, porque Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall de Copilot cloud agent][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes tomarte un descanso o reflexionar sobre lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían completarse correctamente para la aplicación Astro. El proceso puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de cambios no es una comunicación unidireccional. También puedes mencionar a Copilot en comentarios de la PR —como harías con cualquier otra persona del equipo— o en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto prescriptivo que debas utilizar. Puedes pedir a Copilot, por ejemplo, que:

- Añada encabezados de comentario al principio de cada archivo de código con una breve descripción de su función.
- Añada comentarios TSDoc a archivos TypeScript y Astro.
- Cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes utilizar alguna de las ideas anteriores u otra sugerencia de documentación que quieras incluir en la base de código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Fíjate en que inicia una sesión nueva para realizar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, aparecerá un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta quedar conforme. Cuando estés satisfecho, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marca la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la PR que Copilot ha generado para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían completarse correctamente para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu Codespace y probar manualmente la función de juegos relacionados. Ve a tu Codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con Copilot cloud agent para solicitar actualizaciones. Por ejemplo, puedes pedirle que ajuste el número de juegos relacionados o que añada encabezados de comentario y comentarios TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas). Como antes, puedes hacer estas solicitudes con un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste que asignaste en el Ejercicio 3 como el modo claro que solicitaste desde mission control en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía trabaja en la tarea, la PR incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - Los componentes de interfaz de usuario para alternar entre modos.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios de CSS o estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.).
   - El código JavaScript/TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían completarse correctamente para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve a tu Codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles para alternar entre los modos de alto contraste y claro en el navegador. Confirma que funcionan como esperas y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora, puedes mencionar a **@copilot** en un comentario y pedirle que haga cambios, como en las PR anteriores.

## Ejercicio opcional: sigue delegando

Cloud agent funciona mejor cuando puedes asignarle elementos reales del trabajo pendiente y centrarte en otra cosa. Para adquirir esa costumbre, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para que quienes apoyan los juegos expresen su interés en la página de detalles.
- Implementar la paginación en la página con la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno de cloud agent. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para comprobar su efecto en el resultado de cloud agent.
- **Has asignado incidencias a Copilot cloud agent** y observado cómo configura el entorno, planifica y ejecuta tareas de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir controles de modo de alto contraste y modo claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado las solicitudes de cambios de cloud agent y has iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el entorno de cloud agent. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, skills de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y el puente opcional `/delegate` a cloud agent.

En tu propio repositorio, prueba estas ideas:

- Asigna a cloud agent una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el firewall de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
