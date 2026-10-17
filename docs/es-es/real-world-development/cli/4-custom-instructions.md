---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código del filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, asegúrate de que Copilot tenga acceso a esas pautas. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ya has creado el filtrado; ahora explorarás las instrucciones que utilizó Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo recibe el agente las instrucciones del repositorio y los archivos asociados a rutas.
- actualizarás el archivo de instrucciones para que se sigan los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene varias pautas y requisitos para las prácticas de programación:

- Los comentarios deben explicar la intención y las decisiones no evidentes, no repetir lo que ya hace el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su finalidad, parámetros y valores devueltos, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las pautas de formato y lint existentes.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que el trabajo siga estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot comprenda el estilo de programación y los requisitos del proyecto. Puedes especificar convenciones, bibliotecas y comentarios. Hay dos tipos:

- `.github/copilot-instructions.md`: un único archivo de instrucciones que se envía a Copilot en todas las solicitudes del repositorio.
- `.github/instructions/*.instructions.md`: archivos que proporcionan pautas para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones del proyecto

El proyecto de inicio ya incluye varios archivos de instrucciones. Explora su contenido antes de hacer cambios para poder observar el efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Lee la breve descripción del proyecto y las pautas de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora la carpeta `.github/instructions`. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, que define un glob para los archivos afectados.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces a otros archivos de instrucciones y a archivos del proyecto. Estos enlaces permiten dividir pautas extensas en archivos pequeños y reutilizables y dirigir a Copilot a ejemplos.

## Actualiza las instrucciones según las pautas del equipo

Los archivos existentes son un buen comienzo, pero falta una pauta. Modifica `copilot-instructions.md` para que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar los cambios, sal y reanuda la conversación de filtrado para que Copilot use las nuevas pautas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que estás en la rama `game-filters-cli`.
3. Pide a Copilot que aplique las pautas actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los nuevos comentarios TSDoc describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI recibe el contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En concreto:

- has explorado cómo recibe el agente las instrucciones del repositorio y los archivos asociados a rutas.
- has actualizado el archivo de instrucciones para que se sigan los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar el lint y las pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
