| **Nombre del escenario:**   | Flujo principal - Avance correcto del pedido durante su preparación                                                                  |   |               |         |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | - | ------------- | ------- |
| **Nombre del caso de uso:** | Cambiar Estado de Pedido                                                                                                             |   | **ID Única:** | CU04-01 |
| **Área**                    | Sistema Sabor Kiosco, seguimiento del estado de pedidos                                                                              |   |               |         |
| **Actor(es):**              | Cocina, Usuario de Mostrador                                                                                                         |   |               |         |
| **Descripción:**            | Permite actualizar el estado de un pedido siguiendo las transiciones definidas: `RECIBIDO`, `EN_PREPARACION`, `LISTO` y `ENTREGADO`. |   |               |         |

| **Activar Evento:** | Cocina o el Usuario de Mostrador selecciona un pedido y solicita avanzar su estado según corresponda al proceso. | **Identificadores e iniciadores de caso de uso** |
| ------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Tipo de señal:**  | ☑️ Externa                                                                                                       | ☐ Temporal                                       |

| **Pasos desempeñados (ruta principal)**                                                     | **Información para los pasos** |
| ------------------------------------------------------------------------------------------- | ------------------------------ |
| 1. El actor selecciona un pedido existente                                                  | Pedido registrado              |
| 2. El sistema muestra el estado actual del pedido                                           | Estado actual                  |
| 3. Cocina cambia el pedido de `RECIBIDO` a `EN_PREPARACION`                                 | Transición de estado           |
| 4. Cocina cambia el pedido de `EN_PREPARACION` a `LISTO` cuando finaliza la preparación     | Transición de estado           |
| 5. El Usuario de Mostrador cambia el pedido de `LISTO` a `ENTREGADO` al realizar la entrega | Transición de estado           |
| 6. El sistema valida cada transición antes de realizarla                                    | Máquina de estados             |
| 7. El sistema actualiza el estado del pedido cuando la transición es válida                 | Estado actualizado             |
| 8. El sistema mantiene un único estado actual para cada pedido                              | Estado del pedido              |

| **Condiciones, suposiciones y preguntas** |                                                                                                                                                                                           |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Precondiciones:**                       | El pedido existe. El actor está autorizado para realizar la transición correspondiente. El estado actual permite la transición solicitada.                                                |
| **Poscondiciones:**                       | El pedido queda registrado con su nuevo estado y mantiene un único estado actual válido.                                                                                                  |
| **Suposiciones:**                         | Cocina realiza los cambios relacionados con la preparación y el Usuario de Mostrador realiza la entrega.                                                                                  |
| **Reunir requerimentos:**                 | RF3, RNF4                                                                                                                                                                                 |
| **Aspectos sobresalientes:**              | ¿Qué ocurre si se intenta realizar una transición no permitida? ¿Qué mensaje debe mostrar el sistema ante una transición inválida? ¿Qué permisos tiene cada actor para cambiar el estado? |
| **Prioridad:**                            | Alta                                                                                                                                                                                      |
| **Riesgo:**                               | Medio                                                                                                                                                                                     |
