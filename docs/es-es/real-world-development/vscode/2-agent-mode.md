---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos suelen implicar varios archivos y tareas como ejecutar pruebas. Normalmente hay que localizar los archivos necesarios, modificarlos, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto y volver a cambiarlo. La lista continúa.

Para eso está el modo de agente de Copilot.

El modo de agente puede trabajar de forma autónoma en el IDE. Se comporta como un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de comentarios rápidos e interactivos. El agente en la nube trabaja de forma asíncrona, como un compañero del equipo, y se comunica mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, identificar archivos y coordinar cambios.
- implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de fusionarlos.

## Situación

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior añadiste un helper de editores. Ahora completarás el trabajo de la capa de datos, la interfaz de usuario y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio para entender su funcionamiento actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Se trata de una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en el frontmatter; no hay una API de backend independiente ni un framework de interfaz de usuario del lado del cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con API DOM estándar.

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Puedes ejecutarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar un enlace `localhost` en el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Con el sitio en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuándo son importantes.

La implementación inicial funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto desde la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que la muestre.

1. Selecciona **Agent** en el selector de agentes de Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Durante su elaboración se utilizó Claude Sonnet 4.5, por lo que aparece en estas instrucciones para intentar que las sugerencias sean coherentes. Los LLM son probabilísticos y es normal que las sugerencias varíen.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía esta indicación para consultar el backlog:

   ```plaintext
   Muéstrame los elementos pendientes de mi repositorio de GitHub. Ayúdame a priorizarlos según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según su posible utilidad para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que dará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices sobre cómo añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, facilidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo que contiene el enlace. Esto permite reutilizar y dividir instrucciones complejas en partes más manejables, además de proporcionar ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica de filtrado de la capa de datos en `src/lib/`.
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos con la interfaz de filtrado.

Además, hay que ejecutar y superar las pruebas antes de fusionar los cambios. El modo de agente puede realizar estas tareas.

1. Continúa en la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes de Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para añadir filtros por editor y categoría según los requisitos de la incidencia correspondiente de GitHub. Ya existe un helper de editores del ejercicio anterior; consérvalo y mejóralo si es necesario mientras completas el resto de la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución; no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la capa de datos, la interfaz de usuario y las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos, según la complejidad del código. Mientras trabaja, pueden aparecer **Keep** y **Undo** en el editor. Al terminar habrá una opción para cada cambio; no hace falta seleccionarlas mientras siga trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo en Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot se detenga y que algunas pruebas fallen durante el proceso. Es normal: alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot y otras herramientas de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el modo de agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz de usuario y la capa de datos con el modo de agente.
- explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados antes de fusionarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación y si el usuario podrá elegir el tamaño de página.
- proporciona a Copilot las directrices necesarias para crear la implementación deseada.
- puede que tengas que iterar y pedir cambios o proporcionar más contexto. Así suele trabajarse con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
