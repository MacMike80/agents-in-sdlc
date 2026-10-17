---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o hay información que Copilot debería conocer, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] permiten comunicar pautas para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo recibe Copilot Chat el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos asociados a rutas.
- generarás la primera parte de la capa de datos para el filtrado —un helper de editoriales— con las instrucciones actuales.
- añadirás un estándar general al repositorio en `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás las instrucciones y el primer cambio del filtrado en `main` para que Copilot use las pautas actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir alguno de los estándares. Copilot no es determinista. El objetivo es observar la tendencia tras actualizar las instrucciones, no obtener exactamente los mismos caracteres.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de programación:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones proporcionarás a Copilot la información necesaria para que trabaje según estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para ayudar a Copilot a comprender el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que deben incluirse. Hay dos tipos:

- `.github/copilot-instructions.md`: archivo único que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información general aplicable a la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto, prácticas recomendadas y pautas globales.
- `.github/instructions/*.instructions.md`: archivos para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube también usan archivos `*.instructions.md` que incluyan el frontmatter `applyTo`.
>
> Copilot [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar los archivos

- Mantén `copilot-instructions.md` centrado en las pautas generales del proyecto, como la descripción, su estructura y los estándares globales.
- Usa `*.instructions.md` para tipos de archivo o tareas específicos.
- Escribe en lenguaje natural, con indicaciones claras y ejemplos de código adecuados e inadecuados.

No hay una única forma de usar la IA ni de crear instrucciones. Experimenta para descubrir qué funciona en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto verás pautas para [actualizaciones de la interfaz][ui-instructions], [Astro][astro-instructions] y otras tareas.
>
> Copilot también puede generar archivos de instrucciones. En VS Code puedes usar **Configure Chat → Generate Agent Instructions** y, en Copilot CLI, `/init`. La lección correspondiente lo explicará cuando sea relevante.
>
> Para encontrar plantillas, explora [awesome-copilot][awesome-copilot], que contiene archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

El repositorio incluye un `copilot-instructions.md` principal y varios archivos `*.instructions.md`. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las pautas anidadas de **GitHub Actions Workflows**; se aplican a todas las interacciones con Copilot.
3. Explora `.github/instructions`. Verás archivos para Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Su campo `applyTo` define un glob relativo a la raíz del repositorio; en este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Lee las pautas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces al final. Los enlaces a otros archivos de instrucciones y a archivos del proyecto son relativos a este archivo y permiten reutilizar pautas y ejemplos.

> [!NOTE]
> **Code formatting requirements** de `copilot-instructions.md` documenta las convenciones de código, pero todavía no exige documentación en el código. En los siguientes pasos añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para trabajar:

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve a tu codespace, abre Copilot Chat y selecciona **New Chat** para empezar una conversación nueva. El modo y el modelo variarán según el ejercicio; se indicarán cuando sea necesario.

Para ver el efecto de las instrucciones, primero enviarás un prompt con el archivo actual. Más adelante lo actualizarás y repetirás el prompt.

1. Cierra las pestañas del editor que estén abiertas, para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ir el helper.
3. En el selector de agentes de la vista Chat, elige **Agent** para que Copilot pueda aplicar cambios.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, a menudo en el helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esta convención procede de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario para el archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede añadir comentarios TSDoc aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar al repositorio

El archivo `.github/copilot-instructions.md` proporciona información general sobre el proyecto. Añade una pauta de programación para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca **Code formatting requirements**, que debería estar cerca de la línea 27. No contiene reglas sobre la documentación del código, lo que explica por qué el helper generado no tenía comentarios.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

> [!TIP]
> Las instrucciones del repositorio son adecuadas para estándares generales como la regla sobre comentarios TSDoc. Para lenguajes, tipos de archivo o tareas concretos, se pueden usar archivos `*.instructions.md`.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto de la conversación.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto incluye un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que servirá de base en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones, como cualquier otro elemento del repositorio, se gestionan mediante el control de código fuente.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba que están los cambios de instrucciones y del helper:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y los archivos que Copilot haya cambiado para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra cambios de apoyo adicionales (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Después de publicar la rama, crea una PR y relaciónala con la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia y crear y combinar la PR con las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realizará acciones en GitHub. La plantilla ya conecta las herramientas de GitHub al espacio de trabajo. La primera vez que Copilot use una, VS Code podría pedirte que inicies sesión. Sigue los pasos; más adelante aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que el selector de agentes muestra **Agent**.
3. Pide a Copilot que encuentre la incidencia relacionada con las instrucciones y cree una PR desde la rama actual:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando se te pida permitir acciones de GitHub, revisa el comando y selecciona **Allow** si procede.
6. Una vez creada la PR, pide a Copilot que la combine en `main` y actualice la rama local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Revisa las acciones de GitHub y los comandos de shell solicitados y selecciona **Allow** si procede.

Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot.

## Resumen y siguientes pasos

Has explorado cómo Copilot recibe contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt y observar el código generado con las instrucciones existentes.
- añadir un estándar general al archivo `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto sigue el nuevo estándar.
- confirmar los cambios de instrucciones y la base del helper en `main` para continuar en la siguiente lección.

A continuación, aplicarás esas instrucciones al [modo agente][next-lesson] mientras Copilot desarrolla una funcionalidad en varios archivos.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
