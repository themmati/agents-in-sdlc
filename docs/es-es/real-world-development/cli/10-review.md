---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo continuo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial.
- añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera solicitud de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado la funcionalidad con Autopilot y revisado el resultado en modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado el navegador para explorar el filtrado.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisado todo el cambio de filtrado y autorizado Agent Merge para la PR.
- explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Qué has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones por estrellas:** mostrar el `starRating` existente y un estado explícito sin valoración en las tarjetas de juegos.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, utilizaste la misma conversación y rama. Reunimos ese trabajo en una sola PR para agilizar el taller.

## Distintos tipos de verificación

Comprobaste el código de varias formas: pruebas automatizadas, tu propia inspección en el navegador y la exploración de Copilot mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos, y presentó los resultados en el nuevo formato. QA reunió esos resultados con una revisión de los requisitos y la cobertura de pruebas antes de la PR.

Las pruebas añadidas deben cubrir carencias reales; una ejecución de QA que no necesite pruebas nuevas puede ser correcta. Revisa el código y las pruebas de verificación antes de autorizar la fusión, y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a medida que cambien las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para definir un enfoque antes de programar, **Interactive** para mantener el control durante cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para las modificaciones rutinarias y otro más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia sustancialmente el resultado.

## Más opciones para explorar

Ya conoces el flujo de trabajo principal. Estas son algunas funciones adicionales de CLI que puedes explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para razonar sobre un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala para código de producción, para proyectos personales o para esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
