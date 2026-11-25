|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |ProductoPreparado  | | |
| **Superclase:** |Producto | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Proveer el nombre y el precio base heredados de Producto cuando se me agrega a un pedido | ItemPedido | Soy un producto que requiere elaboración manual antes de entregarse | nombre, precio (heredadas) |
| Conocer el tiempo que demora mi elaboración en cocina | | Mi tiempo de preparación me distingue de los productos envasados | tiempoPreparacion |
| Iniciar mi preparación cuando Cocina comienza a elaborar el pedido (CU4) | UsuarioCocina, Pedido | Solo me preparo cuando el pedido que me contiene pasa a EN_PREPARACION | tiempoPreparacion |
|  |  |  |  |
