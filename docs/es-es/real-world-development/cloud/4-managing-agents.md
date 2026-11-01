---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste a Copilot cloud agent que se ocupara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque cloud agent está diseñado para trabajar de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay muchas herramientas para gestionar las tareas que asignas a cloud agent, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen pull requests abiertas, consultar las operaciones que han realizado e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas de cloud agent,
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también podía ser buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar los estilos del sitio e incorporar una función para cambiar de modo, parecía lógico incluirlo. Quieres orientar el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas de Copilot cloud agent

Vamos a consultar el estado de las tareas asignadas a Copilot cloud agent.

1. Navega hasta la [página de Copilot agents](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación de la base de código,
    - generar API para modificar juegos,
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa lo que ha hecho Copilot. Por ejemplo:
    - obtener el código del repositorio,
    - preparar el entorno de trabajo,
    - configurar servidores MCP,
    - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de pull request (PR) de la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Orienta a cloud agent

Ahora que has visto las tareas activas, pídele a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de prepararse y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea de cloud agent en la página de agentes, con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso en el que estaba trabajando al recibir tu mensaje.

El proceso tardará varios minutos, así que puedes hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas de GitHub Copilot cloud agent. Desde ahí puedes seguir las tareas activas y completadas, revisar el trabajo en curso e incluso redirigir una sesión para ajustar su alcance o proporcionar indicaciones adicionales.

Has explorado estos conceptos:

- has usado mission control y la página de agentes para supervisar las tareas de cloud agent,
- has redirigido una sesión en curso para pedir una funcionalidad adicional.

Ahora que Copilot está terminando las funcionalidades de accesibilidad, puedes [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
