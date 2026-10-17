---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que realice tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas que asignas al agente en la nube, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, explorar sus acciones e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se estaba trabajando en cambiar el estilo del sitio y añadir funciones de activación y desactivación, parecía lógico incluirlo. Guiarás el trabajo del agente para que implemente el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube

Veamos el estado de las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver, entre otras, las tareas que has asignado a Copilot para:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las acciones que ha realizado Copilot, que pueden incluir:
   - clonar el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo.

> [!NOTE]
> Las acciones concretas varían según el estado del trabajo y el enfoque que siga Copilot.

4. Observa también el panel de la solicitud de incorporación de cambios (PR) de la derecha. Desde ahí puedes ver la PR y los archivos modificados para supervisar el trabajo.

## Dirige al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto depende del nombre que Copilot haya elegido y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha completado la configuración y ha empezado a trabajar. Verás mensajes similares a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma el prompt y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se pondrá manos a la obra con la tarea actualizada. Incorporará la solicitud nueva al flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Este proceso tardará varios minutos; puedes aprovechar ese tiempo para repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y dirigir las tareas del agente de GitHub Copilot en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y modificar el rumbo de las sesiones en curso.

Has aprendido a:

- explorar el centro de control y la página de agentes para supervisar las tareas.
- redirigir una sesión en curso para solicitar más funcionalidad.

Mientras Copilot termina las funciones de accesibilidad, continúa con la [revisión iterativa de las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
