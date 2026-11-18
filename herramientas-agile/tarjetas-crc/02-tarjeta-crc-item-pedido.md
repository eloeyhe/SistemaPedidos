|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | ItemPedido | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|Calcular mi subtotal y recalcular el precio si sufro modificaciones  | Pedido, Producto | Soy una línea concreta dentro de un pedido y calculo mi propio subtotal | cantidad, precioUnitario, subtotal |
| Agregar o quitar personalizaciones | PersonalizacionItem | Conozco los ajustes o ingredientes extra que se me aplican |  |
| Actualizar cantidad, precio unitario y personalizaciones cuando el pedido se modifica (CU2) | Pedido, PersonalizacionItem | Calculo mi subtotal con cantidad, precio base y costos adicionales | |
|  |  |  |  |
