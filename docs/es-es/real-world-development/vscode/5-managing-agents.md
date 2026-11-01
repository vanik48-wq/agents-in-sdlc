---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición de archivos mientras se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y dirigir la conversación con peticiones de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas en los archivos;
- dirigirás al agente durante la sesión para que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha hecho una petición adicional: también quiere un control para el modo claro, para los usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En vez de empezar una conversación nueva desde cero, dirigirás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso que realiza el agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición de archivo aparece en línea en el chat, junto con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Verás lecturas de archivos, ediciones y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos que el agente ha modificado durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos con los que estés de acuerdo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios realizados.

## Pedir al agente que añada el modo claro

Ahora que está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control para el modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque utilizado para los estilos y el patrón de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía la siguiente petición de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá modificaciones para añadir el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** para cada diferencia.
4. Si el agente toma una dirección que no te convence —por ejemplo, si intenta reorganizar la interfaz de los controles de una forma que no te gusta— puedes detenerlo con el botón **Stop** junto al campo de entrada del chat y enviar un mensaje de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Utiliza peticiones de seguimiento para solicitar ajustes («simplifica el CSS: utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones de los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre algunos de los archivos modificados y revisa sus diferencias. Comprueba que se hayan aplicado los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición de archivo, la has aceptado o rechazado y has enviado peticiones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado cómo:

- supervisar en tiempo real el trabajo de un agente;
- aceptar y rechazar ediciones concretas de archivos;
- dirigir una sesión mientras está en curso mediante peticiones de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios que ha realizado el agente personalizado local durante estas prácticas.

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
