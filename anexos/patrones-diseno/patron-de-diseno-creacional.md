# Anexo – Aplicación de Patrón de Diseño Creacional – Builder

## Patrones de Diseño Creacionales y su relación con SOLID

Los patrones de diseño creacionales abstraen el proceso de instanciación de objetos. Su objetivo es que el sistema no dependa de cómo se crean, componen y representan sus objetos, de modo que la lógica de creación quede separada de la lógica que los utiliza. El catálogo GoF reúne cinco patrones creacionales: **Singleton**, **Factory Method**, **Abstract Factory**, **Builder** y **Prototype**.

Estos patrones se relacionan directamente con los principios SOLID:

- **SRP (Responsabilidad Única):** la responsabilidad de crear un objeto, con sus validaciones y pasos de armado, se traslada a una clase dedicada. La clase que usa el objeto ya no necesita saber cómo construirlo.
- **OCP (Abierto/Cerrado):** al encapsular la creación, se pueden agregar nuevas formas de construir objetos (nuevas fábricas o nuevos builders) sin modificar el código cliente.
- **LSP (Sustitución de Liskov):** cuando la creación se expresa mediante una abstracción, cualquier implementación concreta puede reemplazar a otra sin alterar el comportamiento esperado por el cliente.
- **ISP (Segregación de Interfaces):** las interfaces de creación exponen solo las operaciones necesarias para construir el objeto, sin mezclar operaciones de uso.
- **DIP (Inversión de Dependencias):** el cliente depende de una abstracción de creación (interfaz) y no de las clases concretas que instancia. La implementación concreta se inyecta desde afuera.

---

## Propósito y Tipo del Patrón

**Propósito:**

En el caso de uso **CU1 – Tomar pedido**, el `Pedido` no se crea de una sola vez: el usuario de mostrador lo arma paso a paso. Primero ingresa la referencia de retiro, después agrega productos y combos, carga personalizaciones a cada ítem, eventualmente lo marca como prioritario y, por último, selecciona el medio de pago. Recién cuando el cobro se confirma, el pedido debe quedar registrado y enviarse a cocina.

En el diagrama de clases final, este armado está repartido entre dos clases:

- `ServicioPedidos` guarda el estado intermedio de la construcción (`pedidoEnCurso`, `itemEnCurso`) además de todas sus responsabilidades de gestión (búsquedas, modificaciones, cancelaciones, cambios de estado, monitor de pedidos activos).
- `Pedido` expone `registrarPedido(numeroPedido, fechaHora, referenciaRetiro, listaItems, totalPedido, pago)`, un método de **seis parámetros** que recibe todo junto y depende de que el llamador haya armado correctamente cada parte.

La instanciación tradicional (un constructor o un método con todos los datos) no alcanza porque el pedido se compone en varios pasos que ocurren en momentos distintos de la interacción, y porque un pedido solo es válido cuando cumple un conjunto de reglas (referencia de retiro, al menos un ítem, pago cuyo monto coincide con el total). Mientras tanto, existe un objeto `Pedido` "a medio hacer" dentro del servicio, que podría quedar en un estado inconsistente.

**Tipo:**

Se aplica el patrón creacional **Builder**. Su intención, según GoF, es *separar la construcción de un objeto complejo de su representación, de forma que el mismo proceso de construcción pueda crear diferentes representaciones*.

Encaja con el problema porque:

- El `Pedido` es un objeto complejo, compuesto por `ItemPedido`, `PersonalizacionItem` y `Pago`.
- Se construye **incrementalmente** y el resultado solo se obtiene al final (`construir()`).
- Permite **validar el objeto completo** antes de crearlo, de modo que nunca exista un `Pedido` inválido.

Se descartaron las demás alternativas creacionales:

- **Factory Method** y **Abstract Factory** resuelven la elección de *qué clase* instanciar (o qué familia de objetos), pero crean el objeto en un solo paso. Acá la clase es siempre `Pedido`; lo complejo es el armado.
- **Singleton** garantiza una única instancia y no aplica: hay muchos pedidos.
- **Prototype** crea objetos clonando uno existente y no hay un caso de uso que requiera duplicar pedidos.

---

## Motivación

**Cómo funcionaba originalmente el sistema y sus limitaciones**

En el diagrama de clases final, `ServicioPedidos` actúa como controlador de todos los casos de uso y, además, es quien va armando el pedido durante la toma:

1. `iniciarNuevoPedido()` crea un `Pedido` vacío y lo guarda en `pedidoEnCurso`.
2. `ingresarReferenciaRetiro()`, `seleccionarProducto()`, `seleccionarCombo()` e `ingresarPersonalizacion()` modifican ese objeto, usando también `itemEnCurso` para saber a qué ítem aplicar la personalización.
3. `seleccionarMedioPago()` procesa el cobro con `ProcesadorPago` y, si se aprueba, llama a `Pedido.registrarPedido(...)` con los seis parámetros.
4. `descartarPedidoEnCurso()` tiene que limpiar manualmente `pedidoEnCurso` e `itemEnCurso`.

**Clases involucradas y cómo interactuaban**

- `ServicioPedidos`: controla el flujo y mantiene el estado intermedio de construcción.
- `Pedido`: se crea vacío y se completa desde afuera; `registrarPedido(...)` recibe todos los datos juntos.
- `ItemPedido` y `PersonalizacionItem`: se crean y se asocian al pedido en curso.
- `Producto` y `Combo`: elementos que referencia cada ítem.
- `Pago`: se asocia al pedido una vez aprobado el cobro.

**Por qué este diseño generaba problemas**

- **Rigidez y baja cohesión (viola SRP):** `ServicioPedidos` ya tiene más de 25 operaciones. Sumarle el estado temporal de construcción (`pedidoEnCurso`, `itemEnCurso`) mezcla la gestión de pedidos con el armado de uno nuevo.
- **Objetos inconsistentes:** entre `iniciarNuevoPedido()` y la confirmación del pago existe un `Pedido` sin número, sin pago y quizás sin ítems. Nada impide que otro método lo use en ese estado.
- **Validación dispersa:** las reglas para que un pedido sea válido no están en un único lugar; dependen del orden en que el servicio llama a cada método.
- **Método con muchos parámetros:** `registrarPedido(...)` con seis parámetros es propenso a errores (orden de argumentos, total calculado afuera que puede no coincidir con los ítems).
- **Mantenibilidad:** cualquier dato nuevo en la toma del pedido (por ejemplo, un descuento o un canal de venta) obligaría a modificar `ServicioPedidos`, `Pedido.registrarPedido(...)` y todos sus llamadores.

**Nuevas clases que incorpora el patrón y su función**

- `IPedidoBuilder` (**Builder**): interfaz que define los pasos de construcción de un pedido y la operación final `construir()`.
- `PedidoBuilder` (**ConcreteBuilder**): implementa los pasos, acumula el estado intermedio (`referenciaRetiro`, `items`, `itemEnCurso`, `prioritario`, `pago`), calcula el total parcial y valida el pedido antes de crearlo.

Además, cambian los roles de dos clases existentes:

- `ServicioPedidos` pasa a cumplir el rol de **Director**: decide el orden de los pasos según la interacción del usuario, pero delega el armado en el builder.
- `Pedido` es el **Product**: su constructor pasa a tener visibilidad de paquete y reemplaza a `registrarPedido(...)`.

**Cómo el patrón reorganiza la arquitectura**

El estado intermedio sale de `ServicioPedidos` y queda encapsulado en `PedidoBuilder`. El `Pedido` ya no existe hasta que todos sus datos están completos y validados: se crea de una sola vez en `construir()`, directamente en estado `RECIBIDO`. `ServicioPedidos` recibe el builder por constructor, a través de la interfaz `IPedidoBuilder`, de la misma forma que en el diagrama de DIP se inyectan `IRepositorioPedidos`, `ICatalogoMenu` e `IPublicadorComandasCocina`.

---

## Estructura de Clases

El diagrama incluye únicamente las clases que participan en la implementación del patrón, para mantenerlo claro y centrado en la solución. El resto de las clases del sistema no se modifica.

![Diagrama de clases - Patrón Builder](../../diagramas/01-diagrama-clases/01-patron-creacional-builder.png)

[Ver diagrama en tamaño completo](../../diagramas/01-diagrama-clases/01-patron-creacional-builder.png)

---

## Justificación Técnica de la Estructura de Clases

### Descripción de cada clase incluida en el diagrama

**`IPedidoBuilder` – Builder (interfaz)**

- **Responsabilidad:** definir los pasos para construir un pedido: `conReferenciaRetiro`, `agregarProducto`, `agregarCombo`, `agregarPersonalizacion`, `marcarPrioritario`, `conPago`, `obtenerTotalParcial`, `reiniciar` y `construir`.
- **Relaciones:** es implementada por `PedidoBuilder` y utilizada por `ServicioPedidos`.
- **Por qué es necesaria:** permite que el Director dependa de una abstracción y no de la clase concreta (DIP). Si en el futuro se necesita otra forma de construir pedidos (por ejemplo, reconstruirlos desde persistencia o armar pedidos de prueba), se agrega un nuevo builder sin modificar `ServicioPedidos` (OCP). Los métodos de armado devuelven el propio builder, lo que permite encadenar llamadas (interfaz fluida).

**`PedidoBuilder` – ConcreteBuilder**

- **Responsabilidad:** acumular los datos del pedido en construcción, crear los `ItemPedido`, aplicar las personalizaciones al ítem en curso, calcular el total parcial y, en `construir()`, validar y crear el `Pedido`.
- **Relaciones:** implementa `IPedidoBuilder`; crea (`<<create>>`) instancias de `Pedido` e `ItemPedido`; agrega temporalmente los ítems y el `Pago` hasta que se construye el producto.
- **Por qué es necesaria:** es la clase que concentra en un único lugar la lógica de armado y las reglas de validez del pedido, que antes estaban repartidas entre `ServicioPedidos` y `Pedido`. El método privado `validar()` verifica que exista referencia de retiro, al menos un ítem y un pago registrado cuyo monto coincida con el total. Si alguna regla falla, lanza una excepción y no se crea ningún pedido.

**`ServicioPedidos` – Director**

- **Responsabilidad:** coordinar el CU1 según las acciones del usuario de mostrador y decidir en qué orden se invocan los pasos del builder. Conserva la generación del número de pedido (`generarNumeroPedido()`) y el registro de los pedidos creados.
- **Relaciones:** depende de `IPedidoBuilder` (inyectado por constructor) y gestiona la colección de `Pedido`.
- **Por qué es necesaria:** en el patrón, el Director conoce la secuencia de construcción pero no los detalles de armado. Con este cambio, `ServicioPedidos` deja de tener los atributos `pedidoEnCurso` e `itemEnCurso`.

**`Pedido` – Product**

- **Responsabilidad:** representar el pedido registrado con su número, fecha y hora, referencia de retiro, estado, prioridad y total.
- **Relaciones:** compone uno o más `ItemPedido` y tiene un `Pago` original.
- **Por qué es necesaria:** es el objeto complejo que produce el patrón. Su constructor con visibilidad de paquete (`~`) garantiza que solo el builder pueda instanciarlo, por lo que todo `Pedido` existente es válido.

**`ItemPedido` y `PersonalizacionItem` – Partes del producto**

- **Responsabilidad:** representar cada línea del pedido (producto o combo, cantidad, precio histórico y subtotal) y sus personalizaciones con costo adicional.
- **Relaciones:** `ItemPedido` referencia un `Producto` o un `Combo` y compone sus `PersonalizacionItem`.
- **Por qué son necesarias:** son las partes que el builder va ensamblando. `PedidoBuilder` crea cada `ItemPedido` en `agregarProducto()` o `agregarCombo()` y lo deja como `itemEnCurso` para que `agregarPersonalizacion()` sepa a qué ítem aplicarla.

**`Producto`, `Combo` y `Pago`**

- `Producto` y `Combo` son los elementos del menú que se reciben como parámetro de los pasos de armado; no cambian.
- `Pago` se registra cuando `ProcesadorPago` aprueba el cobro y se entrega al builder con `conPago()`; el builder verifica que su monto coincida con el total.

### Explicación del flujo de creación de objetos

1. **Inicio:** el usuario de mostrador inicia un pedido. `ServicioPedidos.iniciarNuevoPedido()` llama a `builder.reiniciar()`, que deja el builder vacío. Todavía no existe ningún `Pedido`.
2. **Armado incremental:** cada acción del usuario se traduce en un paso del builder:
   - `ingresarReferenciaRetiro()` → `builder.conReferenciaRetiro(referencia)`
   - `seleccionarProducto()` → `builder.agregarProducto(producto, cantidad)`, que crea un `ItemPedido` y lo deja como ítem en curso
   - `seleccionarCombo()` → `builder.agregarCombo(combo, cantidad)`
   - `ingresarPersonalizacion()` → `builder.agregarPersonalizacion(descripcion)`, aplicada al ítem en curso
   - `marcarPrioritario()` → `builder.marcarPrioritario()`
3. **Cobro:** en `seleccionarMedioPago()`, `ServicioPedidos` consulta `builder.obtenerTotalParcial()` y se lo pasa a `ProcesadorPago` para validar el medio de pago y procesar el cobro. Si el cobro se aprueba, crea el `Pago` y lo entrega con `builder.conPago(pago)`.
4. **Construcción final:** `ServicioPedidos` genera el número y llama a `builder.construir(numeroPedido, fechaHora)`. `PedidoBuilder` ejecuta `validar()`, **instancia el `Pedido`** en estado `RECIBIDO` con todos sus ítems y su pago, y se reinicia para el siguiente pedido.
5. **Registro:** `ServicioPedidos` agrega el pedido a su colección y lo envía a cocina mediante `PantallaCocina`, igual que en el diseño original.
6. **Descarte:** si el usuario cancela la toma antes de pagar, `descartarPedidoEnCurso()` solo llama a `builder.reiniciar()`. Como el pedido nunca llegó a crearse, no queda ningún objeto inconsistente que limpiar.

En resumen: **`ServicioPedidos` inicia el flujo y decide el orden de los pasos (Director), `IPedidoBuilder` define el contrato de construcción y `PedidoBuilder` es quien delega en `ItemPedido` el armado de cada línea e instancia el `Pedido` final.**
