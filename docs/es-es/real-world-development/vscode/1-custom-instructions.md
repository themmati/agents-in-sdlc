---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una manera concreta, o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] permiten proporcionar directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos limitados por ruta.
- generarás la primera parte de la capa de datos para el filtrado: un helper de editores, con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás la misma indicación y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás los cambios de instrucciones y del filtrado en `main` para que Copilot pueda utilizar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot no es determinista. El objetivo es observar la tendencia tras actualizar las instrucciones, no obtener siempre el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Situación

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información adecuada para trabajar conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot comprenda mejor tu estilo y requisitos de programación. Puedes especificar convenciones, bibliotecas y tipos de comentarios. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot con **cada** solicitud del repositorio. Debe contener información pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción general del proyecto y las prácticas recomendadas.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretos, como TypeScript, Astro, componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite otros estándares de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Crear archivos de instrucciones está fuera del alcance del taller, pero el proyecto de ejemplo muestra un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices generales, como la descripción y estructura del proyecto y los estándares globales.
- Usa archivos `*.instructions.md` para tipos de archivo o tareas concretos, como pruebas unitarias, componentes Astro o la capa de datos.
- Escribe en lenguaje natural, con directrices claras y ejemplos de código correctos e incorrectos.

No existe una única forma de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar lo que mejor funcione para el proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para diferentes tareas, como [actualizaciones de interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente lo indicará cuando sea pertinente.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones del proyecto

Lee los archivos de instrucciones que incluye el repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**, que se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón glob relativo a la raíz del repositorio que determina qué archivos coinciden. Por ejemplo, coincide con los archivos de prueba TypeScript que cumplan `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Crear una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve a tu codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuándo son importantes.

Para observar el efecto de las instrucciones, primero envía una indicación con el archivo actual. Después actualizarás las instrucciones y enviarás la misma indicación para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot tenga solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse el helper.
3. Selecciona **Agent** en el selector de agentes de Chat para que Copilot pueda modificar archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todos los editores, con su nombre e id. Aplica los cambios al archivo.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente al helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, aplicables a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario para el archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añadir un estándar para todo el repositorio

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añadamos un estándar para que las sugerencias de código sean más completas.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Documenta los estándares de programación del proyecto, pero aún no contiene directrices para la documentación en el código.
3. Añade estas líneas Markdown justo después de los estándares existentes:

   ```markdown
   - Todas las funciones exportadas deben tener un comentario TSDoc que describa su propósito, parámetros y valor de retorno.
   - Antes de las importaciones o de cualquier código, añade al archivo un bloque de comentarios que explique su propósito.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Repetir la indicación y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía la **misma indicación** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todos los editores, con su nombre e id. Aplica los cambios al archivo.
   ```

4. Observa que el archivo comienza ahora con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a los datos de editores de la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editores de la base de datos.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su id y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones, como cualquier otro recurso del repositorio, se gestionan mediante el control de versiones. Confirma los cambios y envía la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que se han actualizado las instrucciones y el helper:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una solicitud de cambios

Ya has enviado la rama. Ahora abre una solicitud de cambios y relaciónala con la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear la PR y fusionarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub, así que no hay que instalar nada. La primera vez que Copilot utilice una herramienta, VS Code podría pedirte que inicies sesión. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto las instrucciones como la base del helper de editores:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de cambios desde la rama actual e indica que cierra esa incidencia. Incluye también que hemos añadido la base del helper de editores para el trabajo de filtrado del siguiente ejercicio.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se haya creado la PR, pídele que la fusione en `main` y que actualice la rama local:

   ```plaintext
   Fusiona la PR en main. Después, vuelve localmente a main y descarga los cambios más recientes para tener el repositorio actualizado.
   ```

7. Cuando Copilot solicite permiso para realizar acciones en GitHub y ejecutar comandos de shell, revisa los comandos y selecciona **Allow** si corresponde.

¡Has creado y fusionado una solicitud de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir la misma indicación y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar los cambios de instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, aplicarás las instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
