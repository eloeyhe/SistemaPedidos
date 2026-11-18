|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |ServicioPedidos  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Coordinar la carga de un pedido: crearlo con número único, asignar la referencia de retiro y agregar ítems y personalizaciones | Pedido, ItemPedido, Producto, Combo | Soy el punto de coordinación entre las pantallas y el dominio: recibo cada acción del usuario y la traduzco en operaciones sobre los pedidos | pedidos, pedidoEnCurso, itemEnCurso, ultimoNumeroPedido |
| Gestionar el cobro (validación y procesamiento vía ProcesadorPago) y registrar el pedido confirmado, o descartar el pedido en curso si el pago es rechazado | ProcesadorPago, Pago, Pedido | | |
| Coordinar la modificación de un pedido RECIBIDO: modificar ítems y personalizaciones, revalidar el estado, registrar el ajuste de pago y confirmar la modificación | Pedido, ItemPedido, Pago, ProcesadorPago, HistorialPago, HistorialCambioPedido | | |
| Solicitar la autorización del encargado para cancelar pedidos EN_PREPARACION | UsuarioEncargado | | |
| Comprobar la disponibilidad de datos y obtener los pedidos por estados para el monitor de activos | Pedido, EstadoPedido | | |
| Notificar a Cocina cada pedido confirmado | PantallaCocina | | |
| Atender las operaciones sobre pedidos existentes solicitadas desde las pantallas (modificar, cancelar, cambiar estado y consultar) | Pedido, PantallaPedidos, Usuario | | |
|  |  |  |  |
