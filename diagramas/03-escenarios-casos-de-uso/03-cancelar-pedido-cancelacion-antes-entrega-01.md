| **Nombre del escenario:**   | Flujo principal - Cancelación de pedido antes de la entrega                                                                                  |     |               |         |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | --- | ------------- | ------- |
| **Nombre del caso de uso:** | Cancelar Pedido                                                                                                                              |     | **ID Única:** | CU03-01 |
| **Área**                    | Sistema Sabor Kiosco, cancelación de pedidos                                                                                                 |     |               |         |
| **Actor(es):**              | Usuario de Mostrador, Encargado                                                                                                              |     |               |         |
| **Descripción:**            | Permite cancelar un pedido que todavía no fue entregado, conservando el registro histórico del pedido y estableciendo el estado `CANCELADO`. |     |               |         |

| **Activar Evento:** | El Usuario de Mostrador o Encargado selecciona un pedido y solicita su cancelación antes de que sea entregado. | **Identificadores e iniciadores de caso de uso** |
| ------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Tipo de señal:**  | ☑️ Externa                                                                                                     | ☐ Temporal                                       |

| **Pasos desempeñados (ruta principal)**                                                     | **Información para los pasos** |
| ------------------------------------------------------------------------------------------- | ------------------------------ |
| 1. El actor selecciona el pedido que desea cancelar                                         | Número o referencia del pedido |
| 2. El sistema muestra el estado actual del pedido                                           | Estado del pedido              |
| 3. El sistema verifica que el pedido está `RECIBIDO` y que el actor puede cancelarlo        | Estado y permisos del actor    |
| 4. El sistema solicita confirmación de la cancelación                                       | Confirmación de cancelación    |
| 5. El actor confirma la cancelación                                                         | Confirmación del usuario       |
| 6. El sistema cambia el estado del pedido a `CANCELADO`                                     | Estado final                   |
| 7. El sistema conserva el identificador, los productos, pagos y demás información histórica | Registro histórico             |
| 8. El sistema registra el usuario y la fecha y hora de la cancelación                       | Registro de auditoría          |

**Ruta alternativa: pedido `EN_PREPARACION` (autorización especial)**

1. En el paso 3, el sistema detecta `EN_PREPARACION` y bloquea la cancelación directa del Usuario de Mostrador.
2. El Encargado (actor del diagrama correspondiente a la clase `UsuarioEncargado` del boceto) revisa la solicitud y autoriza o deniega la cancelación mediante su responsabilidad `autorizarCancelacion()`.
3. Si autoriza, el sistema continúa desde el paso 4 de la ruta principal y registra al Encargado que autorizó junto al responsable de la cancelación en la auditoría.
4. Si deniega, el pedido conserva su estado original: no se confirma ni se registra la cancelación.

En `LISTO` o `ENTREGADO` no se realiza una cancelación automática; cualquier devolución manual por parte del Encargado se trata fuera de este flujo.

| **Condiciones, suposiciones y preguntas** |                                                                                                                                                                                                                                                                                  |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Precondiciones:**                       | El pedido existe. Para la ruta principal está `RECIBIDO` y el actor tiene permisos; en `EN_PREPARACION` requiere autorización del Encargado antes de confirmar.                                                                                                                  |
| **Poscondiciones:**                       | El pedido queda en estado `CANCELADO`, el estado es final e irreversible y se conserva su registro histórico.                                                                                                                                                                    |
| **Suposiciones:**                         | El pedido puede ser identificado mediante su número o referencia. El sistema permite conservar el historial de las operaciones realizadas.                                                                                                                                       |
| **Requerimientos:**                       | RF3, RF5, RF8, RNF4, RNF5                                                                                                                                                                                                                                                        |
| **Aspectos sobresalientes:**              | La cancelación de un pedido `EN_PREPARACION` requiere intervención del Encargado; un pedido `LISTO` o `ENTREGADO` no se cancela por el flujo automático. No existe borrado físico: se conservan pedido, ítems, pagos e historial, y cualquier devolución debe quedar registrada. |
| **Prioridad:**                            | Alta                                                                                                                                                                                                                                                                             |
| **Riesgo:**                               | Medio                                                                                                                                                                                                                                                                            |
