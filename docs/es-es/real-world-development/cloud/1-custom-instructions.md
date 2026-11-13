---
title: "Ejercicio 1 - Instrucciones personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Copilot cloud agent →][next-lesson]

Es probable que muy pocas organizaciones —si es que hay alguna— no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidad que llevan tiempo en la lista de tareas pendientes porque no ha habido tiempo para implementarlas. Copilot cloud agent está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una PR preliminar para que la revise un desarrollador. Así, puede encargarse de tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en cuestiones más importantes.

En este ejercicio explorarás cómo Copilot cloud agent:

- personaliza el entorno para generar código.
- garantiza que las operaciones se realicen de forma segura.
- trabaja con incidencias cuyo alcance está bien definido.
- recibe incidencias asignadas.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no compararás prompts antes y después. Copilot cloud agent trabaja de forma asíncrona con incidencias de GitHub, por lo que su efecto es más difícil de mostrar en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las PR que genere cloud agent en este entorno.

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

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas generales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivos concretos. Permiten definir pautas para determinados lenguajes, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para las sugerencias de edición siguiente.
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

## Añade un estándar para todo el repositorio

Solo editarás un elemento en este ejercicio: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. Cloud agent la aplicará a las incidencias que le asignes más adelante.

Antes de editar, crea una rama. Cloud agent leerá las instrucciones de la rama a la que se dirija la incidencia, pero aun así confirmarás los cambios por separado:

1. En el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Documentemos los estándares de codificación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de codificación, pero aún no incluye pautas sobre la documentación en el código, por lo que el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown debajo de los estándares existentes para pedir a Copilot que incluya encabezados de comentario de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden crearse en el nivel del repositorio (`.github/copilot-instructions.md`) para definir pautas globales o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación.

## Confirma, envía y fusiona la actualización de instrucciones

Cloud agent lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia a Copilot en el próximo ejercicio, creará una rama a partir de `main`. Por eso, los cambios en las instrucciones deben llegar a `main` para que cloud agent pueda utilizarlos.

1. Prepara los cambios y crea un commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios de `custom-instructions-cloud` a `main` y fusiónala. Cloud agent leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama permite practicar un flujo de trabajo habitual en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar a las PR generadas por cloud agent en el resto de este entorno.

A continuación, [asignarás tu primera incidencia a Copilot cloud agent][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
