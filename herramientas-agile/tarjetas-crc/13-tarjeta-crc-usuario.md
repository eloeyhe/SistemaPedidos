|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |Usuario  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |UsuarioMostrador, UsuarioCocina, UsuarioEncargado | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Autentificar el acceso al sistema | | Defino la identidad y credenciales base del personal | nombre, usuario, passwordHash, activo |
| Indicar qué estados de pedido puedo solicitar según mi rol (cada subclase lo redefine) | Pedido, EstadoPedido | Mi rol determina qué transiciones del pedido puedo pedir | |
| Identificarme como responsable de cada cambio de estado, ajuste de pago y acción auditada | HistorialCambioPedido, HistorialPago, AuditoriaPedido | Todo cambio sobre un pedido queda asociado a quien lo hizo | usuario |
| Determinar qué estados de pedidos activos me corresponde ver (CU5) | ServicioPedidos, PantallaPedidos | Según mi rol veo los pedidos de Cocina o los de Mostrador | |
|  |  |  |  |
