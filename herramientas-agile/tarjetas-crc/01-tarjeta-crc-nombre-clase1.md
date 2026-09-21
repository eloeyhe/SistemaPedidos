|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | Pedido | | |
| **Superclase:** | Ninguna | | |
| **Subclase:** | Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Registrar ítems, validar cantidades y personalizaciones | ItemPedido, Producto, PersonalizacionItem | El pedido representa la intención de compra del cliente como unidad de negocio | numeroPedido, referenciaRetiro, fecha, estado, prioritario |
| Calcular total, ajustar subtotales y revalidar pagos | ItemPedido, Pago, EstadoPedido | El pedido mantiene el valor real del consumo y la diferencia económica | total, subtotal, montoPagado |
| Cambiar de estado, bloquear modificaciones en preparación y cancelar | EstadoPedido, AuditoriaPedido, Usuario | El pedido encapsula su ciclo de vida y la seguridad del negocio | estadoActual, fechaCambioEstado |
| Enviar a cocina y mantener trazabilidad de la operación | Usuario, Cocina, AuditoriaPedido | La lógica del pedido debe reflejar el estado de producción en tiempo real | eventoEnvio, usuarioResponsable |
| Priorización, confirmación y control de retiro al cliente | Usuario, Cocina | El pedido coordina la experiencia operativa del mostrador y la cocina | prioridad, referenciaRetiro, entregado |
|  |  |  |  |