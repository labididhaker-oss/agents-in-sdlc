---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta —o hay información de fondo que Copilot debería conocer—, conviene facilitarle ese contexto. Hay varias formas de compartir información específica con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de codificación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos para determinadas rutas.
- generarás la primera parte de los datos para el filtrado (un helper de editoriales) con las instrucciones actuales.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás en `main` la actualización de instrucciones y la parte del filtrado para que Copilot pueda utilizar las nuevas pautas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del cambio después de actualizar las instrucciones, no obtener un resultado idéntico carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información adecuada para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y requisitos de codificación. Permiten orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de codificación y bibliotecas preferidas, e incluso los tipos de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes: la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas generales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivos concretos. Permiten definir pautas para determinados lenguajes, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent utilizan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Explicar a fondo cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, el proyecto de ejemplo muestra un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en la orientación general del proyecto, como la descripción de lo que se está desarrollando, la estructura y los estándares globales de codificación.
- Utiliza archivos `*.instructions.md` para instrucciones específicas de tipos de archivos —pruebas unitarias, componentes Astro o la capa de datos— o de tareas concretas.
- Escribe en lenguaje natural. Expresa las pautas con claridad e incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única forma correcta de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que observes que hay instrucciones para distintas tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente al entorno te lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las pautas anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Observa que hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón global relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en otros más pequeños y reutilizables, y señalar a Copilot ejemplos que seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de codificación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso la indicará cuando sea pertinente.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas del editor de ejercicios anteriores para que Copilot reciba únicamente el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. Selecciona **Agent** en la lista de agentes de la vista Chat para que Copilot pueda aplicar cambios en los archivos.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios de código; a menudo modificará tanto el archivo del helper como sus pruebas.
6. Observa que el helper propuesto es una función con tipos que recibe un cliente `db` como primer argumento y devuelve una matriz de editoriales con tipos. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, podría añadir comentarios de documentación aunque no se lo hayas pedido. No pasa nada; el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Documentemos los estándares de codificación del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de codificación, pero aún no incluye pautas sobre documentación en el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown debajo de los estándares existentes para pedir a Copilot que incluya encabezados de comentario de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden crearse en el nivel del repositorio (`.github/copilot-instructions.md`) para definir pautas globales o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a los datos de editoriales de la plataforma Tailspin Toys Crowd Funding.
    * Proporciona funciones para recuperar información de las editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con sus identificadores y nombres.
    *
    * @param db - El cliente de base de datos Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique al código real sobre el que se basará el siguiente ejercicio.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones se gestionan con el mismo control de versiones que cualquier otro recurso. Confirma los cambios y envía la rama al repositorio.

1. Abre otra ventana de terminal pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, confirma que están presentes las actualizaciones de instrucciones y del helper mediante:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones de soporte adicionales (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una solicitud de cambios

Ya has enviado la rama; ahora crea una PR y vincúlala con la incidencia sobre los estándares de documentación de tu lista de tareas. Podrías hacerlo manualmente, pero Copilot puede utilizar las herramientas de GitHub conectadas al proyecto. Pídele que encuentre la incidencia, cree una PR que la cierre y la fusione.

> [!NOTE]
> En este ejercicio, Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub al espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot utilice una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior verás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR a partir de la rama actual, indicando tanto las actualizaciones de instrucciones como la base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot te pida **Allow** para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
5. Cuando se cree la PR, pídele a Copilot que la fusione en `main` y que vuelva a esa rama:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Cuando Copilot te pida **Allow** para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** según corresponda.

¡Has creado y fusionado una solicitud de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar lo que produce con las instrucciones *existentes*.
- añadir un estándar general a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto incorpora el nuevo estándar.
- confirmar la actualización de instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una nueva función a la base de código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
