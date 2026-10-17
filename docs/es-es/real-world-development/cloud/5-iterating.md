---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda probarla e implemente mejoras de accesibilidad con opciones de alto contraste y modo claro. Vamos a revisar los cambios y, si hace falta, proporcionar comentarios a Copilot.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la incorporación de la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las propuestas de documentación, juegos relacionados y accesibilidad antes de habilitar las revisiones para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente realiza tareas de forma asíncrona y sin supervisión, se aplican restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar la ejecución de los flujos de trabajo de GitHub Actions.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) generada por el agente: la que añade documentación al código. Para revisarla usarás la interfaz de PR habitual de GitHub.com.

> [!NOTE]
> Puede que aparezca un aviso de que el firewall ha bloqueado a GitHub Copilot. Es lo esperado, porque el acceso del agente a recursos externos, incluidas las llamadas a servidores MCP, está limitado por defecto. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** y abre la PR titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos.

3. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

4. Explora el código actualizado y revisa los comentarios TSDoc y otros documentos. Los cambios concretos pueden variar. Comprueba si se han seguido las instrucciones personalizadas que revisaste al principio del ejercicio 2.
5. Vuelve a la pestaña **Conversation** y desplázate hacia abajo. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

6. Comprueba que los flujos de trabajo se han puesto en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. Puede tardar varios minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo con `@copilot` en un comentario de la PR o en un comentario en línea sobre el código. Copilot verá el comentario e iniciará otra sesión para abordar la petición. Como los resultados no son deterministas, no hay una frase concreta que debas usar. Por ejemplo, puedes pedirle que:

- añada encabezados de comentario al principio de cada archivo de código para describir su propósito.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot** como harías con cualquier otra persona. Puedes usar una de las sugerencias anteriores.
2. Selecciona **View Session** para observar el trabajo de Copilot; verás que inicia otra sesión para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.
4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que el resultado te convenza. Cuando esté listo, marca la PR como lista para revisión y combínala en la rama principal.

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora vuelve a la PR que Copilot creó para la incidencia sobre los juegos relacionados en la página de detalles.

1. Ve al repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada algo parecido a **Show related games on the game details page**.
3. Selecciona **Files changed** y revisa el código generado.
4. Cuando hayas terminado, vuelve a **Conversation**. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.
5. Comprueba que las verificaciones del proyecto para la aplicación Astro se superen.
6. **Opcional:** cambia a la rama de la PR en tu Codespace para probar la funcionalidad. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Puedes pedirle cambios con otro comentario en **Conversation**, por ejemplo, ajustar la cantidad de juegos relacionados o añadir encabezados de comentario y comentarios TSDoc. Recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa las mejoras que implementó el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Ve a tu repositorio en GitHub.com, selecciona **Pull Requests** y abre la PR titulada algo parecido a **Add high contrast mode to website**.
2. Si Copilot sigue trabajando y ves la marca **[WIP]**, espera a que termine.
3. Selecciona **Files changed** y revisa la implementación. Presta atención a:
   - los componentes de interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación con teclado.
   - el código JavaScript/TypeScript que controla el cambio de modo.
4. Vuelve a **Conversation**. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.
5. Comprueba que las verificaciones del proyecto para la aplicación Astro se superen.
6. **Opcional:** cambia a la rama de la PR en tu Codespace con `git fetch origin` y `git checkout <branch-name>`. Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador; confirma que funcionan y que la selección se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a orientar la implementación según las prácticas recomendadas. Si detectas problemas o mejoras posibles, puedes pedir cambios mencionando a **@copilot** en un comentario.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta más útil cuando puedes asignarle incidencias reales del backlog y ocuparte mientras tanto de otras tareas. Crea algunas incidencias más y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para expresar interés en un juego en su página de detalles.
- añadir paginación a la lista de juegos.
- validar la entrada y gestionar errores en los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- has revisado los archivos de instrucciones personalizadas del repositorio para ver cómo influyen en el trabajo del agente.
- has asignado incidencias al agente en la nube y observado cómo prepara su entorno, planifica y trabaja de forma asíncrona.
- has creado y usado un agente personalizado de accesibilidad para añadir opciones de alto contraste y modo claro.
- has usado la página de agentes de Copilot como centro de control para supervisar y orientar la sesión de accesibilidad.
- has revisado e iterado sobre las PR del agente, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has terminado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo aprendido:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y la delegación opcional a la nube con `/delegate`.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el entorno de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
