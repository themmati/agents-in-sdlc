---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente en la nube de Copilot que realizara tres tareas distintas para mejorar la experiencia y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde Mission Control puedes ver las tareas de los agentes que tienen solicitudes de cambios (PR) abiertas, revisar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Situación

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya estaba actualizando el estilo del sitio e incorporando controles para cambiar de modo, tenía sentido incluir esta función. Orientarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Veamos el estado actual de las tareas asignadas al agente en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de cambios de la derecha. Permite ver la PR y los archivos modificados para seguir el trabajo.

## Orientar al agente en la nube

Ya has visto las tareas activas. Pidamos a Copilot que añada un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot le haya asignado y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Supervisa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los del ejemplo.
3. En el cuadro **Steer active session while Copilot is working**, añade esta indicación:

    ```plaintext
    Mientras trabajamos en un modo de alto contraste, añadamos también un modo claro. Debe haber un control para que los usuarios puedan seleccionar el modo de visualización que prefieran.
    ```

    ![Captura de pantalla de una tarea del agente en la página de agentes con el cuadro Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot la confirma y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando al recibir el mensaje.

La tarea tardará varios minutos. Puedes aprovechar el tiempo para reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de supervisión y orientación de las tareas del agente en la nube de GitHub Copilot. Desde Mission Control puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una tarea en curso para cambiar su alcance o proporcionarle más directrices.

Has aprendido a:

- utilizar Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
