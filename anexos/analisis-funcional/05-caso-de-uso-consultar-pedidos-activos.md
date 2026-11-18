# Caso de Uso N° 5 - Consultar Pedidos Activos

## 1. Descripción y Trazabilidad con Requisitos Funcionales

**Actor/es:** Cocina y Usuario de Mostrador

**Objetivo:** Permite consultar los pedidos activos y visualizarlos según su estado actual, facilitando el seguimiento y la preparación de los pedidos.

**Flujo principal:**

1. El actor accede al monitor de pedidos activos
2. El sistema obtiene los pedidos que se encuentran activos
3. El sistema agrupa o filtra los pedidos según su estado
4. El sistema adapta la visualización según el actor que realiza la consulta
5. El sistema identifica los pedidos marcados como urgentes
6. El sistema muestra primero los pedidos urgentes dentro del mismo estado
7. El actor consulta la información de los pedidos activos
8. El sistema mantiene un único estado actual para cada pedido mostrado

**Flujos alternativos:**

- **A1: No hay pedidos activos.** Si no hay pedidos en estado `RECIBIDO`, `EN_PREPARACION` o `LISTO`, el sistema muestra la vista vacía e informa que no hay pedidos activos.

**Regla de estados visibles por actor:** Cocina visualiza los pedidos `RECIBIDO` y `EN_PREPARACION`; el Usuario de Mostrador visualiza únicamente los pedidos `LISTO` para su entrega. Esta regla se aplica igual en el diagrama de casos de uso, la actividad, la secuencia, el pseudocódigo y los diagramas de clases mediante `ServicioPedidos.seleccionarEstadosVisibles(actor: Usuario): List<EstadoPedido>`.
- **A2: Falla de conexión o actualización.** Si se interrumpe la comunicación durante la consulta, el sistema advierte que los datos mostrados pueden estar desactualizados.

**Requisitos funcionales que satisface:**

| ID | Requisito Funcional (texto exacto de introduccion.md) | Cómo lo satisface este caso de uso |
| --- | --- | --- |
| RF3 | El sistema debe permitir al personal visualizar sincronizadamente los pedidos activos y controlar su ciclo de vida mediante un único estado secuencial y restringido: *Recibido*, *En preparación*, *Listo*, *Entregado* y *Cancelado*, garantizando una única fuente de verdad para evitar duplicaciones. | Obtiene y agrupa los pedidos por su estado vigente, presenta los estados activos y conserva un único estado actual para cada pedido mostrado. |
| RF7 | El sistema debe permitir marcar manualmente un pedido como prioritario (urgente) desde el mostrador al momento de tomarlo, visualizándose de forma destacada en la pantalla de cocina para que el personal altere el orden de preparación según su criterio. | Lee la prioridad ya registrada en cada pedido y ordena primero los prioritarios dentro de cada estado para destacarlos en la consulta. Este caso de uso los visualiza; no modifica la marca de prioridad. |

---

## 2. Diagrama de Casos de Uso

![Diagrama de Casos de Uso - Consultar Pedidos Activos](../../diagramas/02-casos-de-uso/02-caso-uso-consultar-pedidos-activos-05.png)

**Actores y relaciones:**

- **Cocina** → consulta las comandas en estado `RECIBIDO` y `EN_PREPARACION`.
- **Usuario de Mostrador** → consulta el estado de las entregas en `LISTO`, según el caso de uso A2.
- **Include/Extend:** "Consultar Pedidos Activos" incluye obtener el listado y ordenarlo porque son tareas comunes de la consulta. Las vistas de Cocina y Mostrador extienden el caso base para representar consultas por actor. "Destacar pedidos prioritarios" extiende el ordenamiento cuando hay pedidos prioritarios. "Informar falta de conexión o datos desactualizados" extiende la consulta cuando ocurre una falla.

La vista `LISTO` del Usuario de Mostrador es la misma en la descripción del CU05 de `introduccion.md`, el escenario, la actividad, la secuencia y el pseudocódigo.

---

## 3. Diagrama de Actividades

![Diagrama de Actividades - Consultar Pedidos Activos](../../diagramas/04-diagramas-actividades/04-actividad-consultar-pedidos-activos-05.png)

**Swimlanes:**

- **Sistema de Pedidos:** comprueba disponibilidad, obtiene y agrupa pedidos activos, identifica urgencias, ordena y adapta la vista.
- **Cocina:** accede al monitor y consulta comandas `RECIBIDO` y `EN_PREPARACION`.
- **Usuario de Mostrador:** accede al monitor y consulta pedidos `LISTO` para entrega.

**Decisiones clave del flujo:**

- **¿Hay conexión?** Si no, el sistema advierte que la información puede estar desactualizada.
- **¿Hay pedidos activos?** Si no, muestra la vista vacía.
- **¿Qué actor consulta?** Determina qué estados se muestran en la actividad: Cocina ve `RECIBIDO` y `EN_PREPARACION`; Mostrador ve `LISTO`.
- Cuando hay pedidos, el sistema destaca los urgentes ordenándolos primero dentro de su estado, sin alterar el estado del pedido.

---

## 4. Diagrama de Secuencia

![Diagrama de Secuencia - Consultar Pedidos Activos](../../diagramas/05-diagramas-secuencia/05-secuencia-consultar-pedidos-activos-prioridad-urgentes-05.png)

**Participantes:** `Usuario de Cocina` (`usuarioCocina`) y `Usuario de Mostrador` (`usuarioMostrador`) son actores; `PantallaPedidos:pantalla` y `ServicioPedidos:servicio` son objetos nombrados con la notación `Clase:objeto`; `:Pedido` (`pedido`) es una instancia anónima de la clase `Pedido`.

**Mensajes clave:**

- `accederMonitorPedidosActivos()` → el actor solicita abrir el monitor; la pantalla envía `consultarPedidosActivos(actorSolicitante)` al servicio.
- `comprobarDisponibilidadDatos()` → comprueba si la consulta cuenta con datos disponibles; el resultado se devuelve como `resultadoDisponibilidad`.
- `obtenerPedidosPorEstados(estadosActivos)` → `ServicioPedidos`, que conserva la colección de pedidos, filtra los estados activos (`RECIBIDO`, `EN_PREPARACION`, `LISTO`) y devuelve `listaPedidosActivos`.
- `obtenerEstado()` y `esPrioritario()` → consultan, para cada pedido, el estado y la marca de urgencia.
- `agruparPedidosPorEstado(listaPedidosActivos)`, `ordenarGrupoPorPrioridad(urgentesPrimero)` y `seleccionarEstadosVisibles(actorSolicitante)` → agrupan, priorizan y adaptan los estados consultados al actor: para Cocina devuelve `RECIBIDO` y `EN_PREPARACION`; para Mostrador, `LISTO`.
- `retornarPedidosAgrupadosYOrdenados(vistaAdaptada)` → devuelve a la pantalla el resultado ordenado; la pantalla presenta la vista al actor.
- `consultarDetallePedido(numeroPedido)` y `obtenerDetallePedido(numeroPedido)` → solicitan el detalle; `Pedido.obtenerDetalle()` devuelve `detallePedido` y el servicio devuelve ese resultado a la pantalla.
- `mostrarVistaVacia("No hay pedidos activos")` y `advertirDatosDesactualizados()` → representan las respuestas de la pantalla en los flujos alternativos correspondientes.

**Objetos temporales destruidos:** no se representan objetos temporales ni mensajes de destrucción en esta secuencia. `:Pedido` representa un pedido consultado, no una instancia creada para la consulta.

**Nota de trazabilidad:** `retornarPedidosAgrupadosYOrdenados(...)`, `retornarDetalle(...)` y los mensajes `mostrar...` se dibujan como respuestas (`-->`) en la secuencia. Por eso se documentan como resultados presentados/devueltos, no como métodos invocados en el diagrama de clases. Los estados se escriben como valores de `EstadoPedido` (sin comillas) en la secuencia, el pseudocódigo y las firmas del diagrama de clases.

---

## 5. Diagrama de Clases del Caso de Uso

![Diagrama de Clases - Consultar Pedidos Activos](../../diagramas/01-diagrama-clases/05-clases-consultar-pedidos-activos-05.png)

Fuente PlantUML: [05-clases-consultar-pedidos-activos-05.puml](../../diagramas/01-diagrama-clases/05-clases-consultar-pedidos-activos-05.puml)

**Clases involucradas:**

| Clase | Responsabilidad (según tarjeta CRC o artefacto de origen) | Tarjeta CRC |
| --- | --- | --- |
| Usuario | Autentificar el acceso al sistema | [13-tarjeta-crc-usuario.md](../../herramientas-agile/tarjetas-crc/13-tarjeta-crc-usuario.md) |
| UsuarioCocina | Comenzar preparaciones y marcar pedidos como listos | [15-tarjeta-crc-usuario-cocina.md](../../herramientas-agile/tarjetas-crc/15-tarjeta-crc-usuario-cocina.md) |
| UsuarioMostrador | Registrar y modificar pedidos, y marcarlos como entregados | [14-tarjeta-crc-usuario-mostrador.md](../../herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md) |
| PantallaPedidos | Frontera participante de A3: recibe el acceso al monitor y la consulta de detalle, y presenta sus resultados | [Pantalla Pedidos](../../herramientas-agile/tarjetas-crc/20-tarjeta-crc-pantalla-pedidos.md) |
| ServicioPedidos | Control participante de A3: obtiene, agrupa, ordena y adapta la consulta y solicita el detalle | [Servicio Pedidos](../../herramientas-agile/tarjetas-crc/17-tarjeta-crc-servicio-pedidos.md) |
| Pedido | Agregar, modificar y cancelar la orden. Cambiar mi estado operativo y gestionar la prioridad del pedido | [01-tarjeta-crc-pedido.md](../../herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md) |
| ItemPedido | Calcular mi subtotal y recalcular el precio si sufro modificaciones. Agregar o quitar personalizaciones | [02-tarjeta-crc-item-pedido.md](../../herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md) |
| EstadoPedido | Definir el conjunto cerrado de estados del ciclo de vida del pedido | [21-tarjeta-crc-estado-pedido.md](../../herramientas-agile/tarjetas-crc/21-tarjeta-crc-estado-pedido.md) |

**Relaciones UML:**

| Relación | Clases | Justificación |
| --- | --- | --- |
| Herencia | Usuario → UsuarioCocina y UsuarioMostrador | Las tarjetas CRC definen ambas clases como subclases de `Usuario`. |
| Asociación | UsuarioCocina / UsuarioMostrador → PantallaPedidos | Los actores acceden al monitor y consultan los resultados; no se indica que formen parte del ciclo de vida de la pantalla. |
| Asociación | PantallaPedidos → ServicioPedidos | La pantalla tiene el atributo `servicio: ServicioPedidos`; es una referencia estructural usada para procesar las consultas. |
| Asociación | Pedido → EstadoPedido | Cada pedido conserva su estado actual; el enum limita los estados que puede tener. |
| Composición | Pedido ◆→ ItemPedido (1 a 1..*) | Los ítems son el detalle que integra el pedido y se presentan como parte de éste. La multiplicidad mínima expresa el pedido con uno o más ítems. |
| Dependencia | ServicioPedidos → Pedido | El servicio filtra su colección de pedidos por estados con `obtenerPedidosPorEstados(estados)` y consulta estado, prioridad y detalle de los pedidos; no declara una referencia permanente a cada pedido. |
| Dependencia | ServicioPedidos → EstadoPedido | El servicio usa los valores del enum para filtrar, agrupar y seleccionar estados visibles. |
| Dependencia | ServicioPedidos → ItemPedido | El tipo de retorno del detalle es una lista de ítems del pedido. |
| Dependencia | ServicioPedidos → Usuario | El actor solicitante determina cómo se adapta la vista. |

---

## 6. Pseudocódigo

```text
INICIO Consultar Pedidos Activos

// Cocina o el Usuario de Mostrador accede al monitor de pedidos activos.
actorSolicitante accede mediante PantallaPedidos.accederMonitorPedidosActivos()

// Este mensaje inicia el procesamiento de la consulta en el servicio.
consulta = ServicioPedidos.consultarPedidosActivos(actorSolicitante)

SI ServicioPedidos.comprobarDisponibilidadDatos() es falso
    // La información de la pantalla puede no estar actualizada.
    PantallaPedidos advierte datos desactualizados
    Retornar "Aviso de datos desactualizados"
SINO
    listaPedidosActivos = ServicioPedidos.obtenerPedidosPorEstados([RECIBIDO, EN_PREPARACION, LISTO])

    SI listaPedidosActivos está vacía
        // No hay pedidos en curso para presentar.
        PantallaPedidos muestra la vista vacía con "No hay pedidos activos"
        Retornar "Vista vacía"
    FIN SI

    PARA CADA pedido EN listaPedidosActivos
        estadoActual = pedido.obtenerEstado()
        esUrgente = pedido.esPrioritario()
    FIN PARA

    gruposPorEstado = ServicioPedidos.agruparPedidosPorEstado(listaPedidosActivos)

    PARA CADA grupo de pedidos por estado
        ServicioPedidos.ordenarGrupoPorPrioridad(urgentesPrimero)
    FIN PARA

    estadosVisibles = ServicioPedidos.seleccionarEstadosVisibles(actorSolicitante)
        // Cocina: RECIBIDO y EN_PREPARACION
        // Usuario de Mostrador: LISTO

    // consulta devuelve la vista agrupada, ordenada y adaptada al actor.
    vistaAdaptada = resultado de consulta
    pantalla recibe y presenta vistaAdaptada al actorSolicitante

    SI el actor solicita consultar el detalle de un pedido
        numeroPedido = identificador del pedido seleccionado
        actorSolicitante solicita detalle mediante PantallaPedidos.consultarDetallePedido(numeroPedido)
        detallePedido = ServicioPedidos.obtenerDetallePedido(numeroPedido)
            // ServicioPedidos obtiene el detalle mediante Pedido.obtenerDetalle()
        pantalla presenta detallePedido al actorSolicitante
    FIN SI
FIN SI

// Estado final: los pedidos activos quedan disponibles para consulta,
// agrupados por estado y con los prioritarios primero en cada grupo.
FIN
```

**Nota de consistencia del pseudocódigo:** `seleccionarEstadosVisibles(actorSolicitante)` tiene la misma firma en el pseudocódigo, la secuencia, el diagrama de clases del CU5 y el diagrama final. El servicio decide los estados visibles según el actor: Cocina ve `RECIBIDO` y `EN_PREPARACION`; Mostrador ve `LISTO`, como en el diagrama de casos de uso y el de actividades.