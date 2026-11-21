# Documentación del uso de Inteligencia Artificial — Especialista en Patrón de Diseño Estructural 

--- 

**Proyecto:** SistemaPedidos  
**Rol:** Especialista en Patrón de Diseño Estructural   
**Patrón seleccionado:** Decorator (Decorador)

---

## 1. Archivos referenciados

`diagramas/01-diagrama-clases/06-clases-diagrama-final.puml`
`diagramas/01-diagrama-clases/01-solid-01-srp.puml`
`diagramas/01-diagrama-clases/01-solid-02-ocp.puml`
`diagramas/01-diagrama-clases/01-solid-03-lsp.puml`
`diagramas/01-diagrama-clases/01-solid-04-isp.puml`
`diagramas/01-diagrama-clases/01-solid-05-dip.puml`

--- 

## 2. Prompt utilizado

```text
Actuá como un arquitecto de software especializado en Diseño Orientado a Objetos, patrones GoF y principios SOLID.


Archivos de contexto obligatorios

Analizá los siguientes archivos del repositorio:

diagramas/01-diagrama-clases/06-clases-diagrama-final.puml
diagramas/01-diagrama-clases/01-solid-01-srp.puml
diagramas/01-diagrama-clases/01-solid-02-ocp.puml
diagramas/01-diagrama-clases/01-solid-03-lsp.puml
diagramas/01-diagrama-clases/01-solid-04-isp.puml
diagramas/01-diagrama-clases/01-solid-05-dip.puml

Objetivo

Identificar un problema real y concreto en la estructura actual de clases que pueda resolverse mediante un patrón de diseño estructural GoF.

Instrucciones

1. Analizá las clases, interfaces, dependencias, asociaciones y responsabilidades existentes.
2. Identificá problemas de acoplamiento, duplicación estructural, complejidad de interacción o dificultades de extensión.
3. Proponé hasta tres problemas concretos, indicando las clases involucradas y la evidencia encontrada en el diagrama.
4. Evaluá qué patrón estructural sería más adecuado para cada problema: Adapter, Bridge, Composite, Decorator, Facade, Flyweight o Proxy.
5. Explicá cómo cada solución mejoraría el diseño y qué principios SOLID favorecería.
6. Seleccioná la alternativa mejor fundamentada, priorizando una solución sencilla, coherente con el dominio y justificable académicamente.
7. Indicá qué clases existentes se conservarían, cuáles se modificarían y qué nuevas clases o interfaces serían necesarias.

Restricciones importantes

- No inventes problemas que no estén respaldados por los archivos.
- No propongas patrones innecesarios ni agregues complejidad sin justificación.
- Respetá el modelo de dominio existente y los principios SOLID.
- Si no encontrás evidencia suficiente para justificar un patrón, indicalo expresamente.
- No modifiques ni generes archivos todavía. Esta etapa es exclusivamente de análisis.


```

--- 

## 3. Resultado obtenido de la IA


* Lectura del modelo

Tomo `06-clases-diagrama-final.puml` como el modelo vigente. Los diagramas SOLID muestran alternativas y abstracciones propuestas, pero no necesariamente forman parte de ese modelo final.

| Problema u oportunidad | Clases involucradas y evidencia | Patrón candidato | Ventajas | Desventajas y límites |
|---|---|---|---|---|
| **Las personalizaciones participan en el precio del ítem, además de tener que conservarse como detalle.** Es una oportunidad para separar la composición del precio base de sus adicionales; el diagrama no prueba que hoy haya un bug o duplicación. | `ItemPedido` agrega, quita y actualiza `PersonalizacionItem`, expone `calcularSubtotal(..., costosAdicionales)` y `recalcularPrecio()`. También conserva `precioUnitario` histórico. `Producto` y `Combo` proveen el precio base. | **Decorator**. Una abstracción común para elementos de menú permitiría envolver un producto o combo con una o más personalizaciones que sumen su costo. | Expresa la acumulación de adicionales en la estructura; permite agregar tipos de personalización sin modificar las clases base. Favorece **OCP** y **LSP**; puede favorecer **SRP** si el cálculo de adicionales sale de `ItemPedido`. | Agrega objetos y envoltorios; puede ser excesivo si las personalizaciones son pocas y estables. Hay que preservar el detalle de cada personalización y el precio histórico del pedido, sin volver a tomar precios actuales del catálogo. |
| **El servicio de aplicación depende directamente de una clase concreta de integración con cocina.** Podría aislarse esa dependencia si la integración externa cambia o tiene una API incompatible. | `ServicioPedidos` depende de `PantallaCocina` y la invoca para enviar cabecera y detalle e informar confirmación. | **Adapter**, de forma condicional. Un adaptador podría convertir un puerto interno de publicación de comandas a la interfaz de la pantalla o sistema de cocina. | Aísla al servicio de una API externa y permite sustituir la integración sin cambiar el flujo de pedidos. | El diagrama no muestra una API externa incompatible ni variantes de cocina. Un puerto e inyección de dependencias podrían bastar; el Adapter sería especulativo sin esa evidencia. |

## Recomendación: Decorator

Es la alternativa mejor fundamentada por el dominio: una personalización agrega un costo al precio de un producto o combo, y puede haber varias sobre un mismo ítem. **La recomiendo como propuesta de refactorización, no como corrección de un defecto probado**: el diagrama muestra las responsabilidades y los datos involucrados, pero no la implementación del cálculo.

- **Conservar:** `Producto`, `Combo`, `PersonalizacionItem` y `ItemPedido`, preservando la información de personalización y el precio histórico.
- **Modificar:** introducir una abstracción común, por ejemplo `IElementoMenu`, implementada por `Producto` y `Combo`. El precio compuesto se obtendría al envolver ese elemento con personalizaciones; al registrar el ítem, `ItemPedido` seguiría guardando el precio unitario histórico.
- **Agregar:** un decorador que implemente `IElementoMenu`, contenga otro `IElementoMenu` y una `PersonalizacionItem`, y delegue el precio base sumando el adicional. Para múltiples personalizaciones, los envoltorios pueden encadenarse.

Los diagramas `01-solid-01-srp.puml`, `01-solid-03-lsp.puml` y `01-solid-05-dip.puml` ya exploran `IElementoMenu` como abstracción; sería necesario incorporarla al modelo final para esta propuesta. `01-solid-02-ocp.puml` y `01-solid-04-isp.puml` también fueron revisados, pero no aportan evidencia suficiente para justificar otro patrón estructural.

No recomendaría **Composite** para `Combo`: el modelo lo trata como elemento con nombre y precio propios, pero no muestra que contenga productos o componentes. Tampoco hay evidencia suficiente de variaciones de implementación para **Bridge**, de compartición de objetos para **Flyweight**, ni de acceso controlado o diferido para **Proxy**. **Facade** tampoco resolvería un problema demostrado: `ServicioPedidos` ya concentra la coordinación de los casos de uso.

**Archivo Puml:**

```Text

@startuml 07-solucion-decorator-personalizacion
title Sistema de Pedidos - Decorator para personalizaciones
top to bottom direction
skinparam classAttributeIconSize 0
skinparam linetype ortho
skinparam packageStyle rectangle
hide empty members

package "Dominio - elementos de menu" {
  interface IElementoMenu {
    + obtenerPrecio(): decimal
  }

  abstract class Producto {
    # nombre: String
    # precio: decimal
    + obtenerNombre(): String
    + obtenerPrecio(): decimal
    + cambiarPrecio(nuevoPrecio: decimal): void
  }

  class ProductoPreparado {
    - tiempoPreparacion: int
    + iniciarPreparacion(): void
  }

  class ProductoEnvasado {
    - fechaElaboracion: Date
    - fechaVencimiento: Date
  }

  class Combo {
    - nombre: String
    - precio: decimal
    + obtenerNombre(): String
    + obtenerPrecio(): decimal
  }

  class PersonalizacionItem {
    - descripcion: String
    - costoAdicional: decimal
    + obtenerDescripcion(): String
    + obtenerCostoAdicional(descripcion: String): decimal
    + calcularCosto(): decimal
  }

  class PrecioBaseHistorico <<snapshot>> {
    - precio: decimal
    + obtenerPrecio(): decimal
  }

  abstract class DecoradorElementoMenu {
    # elemento: IElementoMenu
    + obtenerPrecio(): decimal
  }

  class DecoradorPersonalizacion {
    - personalizacion: PersonalizacionItem
    + obtenerPrecio(): decimal
  }

  IElementoMenu <|.. Producto
  IElementoMenu <|.. Combo
  IElementoMenu <|.. PrecioBaseHistorico
  IElementoMenu <|.. DecoradorElementoMenu
  Producto <|-- ProductoPreparado
  Producto <|-- ProductoEnvasado
  DecoradorElementoMenu <|-- DecoradorPersonalizacion

  DecoradorElementoMenu o--> IElementoMenu : envuelve
  DecoradorPersonalizacion --> PersonalizacionItem : obtiene costo
}

package "Dominio - pedido" {
  class ItemPedido {
    - cantidad: int
    - precioUnitario: decimal
    - subtotal: decimal
    + establecerProductoYCantidad(producto: Producto, cantidad: int): void
    + establecerComboYCantidad(combo: Combo, cantidad: int): void
    - conservarPrecioHistorico(precioBase: decimal): void
    + agregarPersonalizacion(descripcion: String): PersonalizacionItem
    + quitarPersonalizacion(personalizacion: PersonalizacionItem): void
    + modificarCantidadYPrecio(cantidad: int, precioUnitario: decimal): void
    + actualizarPersonalizaciones(lista: List<PersonalizacionItem>): void
    - construirElementoConPersonalizaciones(): IElementoMenu
    + calcularSubtotal(cantidad: int, precioBase: decimal, costosAdicionales: List<decimal>): decimal
    + recalcularPrecio(): decimal
  }

  class Pedido {
    - total: decimal
    + agregarItem(producto: Producto, cantidad: int): ItemPedido
    + agregarItemCombo(combo: Combo, cantidad: int): ItemPedido
    - actualizarTotal(subtotal: decimal): void
    + obtenerTotal(): decimal
  }

  Pedido "1" *-- "1..*" ItemPedido : contiene
  ItemPedido "1" *-- "0..*" PersonalizacionItem : conserva el detalle
  ItemPedido "0..*" --> "1" IElementoMenu : elemento elegido
  ItemPedido ..> PrecioBaseHistorico : usa el precio congelado
  ItemPedido ..> DecoradorPersonalizacion : construye la cadena al recalcular
}

note right of DecoradorPersonalizacion
  obtenerPrecio() delega al elemento envuelto
  y suma PersonalizacionItem.calcularCosto().
  Cada decorador agrega una personalizacion;
  se pueden encadenar varios.
end note

note bottom of PrecioBaseHistorico
  Se crea desde el precio base capturado
  por ItemPedido al registrar el elemento.
  Los recálculos de personalizaciones usan
  ese valor y no consultan el precio actual
  del catalogo.
end note

note right of ItemPedido
  construirElementoConPersonalizaciones()
  parte de PrecioBaseHistorico(precioUnitario)
  y aplica un DecoradorPersonalizacion por
  cada personalizacion. El precio resultante
  se usa como precioBase en calcularSubtotal();
  costosAdicionales queda vacio para evitar
  contabilizar dos veces esos costos.
end note

note bottom of Combo
  El combo conserva su precio propio.
  El decorador solo agrega el costo de las
  personalizaciones; no suma componentes.
end note

note bottom of ItemPedido
  Se mantienen la cantidad, el precio historico,
  el subtotal y la composicion de personalizaciones.
  Pedido sigue acumulando subtotales.
  Las reglas de modificacion y pago no cambian.
end note

@enduml

```
--- 

### 4. Ajustes Criticos Realizados

**Sugerencias Aceptadas:**
- Se aceptó utilizar el patrón Decorator para calcular los costos adicionales de las personalizaciones.

- Se incorporó la interfaz IElementoMenu para trabajar con una estructura común.

- Se creó PrecioBaseHistorico para conservar el precio original del pedido.

- Se mantuvieron DecoradorElementoMenu y DecoradorPersonalizacion para sumar los costos adicionales.

- Se conservaron las clases y métodos principales del sistema original.


**Sugerencias Descartadas o Modificadas:**

- Se modificó DecoradorPersonalizacion para recibir directamente el costo adicional en lugar de un objeto PersonalizacionItem.

- Se cambió el método construirElementoConPersonalizaciones() por construirElementoDecorado(precioBase, costosAdicionales).

- Se corrigió el cálculo del subtotal para evitar sumar dos veces las personalizaciones.

- Se recuperaron las relaciones originales de ItemPedido con Producto y Combo.

- Se agregaron los atributos faltantes de Pedido y sus estados.

**Justificación:**

- Los cambios se realizaron para simplificar el diseño, respetar las clases originales y evitar errores en el cálculo de precios, manteniendo las ventajas del patrón Decorator.


--- 



## Code Review 1: PR [#290] - [ Coordinador y Devops]
* **Rama revisada:** `feature/coord-devops-update-docs`
* **Autor:** Santiago Medel Marquez

 ### 1. Prompt utilizado

```text
Actuá como un Senior Software Engineer realizando una code review profesional de la Pull Request del Coordinador y DevOps del Segundo Parcial de SistemaPedidos.

Utilizá como referencia anexos/introduccion.md, las issues vinculadas y los archivos modificados en la PR.

Revisá especialmente:

Cumplimiento de las responsabilidades del Coordinador y DevOps.

Organización, nomenclatura y ubicación de los archivos.

Coherencia entre la PR, las issues y los entregables.

Correcta aplicación de GitFlow y ramas.

Actualización de changelog.md, README e índices.

Funcionamiento de enlaces y rutas relativas.

Instrucciones:

Identificá únicamente problemas reales y verificables.

Enumerá los hallazgos (1, 2, 3…).

Cada hallazgo debe ser independiente.

Indicá archivos y líneas concretas.

No inventes problemas hipotéticos.

No incluyas sugerencias de tests.

No modifiques ningún archivo ni la PR.

Para cada hallazgo utilizá EXACTAMENTE esta estructura:

==================================================

HALLAZGO #1

Archivo:
[Ruta del archivo o sección de la PR]

Línea:
[Número de línea o ubicación exacta]

Tipo de problema:
(GitFlow | trazabilidad | documentación | integración | completitud | nomenclatura | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
[Describí el problema concreto y la evidencia encontrada. Explicá por qué incumple un requisito o afecta la calidad del proyecto.]

Sugerencia de mejora:
[Indicá el cambio específico necesario para resolverlo.]

==================================================

Repetí esta estructura para cada hallazgo, numerándolos consecutivamente.

## Decisión final

Terminá con una sola línea:

**Decisión sugerida: APROBAR / SOLICITAR CAMBIOS — [justificación breve].**

Si no encontrás problemas reales, indicá que no se identificaron hallazgos verificables y sugerí aprobar.

Priorizá los incumplimientos de la consigna y los problemas que puedan afectar la integración del trabajo del equipo.
```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`


### 3. Output obtenido de Copilot


==================================================

HALLAZGO #1

Archivo:  
`README.md`, `patrones_diseno.md`, `diagramas_de_clases.md` y `diagramasUML.md`

Línea:  
README: 42–44; índice de patrones: 7–9; índices de diagramas: 22–24 y 10–12.

Tipo de problema:  
documentación

Severidad:  
media

Explicación técnica:  
Los enlaces a los documentos de Builder, Decorator y el patrón de comportamiento, y a los diagramas de Builder y Decorator, apuntan a archivos que no están en la rama de la [PR #290](https://github.com/eloeyhe/SistemaPedidos/pull/290) ni en `develop`. Las PRs #289 y #298 que aportarían parte de esos archivos siguen abiertas; los nombres del patrón de comportamiento además permanecen como marcadores. Esto deja enlaces rotos e incumple los criterios de navegación de las issues [#277](https://github.com/eloeyhe/SistemaPedidos/issues/277), [#278](https://github.com/eloeyhe/SistemaPedidos/issues/278) y [#279](https://github.com/eloeyhe/SistemaPedidos/issues/279), aunque la sección «Issues vinculados» las marque con `Closes`.

Sugerencia de mejora:  
Coordinar la integración de los documentos y diagramas antes de publicar los enlaces definitivos, reemplazar los marcadores por los nombres reales y no marcar esas issues como resueltas hasta completar sus criterios.

==================================================

HALLAZGO #2

Archivo:  
`coordinador-devops.md`

Línea:  
94–97

Tipo de problema:  
completitud

Severidad:  
alta

Explicación técnica:  
La issue [#280](https://github.com/eloeyhe/SistemaPedidos/issues/280) exige revisar al menos cuatro PRs de integrantes y documentar las revisiones. La tabla registra revisiones de dos PRs distintas (#289 y #298), una tercera pendiente y, como cuarta fila, una segunda revisión de #289. No se alcanza el mínimo de cuatro PRs revisadas y documentadas.

Sugerencia de mejora:  
Completar y registrar las revisiones de PRs distintas que faltan —incluyendo el prompt, el resultado y los ajustes aplicados— y mantener la issue #280 sin cerrar hasta cumplir el mínimo.

==================================================

**Decisión sugerida: SOLICITAR CAMBIOS — hay enlaces requeridos sin destino y el registro no cumple el mínimo de revisiones.**

### 4. Ajustes críticos realizados

* **Sugerencias aceptadas:**  

* El Hallazgo 2 fue aceptado por completo.


* **Sugerencias descartadas o modificadas:** 

* El hallazgo 1, fue modificada su severidad, debido a que El coordinador reconoce en la descripción que su PR es un borrador y que esos elementos están pendientes. 


