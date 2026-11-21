# Anexo – Aplicación de Patrón de Diseño Creacional – Builder

## Patrones de Diseño Creacionales y su relación con SOLID

Los patrones de diseño creacionales abstraen el proceso de instanciación de objetos. Su finalidad consiste en separar la lógica que crea y compone una entidad de la lógica que posteriormente la utiliza. En el catálogo GoF, la categoría creacional incluye Singleton, Factory Method, Abstract Factory, Builder y Prototype. Cada patrón resuelve una necesidad distinta de creación; en el presente sistema, la alternativa seleccionada se fundamenta en la complejidad progresiva de la construcción de un pedido.

La aplicación de estos patrones se relaciona con SOLID de la siguiente manera:

- **SRP:** asigna la responsabilidad de construir y validar el pedido a una clase especializada, separándola de la gestión general de pedidos.
- **OCP:** permite incorporar otro ConcreteBuilder sin modificar el Director ni el contrato que utiliza el cliente.
- **LSP:** toda implementación compatible con `IPedidoBuilder` puede sustituir a otra porque conserva el comportamiento esperado por `ServicioPedidos`.
- **ISP:** `IPedidoBuilder` concentra exclusivamente las operaciones necesarias para construir pedidos.
- **DIP:** `ServicioPedidos`, como clase de alto nivel, depende de `IPedidoBuilder` y recibe su implementación concreta por constructor.

## Propósito y Tipo del Patrón

**Propósito**

Construir el objeto complejo `Pedido` paso a paso durante el caso de uso CU1 – Tomar pedido, evitando que exista un pedido incompleto o que las reglas de construcción queden repartidas entre varias clases.

La construcción original del diagrama de clases final muestra una configuración progresiva:

- `ServicioPedidos` conserva `pedidoEnCurso` e `itemEnCurso`.
- `Pedido` se completa mediante operaciones dispersas y un método `registrarPedido(...)` con seis parámetros.
- `ItemPedido` contiene la cantidad, el precio unitario y el subtotal, y puede incorporar personalizaciones.
- `Producto` y `Combo` representan los elementos seleccionados para cada ítem.
- `Pago` representa el cobro asociado al pedido.

El Builder separa estos datos parciales del producto final y garantiza que el pedido solo pueda instanciarse cuando se verifica su integridad.

**Tipo**

Patrón de diseño **creacional – Builder**.

El patrón Builder corresponde a la categoría creacional porque controla cuándo y cómo se instancia un objeto. Además, permite que el mismo proceso de construcción incorpore datos en sucesivas etapas y produzca un objeto final válido. En este diseño:

- `ServicioPedidos` actúa como Director.
- `IPedidoBuilder` define el contrato del Builder.
- `PedidoBuilder` implementa el ConcreteBuilder.
- `Pedido` es el Product.

El patrón es preferible a los demás creacionales en este caso porque la clase de resultado siempre es `Pedido`, no se requieren familias de objetos y el objeto no se obtiene clonando una instancia existente. Su dificultad principal no consiste en seleccionar una clase concreta, sino en ensamblar un objeto complejo mediante pasos sucesivos y validarlo antes de crear la instancia final.

## Motivación

### Funcionamiento original y limitaciones

En el diagrama de clases final, el proceso de tomar un pedido combina coordinación, estado temporal y construcción del dominio dentro de `ServicioPedidos`. El servicio mantiene `pedidoEnCurso` e `itemEnCurso` y ofrece operaciones para ingresar la referencia de retiro, seleccionar productos o combos, cargar personalizaciones y asociar el pago.

Al finalizar, `Pedido` dispone de `registrarPedido(numeroPedido, fechaHora, referenciaRetiro, listaItems, totalPedido, pago)` para recibir los datos completos en una única operación. Esta forma de construir el pedido presenta las siguientes limitaciones:

1. **Estado intermedio dentro del Director:** `pedidoEnCurso` e `itemEnCurso` mantienen temporalmente el objeto a medio construir y hacen que el controlador también administre datos internos de creación.
2. **Responsabilidad excesiva:** `ServicioPedidos` concentra la coordinación de numerosos casos de uso junto con el armado del pedido.
3. **Construcción dependiente del orden:** si el objeto se modifica directamente desde varios métodos, la integridad final depende de que cada llamada se produzca en la secuencia correcta.
4. **Método con muchos parámetros:** `registrarPedido(...)` recibe seis datos relacionados y exige que el llamador haya construido previamente un estado consistente.
5. **Posibilidad de pedidos incompletos:** antes de confirmar la construcción podrían existir referencias, ítems o pagos ausentes o incoherentes.

### Clases involucradas originalmente

- `ServicioPedidos` orquesta el caso de uso y administraba el estado temporal.
- `Pedido` representaba el producto y ofrecía el método monolítico de registro.
- `ItemPedido` representaba una línea del pedido.
- `PersonalizacionItem` representaba una modificación con costo adicional.
- `Producto` y `Combo` eran los elementos que podían formar un ítem.
- `Pago` contenía el método y el monto asociado al pedido.
- `EstadoPedido` representaba el estado del pedido.

### Problemas de mantenibilidad

Mantener el armado dentro de `ServicioPedidos` aumenta su acoplamiento y reduce su cohesión. Si se incorpora un dato nuevo al pedido, se modificarían simultáneamente el estado temporal del servicio, el método de registro y el llamador. Las validaciones también podrían quedar dispersas e impedir que el sistema garantice que toda instancia de `Pedido` sea válida desde el momento de su creación.

La construcción también sería difícil de extender. Se podrían agregar nuevos pasos o datos sin modificar el proceso completo si no existiera una clase que encapsule de forma independiente cada representación de la secuencia de armado.

### Nuevas clases y reorganización de la arquitectura

Las únicas nuevas clases que introduce el patrón son:

- `IPedidoBuilder`: establece el contrato de construcción y permite que `ServicioPedidos` dependa de una abstracción.
- `PedidoBuilder`: implementa los pasos, reúne el estado temporal y ejecuta la validación antes de instanciar `Pedido`.

Las responsabilidades existentes cambian de esta manera:

- `ServicioPedidos` deja de conservar `pedidoEnCurso` e `itemEnCurso` y pasa a ser el Director.
- `Pedido` pasa a ser el Product y su constructor de paquete reemplaza a `registrarPedido(...)`.
- `PedidoBuilder` pasa a crear temporalmente los `ItemPedido` y a agrupar el `Pago` como parte de la construcción.
- `ItemPedido`, `PersonalizacionItem`, `Producto`, `Combo` y `EstadoPedido` conservan sus responsabilidades de dominio dentro del objeto construido.

La arquitectura se reorganiza de modo que el estado intermedio queda encapsulado en `PedidoBuilder`. El pedido no se instancia hasta que el Director solicita `construir()` y el ConcreteBuilder comprueba que el pedido esté completo.

## Estructura de Clases

El diagrama incluye únicamente las clases que participan en la aplicación del patrón al CU1.

![Diagrama de clases – Patrón Builder](../../diagramas/01-diagrama-clases/01-patron-creacional-builder.png)

[Ver diagrama en tamaño completo](../../diagramas/01-diagrama-clases/01-patron-creacional-builder.png)

## Justificación Técnica de la Estructura de Clases

### Descripción de cada clase

**`ServicioPedidos` – Director**

`ServicioPedidos` pertenece a la capa de aplicación y coordina el flujo de CU1. En el diagrama conserva la colección `pedidos`, una referencia al builder y el número del último pedido.

Su constructor recibe `IPedidoBuilder`, de modo que el Director depende de una abstracción y no de `PedidoBuilder`. También mantiene `generarNumeroPedido()` para asignar el identificador que se entrega al construir el pedido.

Inicia y controla las operaciones `iniciarNuevoPedido()`, `ingresarReferenciaRetiro()`, `seleccionarProducto()`, `seleccionarCombo()`, `ingresarPersonalizacion()`, `marcarPrioritario()`, `seleccionarMedioPago()`, `confirmarPedido()` y `descartarPedidoEnCurso()`. Las operaciones de construcción se delegan al builder; por ello, la clase ya no contiene `pedidoEnCurso` ni `itemEnCurso`.

**`IPedidoBuilder` – Builder**

`IPedidoBuilder` es la interfaz que establece el contrato de construcción. Define `reiniciar()`, los pasos para agregar los datos del pedido, `obtenerTotalParcial()` y `construir()`.

Es implementada por `PedidoBuilder`. Esta relación permite sustituir la implementación concreta y mantiene en `ServicioPedidos` una dependencia dirigida hacia una abstracción. Los métodos con prefijos `con*` y `agregar*` devuelven `IPedidoBuilder`, lo que permite encadenar el proceso de construcción.

**`PedidoBuilder` – ConcreteBuilder**

`PedidoBuilder` es la implementación concreta del Builder. Reúne la referencia de retiro, la lista de ítems, el ítem en curso, la prioridad y el pago. También contiene el método privado `validar()`, que concentra las reglas necesarias antes de crear el producto.

Implementa `IPedidoBuilder`, crea instancias de `Pedido` e `ItemPedido`, agrega temporalmente hasta cero o más ítems y registra un pago opcional durante la construcción. Esta clase es necesaria porque encapsula el estado mutable del armado y evita que dicho estado pertenezca a `ServicioPedidos`.

**`Pedido` – Product**

`Pedido` representa el objeto complejo obtenido al finalizar la construcción. Contiene `fechaHora`, `numeroPedido`, `referenciaRetiro`, `estado`, `prioritario` y `total`.

Su constructor tiene visibilidad de paquete (`~`) y recibe el identificador, la fecha, la referencia, la lista de ítems, la prioridad y el pago. Esta visibilidad impide que otras clases construyan pedidos fuera del Builder. El constructor reemplaza a `registrarPedido(...)` del diagrama de clases final.

El pedido compone entre uno y muchos `ItemPedido`, mantiene un único `Pago` original y utiliza `EstadoPedido` para representar su estado. Expone métodos para obtener el total, el estado y el detalle de ítems.

**`ItemPedido`**

`ItemPedido` representa una línea del pedido. Sus atributos son `cantidad`, `precioUnitario` y `subtotal`.

El builder lo crea al agregar un producto o un combo. Puede establecer un producto individual o un combo, agregar o quitar personalizaciones y recalcular su precio. Cada ítem contiene cero o más `PersonalizacionItem` y referencia cero o un `Producto` o `Combo`; de acuerdo con la composición de la línea, uno de los dos elementos debe estar asociado.

**`PersonalizacionItem`**

`PersonalizacionItem` representa una modificación aplicada a un ítem. Define `descripcion` y `costoAdicional`, y permite obtener ambos datos y calcular su costo.

Es necesaria porque una personalización forma parte de la construcción del `ItemPedido`, no del pedido completo. El ConcreteBuilder conserva el `itemEnCurso` para delegarle la incorporación de cada personalización.

**`Producto`**

`Producto` representa un elemento individual del menú. En el diagrama es una clase abstracta con `nombre` y `precio`, y proporciona operaciones para obtener ambos datos y cambiar el precio.

Es una entrada de construcción: `PedidoBuilder.agregarProducto(...)` utiliza el producto y la cantidad para crear un `ItemPedido`. El multiplicidad cero a muchos indica que un producto puede aparecer en distintos ítems.

**`Combo`**

`Combo` representa un conjunto comercializado como una sola opción del menú. Contiene `nombre` y `precio`, y ofrece operaciones para obtenerlos.

Al igual que `Producto`, no conoce el proceso de creación del pedido. El Builder recibe el combo seleccionado y delega en `ItemPedido.establecerComboYCantidad(...)` la creación de la línea correspondiente.

**`Pago`**

`Pago` representa el pago original asociado al pedido. Contiene `monto`, `metodoDePago` y `fechaHora`, y ofrece `registrarCobro(...)`.

`PedidoBuilder` recibe el pago mediante `conPago(pago)`. El pago es necesario para validar que su monto coincida con el total construido y, una vez creado el pedido, se mantiene como su pago original.

**`EstadoPedido`**

`EstadoPedido` es una enumeración que establece los estados posibles: `RECIBIDO`, `EN_PREPARACION`, `LISTO`, `ENTREGADO` y `CANCELADO`.

En el Builder, un pedido creado por `construir()` debe quedar en estado `RECIBIDO`. La enumeración permite que `Pedido` represente ese estado y los estados posteriores sin incorporar cadenas de texto en sus atributos.

### Flujo de creación del objeto final

El flujo comienza en `ServicioPedidos`, que actúa como Director:

1. **Inicio:** `iniciarNuevoPedido()` llama a `builder.reiniciar()` y prepara un nuevo proceso de construcción.
2. **Datos básicos:** `ingresarReferenciaRetiro()` delega en `IPedidoBuilder.conReferenciaRetiro(...)`.
3. **Armado de ítems:** `seleccionarProducto(...)` delega en `agregarProducto(...)`; `seleccionarCombo(...)` delega en `agregarCombo(...)`. `PedidoBuilder` crea un `ItemPedido` en cada operación.
4. **Personalización:** `ingresarPersonalizacion(...)` delega en `agregarPersonalizacion(...)`. `PedidoBuilder` aplica la personalización al `itemEnCurso`.
5. **Opciones finales:** `marcarPrioritario()` delega en el Builder y `seleccionarMedioPago(...)` proporciona el medio para obtener el pago asociado.
6. **Total y confirmación:** `confirmarPedido()` solicita a `ServicioPedidos` el número y la fecha que necesita `construir(numeroPedido, fechaHora)`.
7. **Validación e instanciación:** `PedidoBuilder.construir(...)` ejecuta `validar()`. Comprueba que la referencia de retiro no esté vacía, que exista al menos un `ItemPedido` y que el `Pago` esté registrado con un monto igual al total. Si todas las condiciones se cumplen, instancia `Pedido` en estado `RECIBIDO`.
8. **Entrega del resultado:** el método `construir()` devuelve el `Pedido` final a `ServicioPedidos`, que lo agrega a `pedidos`.

En resumen, **`ServicioPedidos` inicia y ordena el flujo, `IPedidoBuilder` define el contrato de construcción, `PedidoBuilder` reúne los datos parciales y valida las reglas, y `Pedido` es instanciado por `PedidoBuilder` como objeto final**.
