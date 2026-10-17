---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo tiene lugar en tu equipo y en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones a medida que ocurren. Puedes revisar cada diff, aceptar o rechazar cambios individuales y orientar la conversación con prompts adicionales sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- orientarás al agente durante la sesión para añadir un control del modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, ampliarás el trabajo de la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Las ediciones aparecen con controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, lecturas, ediciones y comandos del terminal.
3. Cuando aparezca una edición:
   - selecciona **Keep** para aceptarla.
   - selecciona **Undo** para revertirla y pedir otra propuesta.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para revisar todos los archivos modificados.

> [!NOTE]
> Si una herramienta solicita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final aparecerá un mensaje que resume los cambios.

## Pide al agente que añada el modo claro

Ahora ampliarás la misma conversación para añadir un modo claro. Como continúas en la misma sesión, el agente conserva el contexto de su trabajo anterior: los componentes, el enfoque de estilo y el patrón de almacenamiento local.

1. En el cuadro de texto de Copilot Chat, envía este prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará sus cambios anteriores y propondrá actualizaciones para el modo claro.
3. Revisa cada cambio y usa **Keep** o **Undo** como antes.
4. Si el agente toma un rumbo que no te convence, por ejemplo, si intenta refactorizar demasiado la interfaz de los controles, detenlo con el botón **Stop** junto al cuadro de texto y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de cero. Puedes pedir ajustes («simplifica el CSS y usa variables en lugar de repetir selectores»), corregir el rumbo («integra el modo claro en el panel de configuración existente; no añadas un tercer control») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados; debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre varios archivos y revisa los diffs. Comprueba que se siguen las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés satisfecho, prepara y confirma los cambios en el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones, las has aceptado o rechazado y has enviado prompts adicionales para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado en tiempo real el trabajo de un agente.
- has aceptado y rechazado ediciones individuales.
- has orientado una sesión en curso con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
