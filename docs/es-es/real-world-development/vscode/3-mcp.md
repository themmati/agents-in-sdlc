---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de cambios, confirma que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** para que Copilot controle un navegador real y pruebe la funcionalidad. Después, publicarás la rama y abrirás una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplía las herramientas de Copilot.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) permite que Copilot lo haga: es un estándar para que el agente se comunique con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. El servidor gestiona la comunicación con herramientas como API existentes o utilidades locales, por ejemplo, paquetes de NPM. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear y actualizar repositorios, y gestionar incidencias y solicitudes de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador con Playwright para visitar páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a herramientas y recursos distintos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Utiliza solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones confidenciales.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías encontrar un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`, que permitió a Copilot leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalarlo localmente: VS Code se conecta mediante HTTP y te autenticas la primera vez que Copilot utiliza una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás otro servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable que podrás utilizar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin ventana visible, algo necesario en un codespace, que no dispone de escritorio para mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar el servidor y confiar en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere sus herramientas, VS Code los inicia y te pregunta si confías en ellos.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

Iniciarlo manualmente es opcional. Si omites estos pasos, VS Code lo iniciará y te pedirá que confirmes si confías en él cuando Copilot necesite sus herramientas.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. En lugar de probar la aplicación manualmente, el agente puede abrir una página, navegar por ella, aplicar filtros y comunicarte los resultados. Es una forma rápida de comprobar el comportamiento sin salir de la conversación.

El servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Por eso, el agente razona sobre elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, igual que hace una tecnología de asistencia. Una prueba funcional breve también permite comprobar algunos aspectos básicos de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Utilizando el servidor MCP de Playwright, abre un navegador en la aplicación que se está ejecutando en http://localhost:4321 y verifica la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Quita el filtro y aplica uno de editor. Confirma que la lista solo muestra juegos de ese editor.
4. Combina un filtro de categoría y otro de editor, y confirma que los resultados cumplen ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará sus resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide más información o que corrija el código antes de abrir la solicitud de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para esta prueba. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot utiliza Playwright MCP, puede que tenga que descargar un navegador; si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de cambios

Una vez confirmada la funcionalidad, publica la rama `filtering-vscode` para que el equipo pueda revisarla.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de cambios

Puedes crear una solicitud de cambios desde github.com o con GitHub CLI. Como ya estás trabajando con GitHub Copilot, pídele que la cree y la vincule a la incidencia correcta.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado para que Copilot pueda utilizar las herramientas de GitHub.
3. Pídele que cree la PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editor. Crea una solicitud de cambios para la rama actual y vincúlala a la incidencia correcta.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot obtenga información y realice las operaciones. La primera vez que utilice una herramienta de GitHub, puede que tengas que autenticarte.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace de Copilot para revisar la solicitud, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplía las herramientas de Copilot.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has utilizado para probar manualmente la funcionalidad de filtrado en un navegador.
- has publicado la rama y abierto una solicitud de cambios.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de él?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
