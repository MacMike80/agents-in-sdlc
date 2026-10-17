---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Se definen en archivos Markdown de `.github/agents` y proporcionan instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para que Copilot realice determinados tipos de trabajo. Los equipos pueden convertir su experiencia en agentes reutilizables, como un agente de accesibilidad que siga [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo contiene frontmatter YAML con al menos `name` y `description`, y un prompt Markdown que describe el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente mediante Markdown y explican a la IA cómo realizar tareas. La diferencia más sencilla es que el **agente personalizado** es quien trabaja y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad y podría invocar habilidades para gestionar PR o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Experimenta y repite el proceso hasta encontrar lo que funciona para tu entorno.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todos, con independencia de sus capacidades o preferencias visuales. Algunas personas han indicado que les cuesta leer el tema oscuro porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha pedido añadir un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es fundamental, por lo que usarás un agente personalizado para implementar esta función con rapidez.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya existe un agente personalizado de accesibilidad. Vamos a revisar las pautas que ofrece a Copilot.

Vuelve a tu codespace y cambia a una rama nueva basada en `main` para separar este trabajo de la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios en los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades del agente al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de HTML, CSS y JavaScript.
   - errores habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Al seleccionar uno, la conversación queda acotada a las instrucciones y herramientas de ese agente.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para empezar una conversación nueva. En cada ejercicio se indicarán el modo y el modelo adecuados.

1. Si aún no está seleccionado, elige **Agent** en el selector de agentes.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes de la parte inferior de Chat; mostrará el agente activo, que por defecto es **default**.
3. Selecciona **Accessibility agent**.
4. Envía este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos. Cada edición aparecerá en el chat con la ruta del archivo y un diff para que puedas revisarla.
6. El indicador **Files changed** se actualizará para mostrar los archivos que ha modificado el agente.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y pruebas relacionadas.

En la siguiente lección revisarás y orientarás el trabajo en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que guían el trabajo de Copilot.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se producen y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
