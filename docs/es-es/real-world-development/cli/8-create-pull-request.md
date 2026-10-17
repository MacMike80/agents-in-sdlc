---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad modificada, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios (PR). Tú combinaste la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo normal sería dividir la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, hemos mantenido todo el trabajo del filtrado y la calidad en una conversación y rama, y lo incluiremos en una única PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás todos los cambios y las pruebas de verificación.
- crearás la PR del filtrado.
- habilitarás Agent Merge después de revisar el trabajo y confirmarás que se combina la PR.

## Escenario

Durante el flujo del filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el resto del proceso de la PR, pero mantener el control de la autorización para combinar.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los impedimentos de la PR —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a aplicar la base cuando es necesario— y habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese paso. Antes de habilitarlo, revisa el trabajo y decide si está listo.

## Usa Agent Merge para gestionar la PR

Cuando hayas creado todo el código, revísalo en conjunto, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones se completen correctamente, la combinará.
7. Comprueba que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y, cuando el repositorio lo permita, completa manualmente la combinación después de revisar los cambios.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación y, ahora, el ciclo de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- has inspeccionado los cambios y las pruebas de verificación.
- has creado la PR del filtrado.
- has habilitado Agent Merge después de revisar el trabajo y confirmado que se ha combinado la PR.

A continuación, [explorarás otros comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y el uso compartido, y delegar trabajo de forma opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
