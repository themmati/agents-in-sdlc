---
title: "Lección 7 - Crear y utilizar un agente de QA"
description: "Crea un agente de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de QA claramente definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y examinarás un perfil de control de calidad reutilizable.
- seleccionarás el agente QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Situación

Antes de abrir una solicitud de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de sus conclusiones. No sustituye a esas personalizaciones ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones que realiza QA y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades respecto a sus incidencias y requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, utilizar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe informar de cada requisito como superado, fallido o bloqueado y aportar pruebas que respalden el resultado. Debe preguntar antes de cambiar el código de implementación y no debe confirmar cambios ni abrir solicitudes de cambios.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Examinar el perfil

Antes de utilizar el agente, revisa su perfil para confirmar que Copilot ha incorporado el flujo y los límites previstos. Así evitarás que un agente incompleto o demasiado amplio modifique la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo da al agente un nombre visible claro.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que QA aporta pruebas, pregunta antes de cambiar el código de implementación y no confirma cambios ni abre solicitudes de cambios.
5. Si al perfil generado le falta alguna de estas responsabilidades o límites, pide al agente predeterminado que lo revise antes de continuar.

## Ejecutar QA frente a la incidencia

Copilot CLI carga los agentes del proyecto cuando se inicia una conversación. Reanuda la conversación de filtrado después de crear el perfil y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación que ya forman parte del contexto.

1. Activa el agente con esta indicación:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Puede que el agente no aparezca en la lista justo después de crearlo. Aun así, el comando anterior lo activará.

2. Pide al agente QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado respecto a la incidencia y las decisiones del plan. ¿Está lista para una PR?
   ```

3. El agente QA empezará a trabajar.
4. Cuando termine, lee el informe que proporciona.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado sus resultados. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- has creado y examinado un perfil de control de calidad reutilizable.
- has seleccionado el agente QA y revisado sus conclusiones respecto a la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil QA, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
