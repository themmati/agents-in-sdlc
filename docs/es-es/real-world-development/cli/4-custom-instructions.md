---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga esas directrices disponibles. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones limitados por ruta.
- actualizarás el archivo de instrucciones para garantizar que se sigan los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Situación

Como cualquier buen equipo de desarrollo, Tailspin Toys dispone de directrices y requisitos para las prácticas de programación. Entre ellos se incluyen:

- Los comentarios deben explicar la intención y las decisiones no evidentes, en lugar de repetir lo que hace el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores de retorno mediante TSDoc/JSDoc, incluido un argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos de `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices existentes de formato y lint.

Mediante los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas conforme a estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que comprenda mejor tu estilo y requisitos de programación. Esta funcionalidad te ayuda a orientar a Copilot para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas e incluso los tipos de comentarios que prefieres incluir en el código. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos, con contexto específico para una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot con **cada** solicitud del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está creando y las prácticas recomendadas.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> Los demás formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad de instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye un conjunto de archivos de instrucciones. Antes de hacer cambios, explora lo que hay para observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que establece un patrón glob para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir.

## Actualizar las instrucciones según las directrices del equipo

Los archivos existentes son un buen punto de partida, pero aún falta algo. Modifica el archivo principal `copilot-instructions.md` para garantizar que el TypeScript generado incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, aproximadamente en la línea 35.
2. Añade este punto al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplicar las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio cuando se inicia una conversación. Después de guardar las instrucciones actualizadas, cierra y reanuda la conversación de filtrado para que Copilot las lea sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar nuestras instrucciones y directrices de código. ¿Puedes actualizar el código que has generado para que las cumpla?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones limitados por ruta.
- has actualizado el archivo de instrucciones para garantizar que se sigan los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que lint y las pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
