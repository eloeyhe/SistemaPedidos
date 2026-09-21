|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | Producto | | |
| **Superclase:** | Ninguna | | |
| **Subclase:** | Combo, ProductoPreparado, ProductoEnvasado | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Definir la identidad y el precio base del artículo disponible para venta | ItemPedido, Pedido, Combo | El producto describe el catálogo de venta sin mezclar lógica de pedido | codigo, nombre, precioBase |
| Exponer la información mínima necesaria para construir un ítem de pedido | ItemPedido, Pedido | Cada producto debe ser un elemento reutilizable y consistente del catálogo | categoria, activo |
| Permitir la evolución del precio dentro del catálogo sin afectar el valor histórico del pedido | ItemPedido, AuditoriaPedido | La entidad de producto tiene responsabilidad de catálogo, no de operación de venta | precioActual, fechaActualizacion |
| Soportar variantes y combinaciones con lógica propia de precio | Combo, ItemPedido | El producto centraliza la semántica del artículo comercial vendido | tipoProducto, descripcion |
|  |  |  |  |
|  |  |  |  |
