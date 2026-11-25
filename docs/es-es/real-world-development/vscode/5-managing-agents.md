---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas Copilot en modo de agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar cada diff, aceptar o rechazar cambios concretos y orientar la conversación con nuevas indicaciones sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- orientarás al agente durante la sesión para que añada un control para el modo claro junto al de alto contraste.

## Situación

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, pensado para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de empezar una conversación nueva, ampliarás el trabajo desde la sesión actual.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada modificación aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas, lectura de archivos, edición y comando de terminal que solicita ejecutar.
3. Cuando proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos con los que estés de acuerdo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Orientar al agente para añadir el modo claro

Ahora que ya está implementado el modo de alto contraste, amplía la misma conversación con un control para el modo claro. Como continúas en la misma sesión de chat, el agente mantiene el contexto de su trabajo: los componentes que modificó, el enfoque de estilo y el uso del almacenamiento local.

1. Envía esta indicación en Copilot Chat:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control para el modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el alto contraste de forma independiente.
    ```

2. El agente continuará desde el trabajo anterior, revisará los cambios y propondrá actualizaciones para añadir el modo claro.
3. Revisa cada modificación a medida que aparezca. Utiliza **Keep** o **Undo** para decidir si la aceptas.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, refactoriza el control de una forma que no te gusta—, detenlo con **Stop**, junto al campo de chat, y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Puedes pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de ajustes existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisar los archivos modificados

Antes de confirmar el trabajo, revisa brevemente todos los cambios del agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa sus diffs. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones concretas.
- orientar una sesión con indicaciones de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de cambios y revisarás los cambios del agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
