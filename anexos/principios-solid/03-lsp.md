# Principio de Sustitución de Liskov (LSP)

## Propósito y Tipo del Principio SOLID

El Principio de Sustitución de Liskov (Liskov Substitution Principle) es un principio fundamental del diseño orientado a objetos que establece que las clases derivadas (subclases) deben poder sustituir a sus clases base (superclases) sin alterar el funcionamiento correcto del programa ni romper las expectativas del código cliente. Su propósito principal es garantizar que la jerarquía de herencia respete el "Diseño por Contrato", evitando comportamientos inesperados, excepciones en tiempo de ejecución o la necesidad de realizar comprobaciones de tipo explícitas (como `instanceof`) en el cliente.

## Motivación

En nuestro boceto inicial y tarjetas CRC, analizamos la jerarquía donde `ProductoPreparado` y `ProductoEnvasado` heredan de la superclase `Producto`. El problema operativo surge al analizar las responsabilidades en el área de cocina. Si la superclase `Producto` forzara un contrato con el método `iniciarPreparacion()`, la subclase `ProductoPreparado` podría cumplirlo sin problemas. Sin embargo, un `ProductoEnvasado` (que ya está listo para ser entregado) no puede ser "preparado". Si una lista genérica de `Producto` se procesa en la cocina, al intentar invocar `iniciarPreparacion()` sobre un producto envasado, este se vería forzado a arrojar una excepción (ej. `NotImplementedException`) o realizar una operación vacía, rompiendo el flujo del programa y violando el principio LSP.

## Explicación de Herencia

La herencia es un mecanismo de la Programación Orientada a Objetos que crea una relación jerárquica de tipo "es-un" entre una superclase y sus subclases. Para garantizar la sustituibilidad (LSP), no basta con que una clase hija herede la firma de los métodos; debe respetar estrictamente las precondiciones, postcondiciones e invariantes del comportamiento esperado por el cliente. Si un componente del sistema espera interactuar con la superclase `Producto`, debe poder recibir cualquier subclase derivada (`ProductoPreparado` o `ProductoEnvasado`) de manera transparente, sin tener que consultar qué tipo específico es (evitando el uso de condicionales o `instanceof`) y sin recibir respuestas inesperadas.

## Estructura de Clases

![Diagrama UML - LSP](C:\Users\Asus\Desktop\SistemaPedido\diagramas\01-diagrama-clases\01-solid-03-lsp.png)

## Justificación Técnica

El diagrama UML ilustra la corrección estructural para garantizar el cumplimiento del LSP:

1. **Jerarquía Comercial Limpia**: La superclase `Producto` mantiene estrictamente los atributos y métodos comerciales (como `precio` y `nombre`), lo que garantiza que, desde el punto de vista de la clase `Item_pedido`, cualquier `Producto` (ya sea preparado o envasado) pueda ser sustituido perfectamente para calcular el subtotal.
2. **Segregación del Comportamiento Operativo**: La capacidad de ser cocinado o elaborado se ha extraído a una interfaz específica llamada `Elaborable`, la cual solo es implementada por `ProductoPreparado`.
3. **Garantía para el Cliente**: La entidad `Cocina` u otros módulos operativos del sistema interactúan únicamente con objetos que implementan `Elaborable`. Esto asegura que los clientes del código operen sobre contratos que los objetos realmente pueden cumplir, protegiendo al sistema de excepciones en tiempo de ejecución y garantizando la sustituibilidad en la jerarquía .