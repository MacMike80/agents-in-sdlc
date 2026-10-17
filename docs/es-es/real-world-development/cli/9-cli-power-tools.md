---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye comandos de barra para interactuar con ella. Permiten acceder a funciones avanzadas, ver información interna y cambiar la configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de una conversación.
- usarás `/model` para revisar los modelos disponibles.
- aprenderás a exportar o compartir una sesión con `/share`.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar al agente en la nube.

## Escenario

Ya has completado el flujo de trabajo principal de CLI. Ahora revisarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas más largas o complejas, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando hace falta, pero también puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y el reparto del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara los resultados. Si la conversación era pequeña, puede que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Usa `/compact` para elegir cuándo hacerlo. Si cambias a una tarea distinta y quieres empezar de cero, usa `/clear` o `/new`.

## Elige un modelo

Cada modelo tiene puntos fuertes distintos y cada desarrollador puede tener preferencias diferentes. Copilot CLI permite ver y seleccionar el modelo que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir los aprendizajes ayuda al equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige el destino que mejor se adapte a tus necesidades; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo aislado para una tarea aparte.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabajará de forma asíncrona y quizá abra una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo o tareas remotas. Antes de usarlos, define una tarea nueva y bien delimitada y revisa el resultado con el flujo habitual. Para profundizar en el trabajo asíncrono de agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra permiten configurar Copilot CLI, compartir sesiones y consultar su funcionamiento interno. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto.
- has usado `/model` para revisar los modelos disponibles.
- has aprendido a exportar o compartir sesiones con `/share`.
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

Hay más comandos de barra y funciones de Copilot CLI que puedes explorar. Para cerrar este recorrido, [repasarás lo aprendido][next-lesson] y descubrirás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
