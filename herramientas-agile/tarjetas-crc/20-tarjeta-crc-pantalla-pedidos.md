|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |PantallaPedidos  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Recibir las acciones del Usuario de Mostrador para tomar, buscar, modificar y marcar como prioritario un pedido | UsuarioMostrador, ServicioPedidos | Soy la pantalla de operación de pedidos: no decido reglas de negocio, traslado cada acción del usuario a ServicioPedidos y muestro su resultado | Ninguna |
| Recibir la solicitud de cancelación, el motivo y la confirmación, y la clave del encargado cuando la cancelación requiere autorización | UsuarioMostrador, UsuarioEncargado, ServicioPedidos | | |
| Recibir las acciones de avance de estado (iniciar preparación, finalizar preparación y registrar entrega) | UsuarioCocina, UsuarioMostrador, ServicioPedidos | | |
| Mostrar los pedidos activos agrupados por estado y el detalle de un pedido seleccionado | ServicioPedidos, Pedido, ItemPedido | | |
| Informar al usuario los rechazos y errores devueltos por ServicioPedidos | ServicioPedidos | | |
|  |  |  |  |
