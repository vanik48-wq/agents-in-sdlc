---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar sobre ella e implementara funciones de accesibilidad, como los modos de alto contraste y claro. Vamos a revisar los cambios del código y, si hace falta, a dar indicaciones a Copilot para mejorar el resultado.

### Escenario

Como hemos destacado en varias ocasiones, los fundamentos del diseño de software y DevOps no cambian con la incorporación de la IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para añadir documentación, crear la funcionalidad de juegos relacionados e implementar las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Copilot cloud agent realiza sus tareas de forma asíncrona y sin supervisión. Para garantizar la seguridad, se han establecido algunas restricciones:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama que utilizará para el código.
- El agente cloud se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar cualquier workflow de GitHub Actions antes de que se ejecute.
- [El acceso a los recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera pull request (PR) que generó GitHub Copilot cloud agent: la que añade documentación al código. Para esta tarea, usarás la interfaz estándar de pull requests de GitHub.com.

> [!NOTE]
> Al explorar la PR, quizá veas una advertencia que indica que un firewall ha bloqueado a GitHub Copilot. Es **lo esperado**, ya que Copilot tiene acceso limitado de forma predeterminada a los recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o desactivar el firewall para Copilot cloud agent][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Pull Requests** para abrir la lista de pull requests.
3. Abre la pull request titulada algo parecido a **Add missing documentation** o con un título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la pull request incluirá la etiqueta **[WIP]**. Si es así, espera a que termine. Puede tardar unos minutos; mientras tanto, puedes hacer una pausa o repasar lo que has aprendido.

4. Cuando la pull request esté lista, selecciona la pestaña **Files changed**.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá los nuevos comentarios de documentación TSDoc y otros cambios en la documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de archivo. Estas pautas proceden de los archivos de instrucciones que consultaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo esté correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay workflows pendientes de aprobación.
8. Si hay workflows pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar workflows](../../../_images/shared-approve-workflows.png)
9. Los workflows aparecerán en cola en la sección de comprobaciones de la pull request. Si todo va bien, verás que se superan las comprobaciones del proyecto para la única aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una pull request no es unidireccional. Puedes mencionarlo en comentarios de la pull request, igual que harías con otros miembros del equipo, o añadir comentarios en línea en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto exacto que debas utilizar. Puedes pedirle, por ejemplo:

- que añada al principio de cada archivo un encabezado con una breve descripción de su contenido,
- que añada comentarios de documentación TSDoc a los archivos TypeScript y Astro,
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia de documentación que quieras incorporar a la base de código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Fíjate en que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la pull request.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, verás un nuevo commit en la pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot generó para resolver la incidencia sobre cómo mostrar juegos relacionados en la página de detalles de un juego.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** o más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código que ha generado.
5. Cuando hayas revisado las actualizaciones y todo esté correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay workflows pendientes de aprobación.
7. Si hay workflows pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar workflows](../../../_images/shared-approve-workflows.png)
8. Los workflows aparecerán en cola en la sección de comprobaciones de la pull request. Si todo va bien, verás que se superan las comprobaciones de la única aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiarte a esta rama en tu Codespace y probar manualmente la funcionalidad de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con Copilot cloud agent y pedirle cambios. Por ejemplo, puedes pedirle que modifique cuántos juegos relacionados se muestran o que añada encabezados de archivo y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de que actualizaras las instrucciones personalizadas. Puedes pedir esos cambios añadiendo un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una nueva sesión.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde mission control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** o más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la pull request incluirá la etiqueta **[WIP]**. Si es así, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de la interfaz para cambiar de modo,
   - el uso del almacenamiento local para conservar las preferencias de usuario,
   - los cambios de CSS o estilo para los modos de alto contraste y claro,
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.),
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo esté correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay workflows pendientes de aprobación.
8. Si hay workflows pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar workflows](../../../_images/shared-approve-workflows.png)
9. Los workflows aparecerán en cola en la sección de comprobaciones de la pull request. Si todo va bien, verás que se superan las comprobaciones de la única aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiarte a esta rama en tu Codespace y probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los interruptores de los modos de alto contraste y claro en el navegador. Comprueba que funcionan como esperas y que los ajustes se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas algún problema o posible mejora, puedes mencionar a **@copilot** en un comentario para pedir cambios, igual que hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

Cloud agent funciona mejor cuando le asignas elementos reales del backlog y puedes dedicarte a otras tareas. Para acostumbrarte a trabajar así, crea algunas incidencias más en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para que la gente muestre interés por patrocinar un juego, en la página de detalles del juego,
- implementar la paginación en la página de la lista de juegos,
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el taller de cloud agent. En estas lecciones:

- **Has examinado los archivos de instrucciones incluidos en el repositorio** para ver más adelante cómo influyen en el trabajo de cloud agent.
- **Has asignado incidencias a Copilot cloud agent** y has visto cómo prepara el entorno, planifica y ejecuta tareas de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las pull requests de cloud agent**, mencionando a `@copilot` para solicitar cambios y aprobando workflows.

## Repasa y continúa

Has completado el taller de cloud agent. Si quieres seguir explorando, los otros talleres complementan lo que has practicado:

- 🖥️ **[Taller de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración de MCP directamente desde el IDE.
- 💻 **[Taller de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` hacia cloud agent.

En tu propio repositorio, prueba estas ideas:

- asigna a cloud agent una incidencia de refactorización o de cobertura de pruebas,
- crea un agente personalizado para otro ámbito,
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el workflow de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el firewall de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
