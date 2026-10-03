# Principio Abierto/Cerrado (OCP)
## Propósito y Tipo del Principio SOLID

El Principio Abierto/Cerrado (Open/Closed Principle) es un principio de diseño fundamental que establece que las entidades de software (clases, módulos, funciones) deben estar **abiertas para su extensión, pero cerradas para su modificación**. Su propósito principal es permitir que el sistema incorpore nuevos comportamientos o características en el futuro sin riesgo de introducir errores o alterar el código existente que ya ha sido probado y está en funcionamiento.

## Motivación

En el diseño original del sistema, se detectó una fuerte dependencia de estructuras condicionales estáticas en dos clases clave del dominio:

1. **Clase `Pago`**: Gestiona el cobro basándose en el atributo `metodoDePago` de tipo `Metodo_pago` (un enum con valores como `efectivo` y `transferencia`). Los métodos `registrarPago()` y `calcularDiferencia()` utilizan bloques condicionales (`if/else` o `switch`) para decidir cómo procesar el cobro. El Requisito Funcional RF8 anticipa la incorporación futura de pagos con tarjeta y código QR, lo que obligaría a modificar el código fuente de `Pago` cada vez que se agregue un nuevo medio de pago.

Este casos representa una clara violación del principio OCP, ya que las variaciones futuras fuerzan la modificación directa de clases estables.

## Explicación de Herencia

La herencia es un mecanismo de la Programación Orientada a Objetos que permite a una clase (subclase) derivar atributos y comportamientos de otra (superclase), estableciendo una relación de tipo "es-un".

Para cumplir con OCP, la herencia (o la implementación de interfaces) se utiliza para crear **abstracciones**. En lugar de que la lógica principal del sistema dependa de clases concretas o enumeraciones llenas de condicionales, depende de superclases abstractas o interfaces estables. Cuando se requiere extender el sistema, simplemente se crea una nueva subclase que hereda de la abstracción e inyecta su propio comportamiento polimórfico, dejando la clase principal y el código existente completamente intactos.

## Estructura de Clases

![Diagrama UML - OCP](C:\Users\Asus\Desktop\SistemaPedido\diagramas\01-diagrama-clases\01-solid-02-ocp.png)

## Justificación Técnica

El diagrama UML asociado demuestra la aplicación del patrón **Strategy** para resolver las violaciones de OCP en el sistema:

1. **Estrategia de Pagos**: Se extrajo la lógica condicional de la clase `Pago` hacia una abstracción llamada `EstrategiaPago`. De ella derivan las subclases concretas `PagoEfectivo` y `PagoTransferencia`. La clase `Pago` delega la ejecución invocando el método polimórfico `procesarCobro()`. Si mañana se requiere incorporar `PagoQR` o `PagoTarjeta`, solo bastará con crear una nueva subclase sin alterar la clase `Pago`.
* **Estrategia de Descuentos**: Se aisló el cálculo del total de `Pedido` mediante la interfaz `EstrategiaDescuento`, permitiendo crear subclases como `DescuentoSocio` o `DescuentoPromocional` de manera independiente.

Técnicamente, el sistema queda **cerrado a modificaciones** en sus políticas de alto nivel y **abierto a la extensión** mediante el agregado de nuevas subclases concretas.

2. **Abstracción de Descuentos:** De manera similar, para la clase `Pedido`, se eliminaron los condicionales internos de cálculo delegando la responsabilidad a la interfaz `EstrategiaDescuento`. Las subclases `DescuentoSocio` y `DescuentoPromocional` implementan el método`aplicarDescuento(montoBase)`. Técnicamente, el sistema está **"cerrado a modificaciones"** porque para agregar un nuevo tipo de cliente o promoción mañana, bastará con crear una nueva subclase de estrategia de descuento, manteniendo la clase `Pedido` intacta y **"abierta a la extensión"**.