|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |ServicioPedidos  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Coordinar la carga de un pedido: crearlo con número único, asignar la referencia de retiro y agregar ítems y personalizaciones | Pedido, ItemPedido, Producto, Combo | Soy el punto de coordinación entre las pantallas y el dominio: recibo cada acción del usuario y la traduzco en operaciones sobre los pedidos | pedidos, pedidoEnCurso, itemEnCurso, ultimoNumeroPedido |
| Gestionar el cobro y registrar el pedido confirmado, o descartar el pedido en curso si el pago es rechazado | Pago, Pedido | | |
| Notificar a Cocina cada pedido confirmado | PantallaCocina | | |
| Atender las operaciones sobre pedidos existentes solicitadas desde las pantallas (modificar, cancelar, cambiar estado y consultar) | Pedido, PantallaPedidos, Usuario | | |
|  |  |  |  |
