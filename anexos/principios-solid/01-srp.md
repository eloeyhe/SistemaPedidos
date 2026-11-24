# Principio de Responsabilidad Única (SRP)

> **Alcance:** este anexo presenta una **variante conceptual no integrada** al diagrama de clases final (`06-clases-diagrama-final.puml`). Las abstracciones `IElementoMenu`, `PoliticaCicloPedido`, `ServicioRegistroPago` y `CalculadorAjustePago` no existen en ese modelo, que resuelve las mismas responsabilidades con `Producto`, `Combo`, `Pedido`, `ServicioPedidos`, `Pago` y `ProcesadorPago`. La sección [Correspondencia con el diagrama de clases final](#correspondencia-con-el-diagrama-de-clases-final) relaciona cada elemento con el modelo entregado.

## Propósito y Tipo de Principio SOLID
En el diseño inicial del Sistema de Pedidos, clases principales como **Pedido**, **Pago** e **ItemPedido** acumulaban **múltiples responsabilidades no relacionadas** dentro de una misma estructura. esto es un problema ya que hacía que el sistema fuera frágil y difícil de mantener (al modificar algo podías romper la estructura del pedido).

La aplicación del **Principio de Responsabilidad Única (SRP)** soluciona el problema de diseño porque **elimina la fragilidad del sistema al garantizar que cada clase tenga una y solo una razón para cambiar**, debido a que si se realiza algun cambio se modifica la clase especialista encargada, sin riesgo de romper los datos principales del pedido.


 ---
  ## Motivación 
  En el boceto inicial del **Sistema de Pedidos**, la clase **Pedido** actuaba como una "clase Dios" (*God Class*), concentrando múltiples responsabilidades que respondían a motivos de cambio totalmente independientes
  #### Responsabilidades mezcladas en `Pedido`:

- **Gestión de la Estructura de la Orden**: Almacenar y administrar la información propia de la orden (identificador, fecha, referencia de retiro y la colección de ítems).
- **Control del Ciclo de Vida y Transiciones de Estado**: El pedido puede cambiar de estado (de `RECIBIDO` a `EN_PREPARACION`, `LISTO`, `ENTREGADO` o `CANCELADO`) y aplicar reglas de como **bloquear modificaciones si el estado es posterior a** **RECIBIDO**.
- **Priorización del Pedido**: La operación para marcar un pedido como prioritario estaba mezclada con las demás operaciones de `Pedido`. Según el requisito RF7, la marca es manual; no se definen criterios automáticos de prioridad.

#### ¿Por qué esto es un problema?

- **Violación de la Cohesión**: La clase `Pedido` hacía "demasiadas cosas heterogéneas".
* **Alto Acoplamiento y Fragilidad**: Si el equipo comercial decidía cambiar alguna regla, ese cambio arriesgaba romper la estructura básica de almacenamiento de los ítems o la fecha.
* **Dificultad de Testeo**: Para probar unitariamente la regla de si un pedido se puede cancelar en estado `RECIBIDO`, era obligatorio crear un objeto `Pedido` completo con todos sus ítems, cliente y datos adicionales.


  #### ¿Cómo el Principio de Responsabilidad Única (SRP) Soluciona el Problema?

El **Principio de Responsabilidad Única (SRP)** establece formalmente que **una clase debe tener una y solo una razón para cambiar**.

SRP ayuda a separar las responsabilidades que tienen razones de cambio distintas, manteniendo el estado y las operaciones propias del pedido en `Pedido` y delegando la validación del ciclo de vida a una política especializada.


Para resolver el problema en el **Sistema de Pedidos**, se distribuyen las responsabilidades entre la entidad `Pedido`, sus entidades relacionadas y servicios especializados:

```
[ DISEÑO ORIGINAL (SIN SRP) ]
┌────────────────────────────────────────────────────────┐
│ Pedido                                                 │
├────────────────────────────────────────────────────────┤
│ - numeroPedido, fechaHora, referenciaRetiro, estado    │
├────────────────────────────────────────────────────────┤
│ + agregarItem(), cancelar()                            │  &lt;-- Estructura
│ + cambiarEstado(), validarModificacionEnRecibido()     │  &lt;-- Ciclo de Vida
│ + priorizar()                                          │  &lt;-- Marcado manual de prioridad
└────────────────────────────────────────────────────────┘

```

 **Refactorización aplicando SRP**:

```
[ DISEÑO REFACTORIZADO (CON SRP) ]

Pedido
  - mantiene sus datos, estado y marca manual de prioridad
  - calcula el total sumando los subtotales de sus ítems
  - delega la validación del ciclo de vida a PoliticaCicloPedido
  - contiene ItemPedido

ItemPedido
  - referencia un IElementoMenu (Producto o Combo)
  - conserva el precio unitario histórico capturado al agregar el ítem
  - calcula su subtotal con el precio histórico, las personalizaciones y la cantidad
  - contiene PersonalizacionItem

IElementoMenu
  - define obtenerPrecio()
  - Producto obtiene su precio base
  - Combo obtiene su precio propio, independiente de sus componentes

ServicioRegistroPago
  - registra cobros y ajustes en Pago e HistorialPago
CalculadorAjustePago
  - calcula la diferencia entre el total anterior y el nuevo

```



**Desglose de las Clases Refactorizadas**

 **Pedido** :
  * **Responsabilidad**: Representar la orden y mantener la coherencia de sus datos y estado, incluida la marca manual de prioridad; calcula el total a partir de sus ítems.
  * **Razón para Cambiar**: Cambios en los datos y las operaciones propias de una orden.
 **PoliticaCicloPedido** **(Componente/Servicio de Dominio)**:
  * **Responsabilidad Única**: Validar las reglas de negocio asociadas a las modificaciones, transiciones y cancelaciones del pedido. `Pedido` aplica el cambio de estado después de la validación.
  * **Única Razón para Cambiar**: Que cambien las reglas del negocio sobre el flujo de trabajo (por ejemplo, si se habilita modificar un pedido en estado `EN_PREPARACION` o si cambian los requisitos de cancelación).
 **Priorización manual**:
  * `Pedido` expone `marcarPrioritario()` para actualizar su marca de prioridad de forma controlada. No se incluye `PoliticaPrioridadPedido`, ya que el requisito establece una decisión manual y no define reglas automáticas que evaluar.

   ### Estructura de clases

   #### Diagrama UML SRP

  ![Diagrama UML SRP](../../diagramas/01-diagrama-clases/01-solid-01-srp.png) 


   ## Justificación Técnica  
   En el diagrama de clases refactorizado se observa una organización estructurada en **tres módulos principales del dominio** (**Módulo Pedido**, **Módulo Pago** y **Módulo Ítem Pedido**):

Las clases y sus relaciones distribuyen las responsabilidades entre componentes enfocados, evitando atribuir a un servicio una decisión de prioridad que el requisito define como manual.

En el **Módulo Pedido**, la entidad **Pedido** mantiene la estructura de la orden, sus ítems, su estado y su marca manual de prioridad. `Pedido` ofrece las operaciones que actualizan su propio estado; antes de las transiciones y cancelaciones, **PoliticaCicloPedido** valida si las reglas del dominio las permiten. No hay una política de prioridad, porque el requisito pide que el usuario marque manualmente el pedido como prioritario.

En el **Módulo Pago**, la clase **Pago** representa el cobro asociado al pedido. **ServicioRegistroPago** registra los cobros y ajustes, y **CalculadorAjustePago** calcula la diferencia entre el total anterior y el nuevo; el servicio registra el movimiento correspondiente en **HistorialPago**.
 
  En el **Módulo Ítem Pedido**, **ItemPedido** referencia un elemento del catálogo mediante **IElementoMenu**, implementada por **Producto** y **Combo**. Cada elemento proporciona su precio con `obtenerPrecio()`; **Combo** usa su precio propio, sin derivarlo de la suma de sus componentes, como requiere RF1. Al agregar un elemento, `ItemPedido` conserva ese valor como `precioUnitarioHistorico`, para que cambios posteriores en el catálogo no alteren el precio acordado. `ItemPedido.calcularSubtotal()` calcula el subtotal con ese precio histórico, la cantidad y los adicionales de sus personalizaciones. **Pedido.calcularTotal()** suma los subtotales, sin conocer las reglas internas de precio de productos o combos.

Desde un punto de vista técnico, las reglas del ciclo de vida y el cálculo de ajustes se mantienen en componentes especializados, mientras que cada entidad protege la coherencia de sus propios datos. Esta separación facilita probar las reglas y los cálculos de forma aislada y conserva el precio histórico acordado para los ítems.


## Correspondencia con el diagrama de clases final

La refactorización SRP es una **variante conceptual**: muestra cómo separar responsabilidades, pero el diagrama de clases final (`06-clases-diagrama-final.puml`) las distribuye entre las clases que usan los casos de uso y las secuencias. La siguiente matriz relaciona cada elemento de la variante con su equivalente en el modelo entregado.

| Elemento de la variante SRP | Equivalente en el diagrama de clases final | Observación |
|---|---|---|
| `Pedido` (datos, estado, prioridad e ítems) | `Pedido` | Mismas responsabilidades y atributos (`numeroPedido: int`, `fechaHora`, `referenciaRetiro`, `estado`, `prioritario`). |
| `Pedido.agregarItem(item)` | `Pedido.agregarItem(producto, cantidad)` y `Pedido.agregarItemCombo(combo, cantidad)` | El modelo final distingue producto y combo porque no tiene una interfaz común. |
| `Pedido.modificarItem(item)` | `Pedido.prepararModificacion(listaCambios)` y `Pedido.actualizarPedido(listaItems, totalActualizado)` | Operaciones del CU2. |
| `Pedido.cancelar()` | `Pedido.cancelarPedido(motivo)` | Operación del CU3. |
| `Pedido.cambiarEstado(nuevoEstado)` | `Pedido.cambiarEstado(nuevoEstado)` | Igual. |
| `Pedido.marcarPrioritario()` | `Pedido.priorizar()`, invocada por `ServicioPedidos.marcarPrioritario(numeroPedido)` | Marca manual (RF7). |
| `Pedido.calcularTotal()` | `Pedido.obtenerTotal()` y `Pedido.actualizarTotal(subtotal)` | |
| `ItemPedido.elemento: IElementoMenu` | Asociaciones `ItemPedido --> Producto` (0..1) e `ItemPedido --> Combo` (0..1) | Cada ítem referencia un producto o un combo, pero no ambos. |
| `ItemPedido.precioUnitarioHistorico` | `ItemPedido.precioUnitario` y `ItemPedido.conservarPrecioHistorico(precioBase)` | Mismo criterio de precio histórico. |
| `ItemPedido.calcularSubtotal()` | `ItemPedido.calcularSubtotal(cantidad, precioBase, costosAdicionales)` | |
| `IElementoMenu.obtenerPrecio()` | `Producto.obtenerPrecio()` y `Combo.obtenerPrecio()` | La interfaz solo existe en esta variante y en la variante del patrón Decorator. |
| `PoliticaCicloPedido` | `Pedido.validarTransicion(nuevoEstado, actor)` y `ServicioPedidos.solicitarCambioEstado(...)` | En el patrón State, las reglas pasan a los estados concretos. |
| `ServicioRegistroPago` | `ServicioPedidos.registrarAjustePago(...)`, `Pago.registrarCobro(...)`, `Pago.registrarCobroAdicional(...)`, `Pago.registrarDevolucion(...)` y `ProcesadorPago.procesarCobro(...)` | La validación del medio de pago es exclusiva de `ProcesadorPago`. |
| `CalculadorAjustePago.calcularDiferencia(...)` | `Pago.calcularDiferencia(totalAnterior, totalActualizado)` | |
| `Pago`, `HistorialPago`, `PersonalizacionItem` y los enums | Mismas clases y enums | `Pedido "1" -- "1" Pago` y `HistorialPago.registrarMovimiento(...)`, igual que en el modelo final. |

Integrar la variante al modelo final implicaría agregar las cuatro abstracciones y actualizar los casos de uso, las secuencias y las tarjetas CRC que hoy usan las operaciones de la columna central.
