---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de cambios y verificar Space Quiz en directo."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Da a Space Quiz un hogar público y publícalo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de cambios antes de fusionarla.
- seguirás la publicación y verificarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero la gente necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia del entorno de ejecución.

## Pide a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de cambios puede confirmar y enviar código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar este prompt. Si está seleccionada la opción **Approve all**, no esperes otro aviso de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisa el flujo de trabajo y la solicitud de cambios

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y publicación del flujo de trabajo, sus desencadenadores y permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de cambios que aún no se haya fusionado.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la fusión.

## Autoriza la fusión y la publicación

> [!WARNING]
> El siguiente prompt autoriza al agente a fusionar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de completar la revisión y confirmar el repositorio y la solicitud. La configuración de aprobación puede permitir estas acciones sin otro aviso.

Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía este prompt por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una solicitud de cambios de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifica la publicación

1. Sigue la ejecución de publicación de GitHub Actions hasta que se complete correctamente.
2. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o fusionar la solicitud, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la publicación se complete y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio publicado y has verificado la publicación en lugar de confiar únicamente en el informe del agente. Continúa con [Lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
