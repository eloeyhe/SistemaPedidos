| **Nombre del escenario:**   | Flujo principal - Modificación de pedido en estado RECIBIDO                                                                                                                               |   |               |         |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - | ------------- | ------- |
| **Nombre del caso de uso:** | Modificar Pedido                                                                                                                                                                          |   | **ID Única:** | CU02-01 |
| **Área**                    | Sistema Sabor Kiosco, modificación de pedidos                                                                                                                                             |   |               |         |
| **Actor(es):**              | Usuario de Mostrador, Cliente                                                                                                                                                             |   |               |         |
| **Descripción:**            | Permite modificar un pedido existente mientras permanece en estado `RECIBIDO`, manteniendo el identificador original y actualizando sus productos, cantidades, personalizaciones y total. |   |               |         |

| **Activar Evento:** | El Usuario de Mostrador busca un pedido existente y solicita modificarlo mientras se encuentra en estado `RECIBIDO`. | **Identificadores e iniciadores de caso de uso** |
| ------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Tipo de señal:**  | ☑️ Externa                                                                                                           | ☐ Temporal                                       |

| **Pasos desempeñados (ruta principal)**                                                 | **Información para los pasos**    |
| --------------------------------------------------------------------------------------- | --------------------------------- |
| 1. El Usuario de Mostrador busca el pedido por número o referencia                      | Número o referencia del pedido    |
| 2. El sistema muestra la información del pedido y verifica que su estado sea `RECIBIDO` | Estado actual del pedido          |
| 3. El Usuario de Mostrador selecciona la opción de modificar el pedido                  | Pedido seleccionado               |
| 4. Se agregan, eliminan o modifican productos y cantidades                              | Productos y cantidades del pedido |
| 5. Se modifican las personalizaciones necesarias                                        | Personalizaciones                 |
| 6. El sistema recalcula el total del pedido                                             | Total actualizado                 |
| 7. Si el pedido ya fue pagado, el sistema conserva el pago original y calcula la diferencia, registrando cada cobro adicional o devolución por separado | Pago registrado y diferencia de pago |
| 8. Se registra el ajuste del pago mediante efectivo o transferencia cuando corresponde  | Medio de pago                     |
| 9. El Usuario de Mostrador confirma la modificación                                     | Confirmación de modificación      |
| 10. El sistema actualiza el pedido conservando el mismo identificador                   | Pedido actualizado                |

| **Condiciones, suposiciones y preguntas** |                                                                                                                                                                                               |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Precondiciones:**                       | El pedido existe y se encuentra en estado `RECIBIDO`. El Usuario de Mostrador tiene acceso al sistema.                                                                                        |
| **Poscondiciones:**                       | El pedido queda actualizado conservando su identificador original. El total queda actualizado y, si corresponde, se registra el ajuste del pago.                                              |
| **Suposiciones:**                         | El pedido todavía no fue tomado por Cocina para su preparación. El Usuario de Mostrador dispone del número o referencia del pedido.                                                           |
| **Requerimientos:**                        | RF3, RF4, RF8, RNF2, RNF4, RNF5                                                                                                                                                                  |
| **Aspectos sobresalientes:**              | La validación del estado debe realizarse al confirmar para impedir modificaciones cuando el pedido ya pasó a `EN_PREPARACION`. El identificador original no cambia. Toda diferencia a cobrar o devolver se registra como ajuste vinculado al pedido y al pago original. |
| **Prioridad:**                            | Alta                                                                                                                                                                                          |
| **Riesgo:**                               | Medio                                                                                                                                                                                         |
