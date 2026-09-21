|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | Pedido | | |
| **Superclase:** | Ninguna | | |
| **Subclase:** | Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Registrar y validar los datos básicos del pedido, la referencia de retiro y la prioridad | Usuario, ItemPedido, EstadoPedido | El pedido representa la compra como una unidad coherente y completa del negocio | numeroPedido, referenciaRetiro, fechaCreacion, estadoActual, prioritario |
| Agregar, quitar y modificar ítems sin romper la identidad del pedido | ItemPedido, Producto, PersonalizacionItem | Cada cambio debe conservar la integridad del pedido y su número original | total, subtotal, cantidadItems |
| Recalcular el total del pedido y ajustar diferencias de cobro | ItemPedido, Pago, EstadoPedido | El objeto pedido es el responsable del valor final que se paga o reembolsa | total, montoPagado, saldoPendiente |
| Cambiar de estado, bloquear cambios fuera del flujo permitido y cancelar de forma definitiva | EstadoPedido, AuditoriaPedido, Usuario | El pedido encapsula su ciclo de vida y la regla del negocio para no alterar órdenes ya preparadas | estadoActual, fechaCambioEstado |
| Enviar la comanda a cocina y soportar la entrega al cliente | Cocina, Usuario, AuditoriaPedido | El pedido centraliza la operación desde la toma hasta la entrega | prioridad, referenciaRetiro, fechaEntrega |
|  |  |  |  |
|  |  |  |  |
