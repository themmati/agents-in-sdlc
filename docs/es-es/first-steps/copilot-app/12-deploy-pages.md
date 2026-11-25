---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de cambios y verificar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Da al Space Quiz un hogar público al publicarlo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte en la publicación, pero eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de cambios antes de fusionarla.
- seguirás el proceso de publicación y verificarás el sitio publicado.

## Situación

El Space Quiz ya está en un repositorio de GitHub, pero la gente todavía necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia de entorno de ejecución.

## Pide a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de cambios puede confirmar y enviar código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar este prompt. Si está seleccionado **Approve all**, no esperes otro aviso de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepara la publicación de este repositorio en GitHub Pages. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages cuando los cambios lleguen a la rama predeterminada. Si aún no existe, añade un README que describa la aplicación y cómo ejecutarla localmente. Crea una solicitud de cambios con estas modificaciones e indica su enlace y cualquier configuración o permiso necesario de Pages. No fusiones la solicitud, no habilites la fusión automática, no cambies la configuración de Pages ni publiques el sitio. Detente y espera a que lo revise.
```

## Revisa el flujo de trabajo y la solicitud de cambios

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y publicación del flujo de trabajo, sus activadores y permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de cambios que aún no se haya fusionado.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la fusión.

## Autoriza la fusión y la publicación

> [!WARNING]
> El prompt siguiente autoriza al agente a fusionar la solicitud de cambios revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de completar la revisión y confirmar el repositorio y la solicitud de cambios. La configuración de aprobación puede permitir estas acciones sin otro aviso de confirmación.

Sustituye `<PR_NUMBER>` por el número de la solicitud de cambios que has revisado y envía este prompt por separado:

```plaintext
He revisado la solicitud de cambios <PR_NUMBER> de este repositorio y autorizo su fusión y publicación. Habilita GitHub Pages mediante GitHub Actions, fusiona esa solicitud solo cuando se superen las comprobaciones obligatorias y supervisa la publicación hasta que se complete correctamente. Indica la URL final del sitio publicado. Si no tienes permiso para habilitar Pages o fusionar la solicitud, dime exactamente qué debo aprobar o hacer; no indiques que ha terminado correctamente hasta que el sitio esté disponible.
```

![Diagrama de tres fases: una solicitud de cambios de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifica la publicación

1. Sigue la ejecución de publicación de GitHub Actions hasta que se complete correctamente.
2. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages ni fusionar la solicitud de cambios, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la publicación se complete y el sitio esté disponible.

## Resumen y siguientes pasos

Tu Space Quiz ya está disponible como sitio publicado y has verificado la publicación en lugar de depender del informe del agente. Continúa con la [Lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
