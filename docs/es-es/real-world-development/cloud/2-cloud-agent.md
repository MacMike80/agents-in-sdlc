---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidades que llevan mucho tiempo en el backlog porque no había tiempo para implementarlas. El agente de GitHub Copilot en la nube puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que una persona la revise. Así puedes delegar tareas tediosas y acelerar el desarrollo, mientras el equipo se centra en asuntos de mayor alcance.

En este ejercicio explorarás cómo usar el agente de Copilot en la nube para:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- entender la importancia de definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que desarrollaron la primera versión del sitio dejaron la documentación en un estado deficiente: en realidad, no hay ninguna. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. En la página de detalles de cada juego quiere mostrar otros juegos relacionados de la misma categoría, para que las personas puedan seguir explorando. Por ahora no hace falta una implementación pulida; solo necesitan algo que les permita probar la experiencia de usuario. Aunque esto les bloquea, hay incidencias de mayor prioridad.

Son tareas que suelen aplazarse y que se pueden asignar al agente de Copilot en la nube. Mientras trabaja de forma asíncrona, puedes ocuparte de otras cosas y revisar después su resultado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a como lo haría una persona. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al hacerlo, Copilot crea una PR en borrador para registrar el progreso, configura el entorno y comienza a trabajar. Puedes consultar su sesión mientras trabaja o cuando haya terminado. Cuando esté lista para que revises la solución propuesta, te mencionará en la PR.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia ni elimina la necesidad de pensar. Incluso las tareas que parecen sencillas se complican cuando se analizan en detalle. Por eso, [define con cuidado el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a una persona colaboradora de programación: divide el trabajo en fases, aprende, experimenta y adapta el enfoque. La incorporación de IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has aprendido cómo guían a Copilot las instrucciones personalizadas. Si ya has completado un ejercicio práctico sobre instrucciones, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones del repositorio en modo de solo lectura para poder observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la siguiente sección, fíjate en si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas pautas proceden de los archivos de instrucciones; volveremos a mencionarlas al revisar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente en la nube

Para generar código, normalmente se necesita un entorno concreto y algunos scripts de configuración para instalar las herramientas y dependencias necesarias. Lo mismo sucede cuando asignas tareas a Copilot, que trabaja de forma similar a una persona desarrolladora.

El agente en la nube usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. El archivo ya viene configurado para el taller e instala Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y rellenada con datos para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Usa los permisos mínimos necesarios para estos pasos. Copilot recibirá un token independiente para sus operaciones.
    permissions:
      # Si los pasos clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso contents: read. Si no se clona, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (ahora toda la aplicación es Astro + Drizzle/libSQL)
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

      # Migra y rellena la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos puntos importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- Observa el activador `workflow_dispatch`, que permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin tener que esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo conoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevas personas. Copilot puede ayudar a crear documentación, por lo que es una tarea idónea para el agente en la nube. Trabajará en segundo plano y revisarás el resultado en un ejercicio posterior.

1. Abre el repositorio en github.com en una pestaña nueva.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Abre la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece en la lista, espera un momento o dos y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> Puede que la lista tarde varios minutos en aparecer. Copilot está preparando el entorno mediante el flujo de trabajo descrito, analizando el proyecto y decidiendo cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la PR hasta el mensaje que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session** para abrir la sesión.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve la tarea: explora el código y entiende el estado actual, se detiene a analizar y planificar el enfoque y, después, genera código.

El proceso puede tardar varios minutos. La ventaja del agente en la nube es que trabaja de forma asíncrona, así que puedes ocuparte de otras tareas. Aprovecharemos esa capacidad y le asignaremos otra mientras nosotros desarrollamos una funcionalidad.

## Añade una sección de juegos relacionados

Una ventaja del agente en la nube es que permite repartir el trabajo para que puedas centrarte en otras tareas. Añadir una sección de juegos relacionados quizá no tarde mucho, pero ese tiempo se puede dedicar a otra cosa. Asígnale la tarea al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo aparecerá un emoji 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como el agente funciona de forma similar a una persona desarrolladora, no tienes que supervisarlo constantemente; basta con revisar el trabajo cuando termine. Ahora puedes crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Puede ocuparse de deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has aprendido a:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes pasar a [crear y usar agentes personalizados][next-lesson]. El agente en la nube también [puede usar servidores MCP][cloud-agent-mcp] y dispone de las instrucciones personalizadas que ya has explorado.

## Recursos

- [Información sobre el agente de Copilot][copilot-agents]
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
