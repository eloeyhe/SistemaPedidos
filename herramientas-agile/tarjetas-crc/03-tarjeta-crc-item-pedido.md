|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | ItemPedido | | |
| **Superclase:** | Ninguna | | |
| **Subclase:** | Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Guardar la cantidad, el precio unitario histórico y el subtotal del ítem solicitado | Pedido, Producto, PersonalizacionItem | El ítem representa una línea concreta de compra dentro del pedido | cantidad, precioUnitario, subtotal |
| Agregar y eliminar personalizaciones sin afectar la identidad del producto base | PersonalizacionItem, Producto | Un ítem debe encapsular tanto el producto como sus ajustes | personalizaciones, costoAdicional |
| Recalcular el subtotal cuando cambia la cantidad o se modifican personalizaciones | Pedido, PersonalizacionItem | La línea del pedido debe reflejar siempre el valor exacto de ese producto en ese momento | subtotal, precioBase, totalItem |
| Mantener la información necesaria para auditoría y ajustes posteriores | Pedido, Pago, AuditoriaPedido | El ítem conserva la historia de venta y su costo asociado | precioHistorico, fechaRegistro |
|  |  |  |  |
|  |  |  |  |
