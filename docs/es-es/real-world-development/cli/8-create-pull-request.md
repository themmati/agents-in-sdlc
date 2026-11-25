---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es el momento de revisarlos juntos y abrir una solicitud de cambios. Tú mismo fusionaste la solicitud de cambios (PR) de valoración por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- revisarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR del filtrado.
- activarás Agent Merge solo después de revisarla y confirmarás que se ha fusionado.

## Situación

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización de fusión bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la base cuando hace falta. Después habilita la fusión automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora seleccionabas personalmente **Merge pull request**. Agent Merge puede hacerse cargo de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Con el código ya creado, revisémoslo, creemos la PR y permitamos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de agentes con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR y, cuando se superen todas las comprobaciones, la fusionará.
7. Confirma que Copilot muestra un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos ausentes. Si se bloquea, revisa el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo, como la generación, las pruebas y la validación del código, y ahora también el proceso de solicitud de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- has revisado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge solo después de revisarla y has confirmado que se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
