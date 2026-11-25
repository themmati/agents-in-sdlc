---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidad relegadas en el backlog por falta de tiempo. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de cambios (PR) en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en cuestiones de mayor alcance.

Con el agente en la nube de Copilot aprenderás a:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- crear incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: prácticamente no existe. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar, en la página de detalles de cada juego, otros títulos de la misma categoría para que los visitantes sigan explorando. No necesita una implementación pulida todavía; solo algo que permita hacer pruebas de aceptación de la experiencia. Este trabajo está bloqueado por otras incidencias de mayor prioridad.

Son tareas que pueden quedar relegadas fácilmente y que conviene asignar al agente en la nube. El agente trabaja en ellas de forma asíncrona; mientras tanto, el desarrollador puede ocuparse de otras tareas y volver después para revisar el resultado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma similar a un desarrollador. Una opción es [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después, Copilot crea una PR en borrador para mostrar el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras trabaja o después de que termine. Cuando esté lista para revisar la solución, Copilot te mencionará en la PR.

## Importancia de definir bien el alcance

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas. [Define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste el ejercicio práctico, conoces `.github/copilot-instructions.md`; si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente en la nube, consulta sin modificar los archivos de instrucciones del repositorio para que después puedas reconocer su efecto.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, respeta las convenciones de TypeScript y añade encabezados de comentario. Estas prácticas proceden de los archivos de instrucciones y volveremos a señalarlas al revisar las PR más adelante.

## Configurar el entorno del agente en la nube

La creación de código suele requerir un entorno específico y scripts de configuración para garantizar que todo esté listo. Lo mismo se aplica al agente en la nube, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de configuración][setup-workflow] que se ejecuta antes de que empiece a trabajar. Se define en `.github/workflows/copilot-setup-steps.yml` y permite instalar las herramientas y dependencias necesarias. Este taller ya incluye el flujo para que Copilot tenga Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña Actions del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos necesarios. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si estos pasos clonan el repositorio, por ejemplo, para instalar dependencias, necesitan contents: read. Si no lo clonan, Copilot lo hará automáticamente al finalizar los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación utiliza Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local con datos iniciales para la compilación y las pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de GitHub, pero hay dos detalles importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Muchos proyectos tienen documentación desactualizada o carecen de ella. Esta deuda técnica reduce la productividad, dificulta el mantenimiento y complica la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que esta es una buena incidencia para asignarle. Trabajará en segundo plano y revisarás el resultado en un ejercicio posterior.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Nuestra organización requiere que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Sin embargo, las actualizaciones recientes no han seguido este estándar. Hay que actualizar el código existente para añadir comentarios de documentación donde ayuden a aclarar el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será similar a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno, ejecutando el flujo de trabajo descrito antes, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR y comprueba que Copilot haya indicado que ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Recorre la sesión en directo para observar cómo Copilot explora el código, analiza su estado, se detiene a decidir el enfoque y crea código.

La tarea puede tardar varios minutos. Una ventaja del agente en la nube es que puede trabajar de forma asíncrona. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, escribiremos código para añadir funcionalidades a la aplicación.

## Añadir juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que permite repartir el trabajo. Mientras el equipo de diseño espera una sección de juegos relacionados, puedes ocuparte de otra tarea. Asignemos esta tarea al agente en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia y realizar pruebas de aceptación. Requisitos:

    - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego, excluyendo el propio juego.
    - Mostrar en la página de detalles una sección "Related games" que utilice el helper.
    - Gestionar el caso en que un juego no tenga otros juegos relacionados.
    - Crear pruebas unitarias para el helper nuevo.
    - Asegurarse de que todas las pruebas se superen antes de crear la PR.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma similar a un ingeniero de software: no necesitas supervisarlo continuamente, sino revisar su resultado cuando termine. Ahora puedes crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y pedirle que resuelva deuda técnica, cree funcionalidades o ayude a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- crear incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube también admite servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya exploramos en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
