---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, tienes que comprobar que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar el filtrado manualmente en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede acceder a herramientas externas. Model Context Protocol (MCP) es el protocolo que permite a Copilot hacerlo: un estándar de comunicación entre el agente y las herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y de cómo interactúa con el contexto, el LLM y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite a los agentes de IA comunicarse con herramientas y servicios externos en tiempo real, acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente y los servicios externos, como API existentes o herramientas locales (por ejemplo, paquetes npm). Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: ofrece acceso a API para gestionar repositorios de GitHub, crear y actualizar repositorios y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar un navegador con Playwright para navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa el código fuente, comprueba quién lo publica y evalúa las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Ya debería haber un servidor `github` configurado:

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

El servidor `github` forma parte de la plantilla del proyecto y permitió que Copilot leyera el backlog en el ejercicio anterior. Usa el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hay que instalarlo localmente. VS Code se conecta mediante HTTP y la primera vez que Copilot usa una herramienta solicita autenticación en GitHub.

## Añade el servidor Playwright MCP

Añadirás un segundo servidor. [Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. Añade una entrada `playwright` junto a `github` en `.vscode/mcp.json`:

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

A diferencia del servidor remoto `github`, `playwright` se ejecuta en local: VS Code inicia `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin mostrar una ventana, algo necesario en un codespace, que no tiene escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando hacen falta**: la primera vez que Copilot necesita una herramienta, VS Code inicia el servidor y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, para iniciar el servidor y comprobar que la configuración es correcta.
2. Si VS Code pregunta si confías en el servidor, revisa la configuración y confirma la confianza para iniciarlo.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y solicitará confianza la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

Con Playwright MCP, Copilot puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Así puede resumirte el comportamiento sin que tengas que probar la aplicación fuera de la conversación.

Internamente, el servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, del mismo modo que lo hace una tecnología de asistencia. Así, una comprobación funcional rápida sirve también como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador con Playwright MCP, seguirá los pasos y te resumirá lo que ha observado. Compara el informe con los criterios de aceptación de la incidencia. Si detectas un problema, pide aclaraciones o solicita que corrija el código antes de abrir la PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en marcha en `http://localhost:4321`. Si has detenido el servidor, vuelve a iniciarlo antes de enviar el prompt. La primera vez que se use Playwright MCP quizá tenga que descargar un navegador. Si falta, pide a Copilot que ejecute `npx playwright install chromium` y lo intente de nuevo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Después de comprobar la funcionalidad, publica la rama `filtering-vscode` para abrir una PR.

1. En el codespace, abre el panel **Source Control** y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Usa el botón **Sparkle** para generar un mensaje de confirmación.

   ![Captura del panel Source Control con los cambios](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para publicar la rama en el repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una PR, por ejemplo, mediante github.com o GitHub CLI. También puedes pedir a Copilot que la cree y asocie con la incidencia correspondiente.

1. Abre Copilot Chat y selecciona **New Chat**.
2. Comprueba que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Envía este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las acciones. La primera vez que use una herramienta de GitHub, quizá te pida autenticarte.
5. Observa cómo Copilot busca la incidencia correcta y crea la PR.
6. Selecciona el enlace que Copilot genera para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es MCP y cómo amplía las herramientas de Copilot.
- has añadido Playwright MCP al espacio de trabajo.
- lo has usado para probar manualmente la funcionalidad en un navegador.
- has publicado la rama y abierto una PR.

Has usado MCP para probar una funcionalidad, pero no es la única forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] que facilite tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
