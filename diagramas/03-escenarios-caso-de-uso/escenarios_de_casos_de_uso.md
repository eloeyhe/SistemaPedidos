# Escenarios de Casos de Uso

Este documento contiene los escenarios correspondientes a los cinco casos de uso principales definidos para el MVP de **Sabor Kiosco**.

| ID      | Caso de uso               | Escenario                                            | Archivo                                                                                                          |
| ------- | ------------------------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| CU01-01 | Tomar Pedido              | Registro de pedido exitoso                           | [03-tomar-pedido-registro-pedido-01.md](./03-tomar-pedido-registro-pedido-01.md)                                 |
| CU02-01 | Modificar Pedido          | Modificación de pedido en estado `RECIBIDO`          | [03-modificar-pedido-modificacion-recibido-01.md](./03-modificar-pedido-modificacion-recibido-01.md)             |
| CU03-01 | Cancelar Pedido           | Cancelación de pedido antes de la entrega            | [03-cancelar-pedido-cancelacion-antes-entrega-01.md](./03-cancelar-pedido-cancelacion-antes-entrega-01.md)       |
| CU04-01 | Cambiar Estado de Pedido  | Avance correcto del pedido durante su preparación    | [03-cambiar-estado-pedido-avance-preparacion-01.md](./03-cambiar-estado-pedido-avance-preparacion-01.md)         |
| CU05-01 | Consultar Pedidos Activos | Consulta de pedidos activos priorizando los urgentes | [03-consultar-pedidos-activos-prioridad-urgentes-01.md](./03-consultar-pedidos-activos-prioridad-urgentes-01.md) |

## Casos de uso incluidos

### CU01 – Tomar Pedido

Permite registrar un nuevo pedido, seleccionar productos o combos, agregar personalizaciones, registrar el pago y enviar el pedido a Cocina.

### CU02 – Modificar Pedido

Permite modificar un pedido mientras se encuentre en estado `RECIBIDO`, conservando su identificador original.

### CU03 – Cancelar Pedido

Permite cancelar un pedido antes de su entrega, conservando su información histórica y estableciendo el estado `CANCELADO`.

### CU04 – Cambiar Estado de Pedido

Permite avanzar el pedido mediante las transiciones de estado definidas para el proceso: `RECIBIDO` → `EN_PREPARACION` → `LISTO` → `ENTREGADO`.

### CU05 – Consultar Pedidos Activos

Permite consultar los pedidos que se encuentran activos, mostrando primero los pedidos marcados como urgentes dentro de un mismo estado.
