# Documentación de Uso de IA - Especialista en Patrón de Diseño Creacional

Este documento registra el uso de IA en el Segundo Parcial para el rol de **Especialista en Patrón de Diseño Creacional**: la selección del patrón **Builder**, la generación del diagrama de clases y la redacción del anexo con **GitHub Copilot Agent Mode**.

**Entregables del rol:**

- `diagramas/01-diagrama-clases/01-patron-creacional-builder.puml`
- `diagramas/01-diagrama-clases/01-patron-creacional-builder.png`
- `anexos/patrones-diseno/patron-de-diseno-creacional.md`

**Contexto previo:** al momento de usar Copilot, la rama `feature/esp-patron-creacional-add-patron-builder` ya contenía una versión inicial del diagrama y del anexo. Copilot la leyó como contexto adicional durante las tres iteraciones.

---

## 1. Archivos de contexto referenciados

- `diagramas/01-diagrama-clases/06-clases-diagrama-final.puml` (diagrama de clases final; la consigna lo nombra `01-diagrama-clases-final.puml`)
- `diagramas/01-diagrama-clases/01-solid-01-srp.puml`
- `diagramas/01-diagrama-clases/01-solid-02-ocp.puml`
- `diagramas/01-diagrama-clases/01-solid-03-lsp.puml`
- `diagramas/01-diagrama-clases/01-solid-04-isp.puml`
- `diagramas/01-diagrama-clases/01-solid-05-dip.puml`

Por su cuenta, Copilot también leyó `01-patron-creacional-builder.puml`, `01-clases-tomar-pedido-01.puml`, `05-secuencia-tomar-pedido-registro-de-pedido-01.puml`, `anexos/principios-solid/05-dip.md` y `anexos/principios-solid/principios_solid.md`.

---

## 2. Iteración 1 – Identificación del problema y selección del patrón

### 2.1 Prompt utilizado

```text
Actuá como especialista en patrones de diseño GoF. Tomando como base el diagrama de clases final
#file:06-clases-diagrama-final.puml y los diagramas SOLID #file:01-solid-01-srp.puml
#file:01-solid-02-ocp.puml #file:01-solid-03-lsp.puml #file:01-solid-04-isp.puml
#file:01-solid-05-dip.puml del Sistema de Pedidos:

1. Identificá un problema concreto de creación de objetos en el sistema.
2. Evaluá cuál de los patrones creacionales (Singleton, Factory Method, Abstract Factory,
   Builder, Prototype) lo resuelve mejor y justificá por qué descartás los demás.
3. Indicá qué clases existentes cambian de rol y qué clases nuevas se incorporan.

No generes código todavía, solo el análisis. No modifiques archivos del repositorio.
```

### 2.2 Resultado de la IA

Copilot leyó los seis diagramas de contexto y el diagrama Builder existente, y respondió con un análisis en tres partes:

- **Problema:** `ServicioPedidos` concentra estado y lógica de construcción (`pedidoEnCurso`, `itemEnCurso`, identificadores, referencia, ítems, personalizaciones, prioridad y pago), y `Pedido` expone `registrarPedido(...)` con seis parámetros. Esto permite pedidos inválidos o incompletos y mezcla responsabilidades de aplicación y de construcción. El problema es visible en el CU1, donde el pedido se arma gradualmente y recién al final se puede validar.
- **Patrón seleccionado:** Builder, porque permite construir el pedido por pasos, impedir un `Pedido` incompleto mediante validaciones en `construir()`, dejar a `ServicioPedidos` como Director y respetar DIP si el servicio depende de `IPedidoBuilder`.
- **Descartes:**
  - Singleton: resuelve acceso único y no construcción compleja.
  - Factory Method: no simplifica una construcción de muchos pasos.
  - Abstract Factory: no hay familias de productos.
  - Prototype: clonar no garantiza la validez de un pedido nuevo.
- **Cambios de rol:**
  - `ServicioPedidos` pasa a ser Director.
  - `Pedido` es el Product, con construcción controlada.
  - `ItemPedido`, `Pago`, `Producto` y `Combo` conservan su responsabilidad y pasan a ser partes o entradas del Builder.
- **Clases nuevas:** `IPedidoBuilder` (Builder) y `PedidoBuilder` (ConcreteBuilder).
- **SOLID:** mejora SRP y DIP, y conserva OCP, LSP e ISP.

### 2.3 Revisión y ajustes realizados

- El análisis coincidió con el problema identificado en la versión previa de la rama, así que se aceptó sin cambios como justificación de la elección de Builder.
- Se verificó que los descartes de los otros cuatro patrones creacionales fueran coherentes con la sección "Tipo" del anexo.

### 2.4 Prompt adicional

Después del análisis se le pidió a Copilot que aplicara los cambios:

```text
Ahora midifica los archivos
```

**Resultado de la IA:** Copilot interpretó el pedido de forma amplia y, además del diagrama del patrón, modificó tres diagramas que no forman parte de los entregables del rol:

- `diagramas/01-diagrama-clases/06-clases-diagrama-final.puml`
- `diagramas/01-diagrama-clases/01-clases-tomar-pedido-01.puml`
- `diagramas/05-diagramas-secuencia/05-secuencia-tomar-pedido-registro-de-pedido-01.puml`

En ellos integró `IPedidoBuilder` y `PedidoBuilder` y eliminó las referencias a `pedidoEnCurso`, `itemEnCurso` y `registrarPedido(...)`. Informó que `git diff --check` finalizó sin errores.

**Ajuste realizado:** se revirtieron con `git restore` los cambios en esos tres archivos y el PNG del diagrama de secuencia que había quedado eliminado, por tres motivos:

1. La consigna indica tomar `06-clases-diagrama-final.puml` como **base** y documentar el patrón en un diagrama propio, mostrando solo las clases que participan de él.
2. El diagrama final y los diagramas del CU1 ya fueron aprobados en la Actividad Obligatoria N°4. Modificarlos excede el alcance del rol y no regeneraba sus PNG.
3. Los especialistas en patrones estructural y de comportamiento usan el mismo diagrama final como base. Cambiarlo en esta rama generaría inconsistencias y conflictos con sus PR.

Solo se conservó el resultado sobre `01-patron-creacional-builder.puml`, que se revisó en la iteración 2.

---

## 3. Iteración 2 – Generación del diagrama de clases

### 3.1 Prompt utilizado

```text
Con el análisis anterior, generá el contenido de
diagramas/01-diagrama-clases/01-patron-creacional-builder.puml aplicando el patrón Builder
a la construcción de Pedido en el CU1 (Tomar pedido). Requisitos:

- Incluí solo las clases que participan del patrón: ServicioPedidos (Director),
  IPedidoBuilder (Builder), PedidoBuilder (ConcreteBuilder), Pedido (Product), ItemPedido,
  PersonalizacionItem, Producto, Combo, Pago y EstadoPedido.
- Respetá los nombres, atributos, tipos y multiplicidades del diagrama de clases final.
- ServicioPedidos ya no debe tener pedidoEnCurso ni itemEnCurso; recibe IPedidoBuilder
  por constructor (DIP, igual que en 01-solid-05-dip.puml).
- El constructor de Pedido debe tener visibilidad de paquete y reemplazar a
  registrarPedido(...) de 6 parámetros.
- construir() debe validar referencia de retiro, al menos un ítem y pago con monto
  igual al total.
- Usá estereotipos <<Director>>, <<Builder>>, <<ConcreteBuilder>> y <<Product>>, las
  mismas skinparam del diagrama final y notas que expliquen la validación.
```

### 3.2 Resultado de la IA

Copilot leyó el diagrama final (líneas de `ServicioPedidos` a `Pedido` y las `skinparam` iniciales) y el diagrama Builder existente, y reemplazó `01-patron-creacional-builder.puml` por una versión limitada a las clases requeridas. Respecto de la versión previa:

- agregó `scale 0.8`;
- sumó a `ServicioPedidos` el atributo `ultimoNumeroPedido` y la operación `confirmarPedido(): Pedido`;
- completó `ItemPedido`, `PersonalizacionItem` y `Producto` con más operaciones tomadas del diagrama final;
- agregó la dependencia `Pago ..> MetodoPago`.

Informó que:

- Incluye Director, Builder, ConcreteBuilder, Product y las clases de dominio necesarias.
- `ServicioPedidos` recibe `IPedidoBuilder` por constructor y ya no tiene `pedidoEnCurso` ni `itemEnCurso`.
- `git diff --check` no reportó errores de formato.

Copilot buscó un ejecutable de PlantUML en el repositorio, pero no lo encontró, por lo que no verificó el renderizado del diagrama.

### 3.3 Revisión y ajustes realizados

- Se aceptó la versión de Copilot. Se verificó que mantuviera los nombres, tipos y multiplicidades del diagrama final, la inyección de `IPedidoBuilder` por constructor, el constructor de `Pedido` con visibilidad de paquete y las notas de validación. `confirmarPedido()` hace explícito el paso en el que el Director invoca `construir()`.
- **Ajuste manual:** Copilot agregó la dependencia `Pago ..> MetodoPago`, pero no declaró la enumeración. Al no estar declarada, PlantUML genera automáticamente una clase vacía `MetodoPago`. Se agregó `enum MetodoPago { EFECTIVO, TRANSFERENCIA }` dentro del paquete `Dominio`, igual que en el diagrama final.
- Como Copilot no pudo renderizar el diagrama, se abrió en la vista previa de PlantUML de VS Code y se volvió a exportar `01-patron-creacional-builder.png`, comprobando que la imagen se viera completa y coincidiera con el `.puml`.

---

## 4. Iteración 3 – Redacción del anexo

### 4.1 Prompt utilizado

```text
Redactá el contenido de anexos/patrones-diseno/patron-de-diseno-creacional.md siguiendo
exactamente esta estructura de la consigna:

# Anexo – Aplicación de Patrón de Diseño Creacional – Builder
## Patrones de Diseño Creacionales y su relación con SOLID
## Propósito y Tipo del Patrón (Propósito / Tipo)
## Motivación (funcionamiento original y limitaciones, clases involucradas, problemas de
   mantenibilidad, nuevas clases y su función, cómo el patrón reorganiza la arquitectura)
## Estructura de Clases (imagen incrustada de
   ../../diagramas/01-diagrama-clases/01-patron-creacional-builder.png y enlace
   "Ver diagrama en tamaño completo")
## Justificación Técnica de la Estructura de Clases (descripción de cada clase:
   responsabilidad, relaciones, por qué es necesaria; y flujo de creación indicando qué
   clase inicia el flujo, cuál delega y cuál instancia el objeto final)

Basate en #file:01-patron-creacional-builder.puml y #file:06-clases-diagrama-final.puml.
Usá estilo académico en español, sin inventar clases que no estén en el diagrama.
```

### 4.2 Resultado de la IA

Para mantener la terminología y el formato del proyecto, Copilot revisó:

- los anexos existentes (`05-dip.md` y `principios_solid.md`);
- el diagrama Builder;
- el diagrama final;
- la existencia del PNG.

Después reescribió `anexos/patrones-diseno/patron-de-diseno-creacional.md` con la estructura solicitada, la imagen incrustada y el enlace al diagrama completo. Además, buscó referencias a clases ajenas al patrón (`ProcesadorPago`, `Usuario`, `Pantalla*`, `IRepositorio*`, `ICatalogo*`, `IPublicador*`) para controlar que no se incluyeran clases fuera del diagrama. `git diff --check` no reportó errores.

### 4.3 Revisión y ajustes realizados

Se aceptó la versión de Copilot: respeta los apartados de la plantilla de la consigna, describe únicamente las clases del diagrama y desarrolla con más detalle que la versión previa la relación con SOLID, las limitaciones originales y cada clase del patrón. Al revisarla se hicieron las siguientes correcciones manuales:

- **Concordancia:** "El multiplicidad cero a muchos" → "La multiplicidad cero a muchos".
- **Párrafo contradictorio:** en "Problemas de mantenibilidad", Copilot afirmaba que se podrían agregar pasos sin modificar el proceso "si no existiera" una clase que encapsule el armado, lo cual invierte el argumento. Se reescribió para indicar que, sin esa clase, cada paso nuevo obliga a modificar el proceso completo.
- **Redacción:** "agrega temporalmente hasta cero o más ítems" → "agrega temporalmente cero o más ítems".
- **Flujo, paso 6:** decía que `confirmarPedido()` "solicita a `ServicioPedidos`" el número, cuando `confirmarPedido()` es un método del propio `ServicioPedidos`. Se corrigió para indicar que el Director genera el número con `generarNumeroPedido()` e invoca `construir(numeroPedido, fechaHora)`.
- **Flujo, paso 9 (agregado):** Copilot no describía `descartarPedidoEnCurso()`, aunque figura en el diagrama. Se agregó el paso de descarte, que solo reinicia el builder porque el `Pedido` nunca llegó a instanciarse.
- Se verificó que la ruta relativa de la imagen (`../../diagramas/01-diagrama-clases/01-patron-creacional-builder.png`) se visualizara correctamente en la vista previa de Markdown.

---

## 5. Conclusión

Copilot Agent Mode fue útil para validar la elección del patrón y para completar el diagrama y el anexo con más detalle a partir del diagrama de clases final y de los diagramas SOLID. Sus resultados se aceptaron, pero requirieron revisión manual: una enumeración sin declarar en el diagrama y errores de redacción y de lógica en el anexo.

En modo agente, Copilot tendió a ampliar el alcance de los cambios más allá de lo pedido: llegó a modificar el diagrama final, el diagrama del CU1 y su secuencia, que no correspondían a este rol. Como consecuencia, también se eliminó el PNG de esa secuencia. Por eso fue necesario revisar cada cambio, revertir con `git restore` los archivos fuera del alcance (incluido el PNG eliminado) y verificar manualmente el renderizado del diagrama, ya que Copilot no tenía PlantUML disponible en su entorno.
