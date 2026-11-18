|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | Pedido | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Agregar, modificar y cancelar la orden | Cliente, ItemPedido | Soy la intención de compra del cliente y centralizo la orden completa | fecha, numeroPedido, referenciaRetiro |
| Cambiar mi estado operativo y gestionar la prioridad del pedido | EstadoPedido | Conozco mi estado dentro del ciclo de vida del negocio | estado, prioritario |
| Preparar la modificación de ítems y personalizaciones y actualizarme conservando mi identificador (CU2) | ItemPedido, PersonalizacionItem, ServicioPedidos | Solo acepto modificaciones mientras estoy en RECIBIDO | total |
| Conocer mi único pago original | Pago | Tengo un único pago original; cobros adicionales y devoluciones quedan como movimientos de ese pago | pago |
|  |  |  |  |
