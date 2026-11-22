|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |Combo  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Proveer mi nombre y mi precio propio cuando el cliente me elige (CU1) | ItemPedido | Soy una oferta empaquetada e independiente en el menú | nombre, precio |
| Mantener un precio propio, que no se calcula sumando el precio de productos | | No me modelo como una composición de productos: mi precio está definido en el menú | precio |
| Ser referenciado por un ítem en lugar de un producto individual | ItemPedido, ServicioPedidos | Cada ítem referencia un producto o un combo, nunca ambos | |
|  |  |  |  |
