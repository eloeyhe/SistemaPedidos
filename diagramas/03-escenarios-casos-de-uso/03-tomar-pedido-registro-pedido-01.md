| **Nombre del escenario:**   | Flujo principal - Registro de pedido exitoso                                                                                                                                                            |   |               |         |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - | ------------- | ------- |
| **Nombre del caso de uso:** | Tomar Pedido                                                                                                                                                                                            |   | **ID Única:** | CU01-01 |
| **Área**                    | Sistema Sabor Kiosco, gestión de pedidos                                                                                                                                                                |   |               |         |
| **Actor(es):**              | Usuario de Mostrador y Cliente (interacción indirecta)                                                                                                                                                  |   |               |         |
| **Descripción:**            | Permite al Usuario de Mostrador registrar un nuevo pedido, seleccionar productos o combos, agregar personalizaciones, calcular el total, registrar el pago y enviar automáticamente el pedido a Cocina. |   |               |         |

| **Activar Evento:** | El Usuario de Mostrador inicia un nuevo pedido desde el sistema y carga los datos solicitados por el cliente. | **Identificadores e iniciadores de caso de uso** |
| ------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Tipo de señal:**  | ☑️ Externa                                                                                                    | ☐ Temporal                                       |

| **Pasos desempeñados (ruta principal)**                                                    | **Información para los pasos**      |
| ------------------------------------------------------------------------------------------ | ----------------------------------- |
| 1. El Usuario de Mostrador inicia un nuevo pedido                                          | Sistema de gestión de pedidos       |
| 2. El sistema genera un número único para el pedido y registra la fecha y hora             | Identificador único del pedido      |
| 3. El Usuario de Mostrador ingresa el nombre o referencia para el retiro                   | Datos del pedido                    |
| 4. Se seleccionan los productos o combos y sus cantidades                                  | Catálogo de productos y combos      |
| 5. Se agregan las personalizaciones correspondientes a los productos                       | Personalizaciones del pedido        |
| 6. El sistema calcula los subtotales y el total del pedido                                 | Precios históricos de los productos |
| 7. Se selecciona y valida el medio de pago: efectivo o transferencia                       | Medio de pago permitido             |
| 8. El sistema registra el pago asociado al pedido                                          | Registro de Pago                    |
| 9. El sistema registra el pedido con estado `RECIBIDO`, conservando los precios históricos | Pedido y detalle del pedido         |
| 10. El sistema envía automáticamente el pedido confirmado a Cocina                         | Pantalla de Cocina                  |

| **Condiciones, suposiciones y preguntas** |                                                                                                                                                                                                   |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Precondiciones:**                       | El Usuario de Mostrador tiene acceso al sistema. Los productos o combos seleccionados se encuentran disponibles.                                                                                  |
| **Poscondiciones:**                       | El pedido queda registrado con un identificador único, estado `RECIBIDO`, precios históricos y pago asociado. El pedido queda visible para Cocina.                                                |
| **Suposiciones:**                         | El Usuario de Mostrador dispone de la información necesaria proporcionada por el cliente. El sistema tiene acceso al catálogo de productos y al registro de pedidos.                              |
| **Requerimientos:**                        | RF1, RF2, RF3, RF6, RF8, RNF2, RNF4, RNF5                                                                                                                                                            |
| **Aspectos sobresalientes:**              | El pedido no se guarda si falta stock o el pago por transferencia es rechazado. El precio unitario y las personalizaciones quedan congelados como histórico. La confirmación debe enviar el pedido a Cocina sin una segunda carga manual. |
| **Prioridad:**                            | Alta                                                                                                                                                                                              |
| **Riesgo:**                               | Medio                                                                                                                                                                                             |
