---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos de una aplicación suelen requerir actualizar varios archivos y ejecutar pruebas. Normalmente hay que localizar los archivos pertinentes, hacer los cambios, probarlos, depurar, descubrir qué archivo falta y volver a modificar el código. La lista puede ser larga.

Para eso sirve el modo agente de Copilot.

El modo agente puede trabajar con más autonomía en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube están diseñados para experiencias distintas. El modo agente trabaja en el IDE y permite ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona, como una persona del equipo, mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo agente de Copilot para explorar el proyecto e identificar archivos.
- implementar una funcionalidad que abarque la interfaz y la capa de datos.
- revisar los cambios y las pruebas que genere antes de integrarlos en el código.

## Escenario

El catálogo de juegos está creciendo y quieres que las personas puedan filtrarlo por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo agente.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer sus funciones. Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una aplicación Astro única que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local desde el frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/` y la interactividad se implementa con scripts Astro pequeños y específicos que usan las API DOM estándar.

### Inicia el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor esté listo, aparecerá una dirección similar a `http://localhost:4321`.

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, el enlace local se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor web del codespace.

### Explora el sitio web

La página principal muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones. Al seleccionar un juego, se abre su página de detalles con información adicional, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de seguir, vuelve a tu codespace, abre Copilot Chat y selecciona **New Chat** para empezar una conversación nueva. El modo y el modelo se indicarán en cada ejercicio.

La implementación inicial funciona, pero queremos añadir funciones. Cuando creaste el proyecto a partir de la plantilla, se generó un backlog de incidencias. Pide a GitHub Copilot que lo muestre.

1. En la lista de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> Los autores no están recomendando un modelo concreto. Se usó Claude Sonnet 4.5 al crear el taller para procurar que las sugerencias fueran relativamente coherentes. Los modelos de lenguaje son probabilísticos, por lo que los resultados pueden variar; es normal.

> [!NOTE]
> Debido al comportamiento probabilístico de los modelos de lenguaje, Copilot podría usar un comando MCP distinto y aun así completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada y la priorización sugerida.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le dará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que describe cómo añadir funcionalidad al sitio.

1. Abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas generales, entre ellas:
   - la arquitectura.
   - los principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - los enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y del proyecto. Las rutas son relativas al archivo de instrucciones; así se reutilizan pautas, se dividen archivos extensos y se ofrecen ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado se necesitan al menos tres cambios:

- añadir o ajustar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para introducir la interfaz de filtrado.

Antes de combinar los cambios, las pruebas deben ejecutarse y superarse. El modo agente puede realizar estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que sigue seleccionado **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad descrita en la incidencia:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los helpers, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar unos minutos en explorar, modificar archivos y ejecutar pruebas. Mientras trabaja, podrías ver los botones **Keep** y **Undo** en el editor. Espera a que termine; no tienes que seleccionarlos mientras el trabajo está en curso.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.
9. Inspecciona el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio abierto y prueba la nueva funcionalidad.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

Has explorado cómo usar el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y la capa de datos.
- explorar el proyecto e identificar archivos relacionados.
- revisar los cambios y las pruebas antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementa la paginación

Al crecer la lista de juegos, será necesario habilitar la paginación. Pide a Copilot que añada esta función aplicando lo aprendido:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si se puede elegir el tamaño de página o si será fijo.
- proporciona a Copilot las pautas necesarias para que la implementación coincida con tus preferencias.
- itera con Copilot y aporta cambios y contexto cuando haga falta; es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
