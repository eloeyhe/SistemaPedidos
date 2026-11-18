|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |EstadoPedido (enumeración)  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Definir el conjunto cerrado de estados del ciclo de vida de un pedido: RECIBIDO, EN_PREPARACION, LISTO, ENTREGADO y CANCELADO | Pedido | Soy la única fuente de verdad de los estados posibles: un pedido no puede tener un estado que yo no defina | RECIBIDO, EN_PREPARACION, LISTO, ENTREGADO, CANCELADO |
| Servir de tipo para el estado vigente del pedido y para validar sus transiciones | Pedido, HistorialCambioPedido | | |
| Servir de criterio para agrupar y filtrar los pedidos activos que muestra cada pantalla | ServicioPedidos | | |
|  |  |  |  |
