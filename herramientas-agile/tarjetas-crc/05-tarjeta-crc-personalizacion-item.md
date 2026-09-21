|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | PersonalizacionItem | | |
| **Superclase:** | Ninguna | | |
| **Subclase:** | Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Registrar una modificación concreta sobre un ítem del pedido, como quitar o agregar ingredientes | ItemPedido, Producto | La personalización representa un ajuste del producto y no un producto independiente | descripcion, tipoModificacion |
| Calcular su costo adicional y reflejarlo en el subtotal del ítem | ItemPedido, Pago | Cada ajuste debe influir explícitamente en el valor final de la compra | costoAdicional, esOpcional |
| Validar que la modificación sea compatible con el producto asociado | Producto, ItemPedido | La personalización debe ser una regla local del ítem, evitando acoplamiento con todo el sistema | reglaNegocio, activo |
| Mantener la trazabilidad del ajuste aplicado al pedido | ItemPedido, AuditoriaPedido | Toda modificación debe poder ser auditada y revertida si corresponde | fechaAplicacion, usuarioResponsable |
|  |  |  |  |
|  |  |  |  |
