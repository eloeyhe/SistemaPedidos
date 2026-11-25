# Principio de Sustitución de Liskov (LSP)

> **Alcance:** este anexo presenta una **variante conceptual no integrada** al diagrama de clases final (`06-clases-diagrama-final.puml`). `IElementoMenu`, `IElaborable` y `Cocina` no existen en ese modelo; las clases `Producto`, `ProductoPreparado`, `ProductoEnvasado` y `Combo` conservan sus nombres, atributos y operaciones finales. La sección [Correspondencia con el diagrama de clases final](#correspondencia-con-el-diagrama-de-clases-final) relaciona cada elemento con el modelo entregado.

## Propósito y Tipo del Principio SOLID
El Principio de Sustitución de Liskov (Liskov Substitution Principle) es un principio fundamental del diseño orientado a objetos que establece que las clases derivadas (subclases) deben poder sustituir a sus clases base (superclases) sin alterar el funcionamiento correcto del programa ni romper las expectativas del código cliente. Su propósito principal es garantizar que la jerarquía de herencia respete el "Diseño por Contrato", evitando comportamientos inesperados, excepciones en tiempo de ejecución o la necesidad de realizar comprobaciones de tipo explícitas (como `instanceof`) en el cliente.

## Motivación
En nuestro boceto inicial y tarjetas CRC, analizamos la jerarquía donde `ProductoPreparado` y `ProductoEnvasado` heredan de la superclase `Producto`. El problema operativo surge al analizar las responsabilidades en el área de cocina. Si la superclase `Producto` forzara un contrato con el método `iniciarPreparacion()`, la subclase `ProductoPreparado` podría cumplirlo sin problemas. Sin embargo, un `ProductoEnvasado` (que ya está listo para ser entregado) no puede ser "preparado". Si una lista genérica de objetos `Producto` se procesa en la cocina, al intentar invocar `iniciarPreparacion()` sobre un producto envasado, este se vería forzado a arrojar una excepción (ej. `NotImplementedException`) o realizar una operación vacía, rompiendo el flujo del programa y violando el principio LSP.

## Explicación de Herencia
La herencia es un mecanismo de la Programación Orientada a Objetos que crea una relación jerárquica de tipo "es-un" entre una superclase y sus subclases. Para garantizar la sustituibilidad (LSP), no basta con que una clase hija herede la firma de los métodos; debe respetar estrictamente las precondiciones, postcondiciones e invariantes del comportamiento esperado por el cliente. Si un componente del sistema espera interactuar con la superclase `Producto`, debe poder recibir cualquier subclase derivada (`ProductoPreparado` o `ProductoEnvasado`) de manera transparente, sin tener que consultar qué tipo específico es (evitando el uso de condicionales o `instanceof`) y sin recibir respuestas inesperadas.

## Estructura de Clases

![Diagrama UML - LSP](../../diagramas/01-diagrama-clases/01-solid-03-lsp.png)

## Justificación Técnica
El diagrama UML ilustra la corrección estructural para separar claramente los contratos y garantizar el cumplimiento del LSP:

1. **Contrato Comercial (Precio)**: Entidades como `Producto` (o agrupaciones como `Combo`) se rigen por un contrato estrictamente comercial a través de interfaces como `IElementoMenu` (la cual define `obtenerPrecio()`). Esto garantiza que, desde el punto de vista de la clase `ItemPedido` o la Caja, cualquier elemento de compra pueda ser sustituido perfectamente para calcular subtotales, sin importar su naturaleza.

2. **Contrato Operativo (Preparación)**: Se establece una frontera conceptual clara respecto a la cocina. La capacidad de ser cocinado se ha extraído a una interfaz específica llamada `IElaborable` (que define `iniciarPreparacion()`). Esta interfaz **solo** es implementada por `ProductoPreparado`. Un `Combo` o un `ProductoEnvasado` pueden ser elementos del menú para ser cobrados, pero no son elementos preparables.

3. **Garantía para el Cliente**: La entidad `Cocina` u otros módulos operativos del sistema interactúan únicamente con objetos que implementan `IElaborable`, no con `IElementoMenu`. Esto asegura que los clientes del código operen sobre contratos que los objetos realmente pueden cumplir, protegiendo al sistema de excepciones en tiempo de ejecución y garantizando la sustituibilidad exacta en cada contexto.

## Validación mediante Tests
Para asegurar la correcta sustituibilidad de las clases en el entorno operativo y evitar fallos en producción, este diseño facilita la creación de **pruebas de integración** enfocadas en el módulo `Cocina`. Estos tests deben validar que las dependencias de la `Cocina` aceptan exclusivamente colecciones de objetos `IElaborable`. Esto blinda el sistema y asegura a nivel de pruebas automatizadas que nunca se intentará encolar o preparar un `ProductoEnvasado` o un `Combo` genérico, erradicando definitivamente el riesgo de disparar una `NotImplementedException` por un mal uso de la jerarquía.


## Correspondencia con el diagrama de clases final

La solución LSP es una **variante conceptual**. El diagrama de clases final (`06-clases-diagrama-final.puml`) ya cumple el principio sin las interfaces: `iniciarPreparacion()` está declarada solo en `ProductoPreparado` y no en `Producto`, por lo que ningún `ProductoEnvasado` queda obligado a implementarla.

| Elemento de la variante LSP | Equivalente en el diagrama de clases final | Observación |
|---|---|---|
| `IElementoMenu.obtenerPrecio(): decimal` | `Producto.obtenerPrecio()` y `Combo.obtenerPrecio()` | Sin interfaz común. La interfaz solo existe en esta variante y en la del patrón Decorator. |
| `Producto` (`nombre`, `precio`, `obtenerNombre()`, `obtenerPrecio()`, `cambiarPrecio(...)`) | `Producto` | Igual. |
| `ProductoEnvasado` (`fechaElaboracion`, `fechaVencimiento`) | `ProductoEnvasado` | Igual. |
| `ProductoPreparado` + `IElaborable.iniciarPreparacion()` | `ProductoPreparado.iniciarPreparacion()` | La operación existe, pero sin interfaz. |
| `Combo` (`nombre`, `precio`, `obtenerNombre()`, `obtenerPrecio()`) | `Combo` | Igual; el precio es propio del combo (RF1). |
| `ItemPedido.elemento: IElementoMenu` y `calcularSubtotal()` | Asociaciones `ItemPedido --> Producto` / `ItemPedido --> Combo` y `ItemPedido.calcularSubtotal(cantidad, precioBase, costosAdicionales)` | |
| `Cocina.prepararItem(item: IElaborable)` | `UsuarioCocina.iniciarPreparacion(pedido)` y `PantallaPedidos.iniciarPreparacion(numeroPedido)` (CU4) | El modelo final no tiene una clase `Cocina`: la preparación se registra por pedido. |
