---
title: "Ejercicio 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones, si es que hay alguna, no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en la lista de tareas pendientes porque no había tiempo para implementarlas. GitHub Copilot cloud agent está pensado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una pull request en borrador para que la revise un desarrollador. Esto permite delegar tareas tediosas, acelerar el desarrollo y liberar a los desarrolladores para que se centren en objetivos de mayor alcance.

Con Copilot cloud agent explorarás cómo:

- personalizar el entorno de generación de código,
- asegurarte de que las operaciones se realicen de forma segura,
- definir incidencias con un alcance claro,
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, verás que no hay prácticamente ninguna. Como primer paso, quieren añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría para que la gente pueda seguir explorando. No necesita una implementación pulida todavía; solo quiere algo que sirva para realizar pruebas de aceptación de la experiencia de usuario. Esto les bloquea, aunque ahora mismo hay otras incidencias con mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente y son ideales para asignarlas a Copilot cloud agent. El agente puede trabajar en ellas de forma asíncrona mientras los desarrolladores se ocupan de otras tareas. Después, podrán volver para revisar el trabajo de Copilot y comprobar que todo está como se espera.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma muy parecida a un desarrollador. Al igual que cuando trabajas con otra persona, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una pull request en borrador para hacer un seguimiento del trabajo, prepara un entorno y empieza a trabajar. Puedes consultar su sesión mientras sigue en curso o una vez terminada. Cuando la solución esté lista para que la revises, te mencionará en la pull request.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace magia: no te permite dejar de pensar. Hasta las tareas que parecen sencillas pueden volverse complejas cuando se examinan de cerca, así que [define con cuidado el alcance de las tareas para Copilot cloud agent][cloud-agent-best-practices]. Trátalo como a un programador en pareja que usa IA: trabaja por etapas, aprende, experimenta y adapta el enfoque sobre la marcha. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo guían a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo a Copilot cloud agent, consulta los archivos de instrucciones incluidos en este repositorio sin modificarlos; más adelante podrás observar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub de tu repositorio o en un codespace que ya tengas abierto:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el front matter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes a cloud agent la incidencia `Code lacks documentation` en la sección siguiente, comprueba si la pull request resultante contiene comentarios TSDoc, sigue las convenciones de TypeScript e incluye encabezados de archivo. Estas pautas proceden de los archivos de instrucciones. Volveremos a este punto al revisar las pull requests en un ejercicio posterior.

## Configura el entorno de desarrollo para Copilot cloud agent

La creación de código suele requerir un entorno específico y la ejecución de algunos scripts de configuración para garantizar que todo esté preparado, independientemente de quién realice el trabajo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

Cloud agent usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [workflow de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este workflow ya está configurado para facilitar el desarrollo del taller y la experiencia de aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y preparada para la única aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos en el nivel mínimo necesario para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación ahora usa Astro + Drizzle/libSQL)
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

      # Migra y prepara la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro workflow de GitHub, pero hay algunos puntos clave:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la pull request.
- El disparador `workflow_dispatch` permite ejecutar el workflow manualmente desde la pestaña **Actions** del repositorio. Esto sirve para comprobar que se ejecuta correctamente en lugar de esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo comprende la importancia de la documentación, la mayoría de los proyectos tienen información obsoleta o carecen de ella. Este tipo de deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta mantener la base de código o incorporar nuevos desarrolladores al equipo. Copilot es especialmente útil para crear documentación, así que esta es una tarea ideal para asignar a Copilot cloud agent. El agente trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás para revisar el resultado.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Establece el **Title** en `Code lacks documentation`.
6. Establece la **Description** en:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En la parte derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la pull request (PR) que se acaba de generar. Tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece una PR nueva, espera un momento y vuelve a cargar la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno (ejecutando el workflow descrito antes), analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que Copilot va a completar.
14. Desplázate hacia abajo por la cronología de la pull request. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que volver a cargar la ventana para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema. Verás cómo explora el código y analiza el estado del proyecto, se detiene a pensar y decide cuál es el plan adecuado, y crea código.

El proceso probablemente tardará varios minutos. Uno de los objetivos principales de Copilot cloud agent es realizar tareas de forma asíncrona para que puedas centrarte en otras cosas. Aprovecharemos esa función asignándole otra tarea y, después, nos pondremos a escribir código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Como ya hemos indicado, una de las grandes ventajas de GitHub Copilot cloud agent es que permite dividir el trabajo. Puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, ese tiempo podría dedicarse a otras tareas. ¡Vamos a asignársela a Copilot cloud agent!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Establece el **Title** en `Show related games on the game details page`.
6. Establece la **Description** en:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En la parte derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás varios 👀 en el primer comentario de la incidencia. Esto indica que Copilot ya está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Copilot cloud agent funciona de forma parecida a un ingeniero de software: no hace falta que lo supervises continuamente, pero sí que revises el resultado cuando termine. Ahora vamos a centrarnos en crear y utilizar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignar incidencias a cloud agent para que las resuelva de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código,
- garantizar que las operaciones se realicen de forma segura,
- definir incidencias con un alcance claro,
- asignar incidencias a Copilot.

Mientras cloud agent trabaja en segundo plano, ahora podemos centrarnos en crear y utilizar agentes personalizados. [Copilot cloud agent también puede usar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que vimos en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el workflow de configuración de Copilot cloud agent][cloud-agent-best-practices]

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
