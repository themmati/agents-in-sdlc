---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual dirigido por agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la fusionarás correctamente con el proyecto.

> [!NOTE]
> Continuarás con la misma conversación y rama durante todo el flujo de trabajo. Normalmente utilizarías ramas o solicitudes de cambios distintas para diferentes tipos de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot desde una incidencia de GitHub.
- definirás los requisitos en modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente QA y abrirás la PR de la funcionalidad.

## Situación

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los principios fundamentales; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un flujo similar al siguiente:

1. Partir de una incidencia registrada que describa el trabajo pendiente.
2. Crear un plan de lo que debe desarrollarse.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, fusionar el cambio.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos son variantes del flujo anterior.

Al mantener este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas junto con el agente. Sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Partir de la incidencia

Antes de empezar con el filtrado, vuelve a tu Codespace y prepara el repositorio y el terminal.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de valoración por estrellas se ha fusionado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

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
8. Usa las flechas para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la indicación y volver a la pestaña **Session**.

Observa que la indicación comienza con `#7` (o un número similar). El carácter `#` permite añadir al contexto una incidencia o solicitud de cambios de GitHub.

## Planificar el filtrado

La planificación te permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de asignársela a Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificar. Cambia al modo Plan y pide a Copilot que elabore el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo de la indicación muestra **Plan**.
2. Después de la referencia a la incidencia añadida en el paso anterior, escribe:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Puede que Copilot haga preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias; no hay una única respuesta correcta.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Aprobar Autopilot

Una vez redactado y revisado el plan, toca implementarlo. Deja que Copilot trabaje en la tarea con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un nombre similar en tu versión.
2. Confirma que el indicador debajo de la indicación muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisar y verificar la implementación

Una vez generado el código, hay que revisarlo antes de fusionarlo, igual que cualquier otro código. Revisemos los cambios y ejecutemos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar en el navegador la funcionalidad de filtrado. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editor, así como las combinaciones acordadas en el plan.
4. Confirma que el reinicio y el comportamiento sin resultados coinciden con la incidencia y con tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva de Copilot desde una incidencia de GitHub.
- has definido los requisitos en modo Plan.
- has implementado la funcionalidad con Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
