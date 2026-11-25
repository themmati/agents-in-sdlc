---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una manera concreta, o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] permiten proporcionar esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones limitados por ruta.
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no ejecutarás una indicación antes y después del cambio. El agente en la nube de Copilot trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar en tiempo real el efecto de las instrucciones. Lo verás más adelante, cuando revises las solicitudes de cambios que cree el agente.

## Archivos de instrucciones

### Situación

Tailspin Toys tiene un conjunto de directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información adecuada para trabajar conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que comprenda mejor tu estilo y requisitos de programación. Puedes especificar las convenciones de programación, las bibliotecas e incluso los tipos de comentarios que prefieres. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

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

Crear archivos de instrucciones está fuera del alcance de este taller, pero el proyecto de ejemplo muestra un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para tipos de archivo o tareas concretos, como pruebas unitarias, componentes Astro o la capa de datos.
- Escribe en lenguaje natural, con directrices claras y ejemplos de código correctos e incorrectos.

No existe una única forma de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar lo que mejor funcione para el proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para distintos tipos de tareas, como [actualizaciones de interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente lo indicará cuando sea pertinente.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye el repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**, que se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón glob relativo a la raíz del repositorio que determina los archivos afectados. Por ejemplo, coincide con cualquier archivo de pruebas TypeScript que cumpla `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentario de los archivos.

## Añadir un estándar para todo el repositorio

El siguiente paso es la única edición que harás aquí: añadir un estándar general para que la documentación se incluya en el código mediante comentarios TSDoc y encabezados de comentario en cada archivo. El agente en la nube aplicará esta directriz cuando trabaje en incidencias que le asignes más adelante.

Antes de editar, crea una rama para trabajar. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero así confirmarás los cambios de forma ordenada:

1. Desde el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade el estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Documenta los estándares de programación del proyecto, pero aún no contiene directrices para la documentación en el código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade estas líneas Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo y tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, enviar y fusionar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot, que creará una rama desde `main`. Por eso, los cambios de instrucciones deben llegar a `main`.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios desde `custom-instructions-cloud` a `main` y fusiónala. Así el agente en la nube podrá leer las instrucciones al trabajar en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama se incluye para reflejar cómo se haría en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de cambios que cree el agente en la nube durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
