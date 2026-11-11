# Documentación de Uso de IA - Analista Funcional de Casos de Uso 4 y 5

Este documento registra el proceso de generación asistida con **GitHub Copilot Agent Mode** y la posterior revisión y refinamiento manual para el diseño de los diagramas de clases específicos correspondientes al **Caso de Uso 4 (Cambiar Estado de Pedido)** y al **Caso de Uso 5 (Consultar Pedidos Activos)** en la Actividad Obligatoria N°4.

---

## 1. Diagrama de Clases - Caso de Uso 4: Cambiar Estado de Pedido

### 1.1 Archivos de contexto referenciados
Para la generación del diagrama `diagramas/01-diagrama-clases/04-clases-cambiar-estado-pedido-04.puml`, se adjuntaron explícitamente en Copilot Agent Mode los siguientes artefactos del repositorio:

- **Boceto inicial de clases (A1):** `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- **Diagrama de caso de uso (A2):** `diagramas/02-casos-de-uso/02-caso-uso-cambiar-estado-pedido-04.puml`
- **Escenario de caso de uso (A2):** `diagramas/03-escenarios-casos-de-uso/03-cambiar-estado-pedido-avance-preparacion-01.md`
- **Diagrama de actividades (A3):** `diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-pedido-04.puml`
- **Diagrama de secuencia (A3):** `diagramas/05-diagramas-secuencia/05-secuencia-cambiar-estado-pedido-avance-preparacion-04.puml`
- **Tarjetas CRC involucradas (A2):**
  - `herramientas-agile/tarjetas-crc/` (`Pedido`, `Estado_pedido`, `Historial_cambio_pedido`, `Usuario`, `UsuarioCocina`, `UsuarioMostrador`)

### 1.2 Prompt utilizado

```text
Actúa como un Analista Funcional y Diseñador Orientado a Objetos experto en PlantUML.
Analiza explícitamente los siguientes archivos de contexto del proyecto SistemaPedidos:
- #file:diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
- #file:diagramas/02-casos-de-uso/02-caso-uso-cambiar-estado-pedido-04.puml
- #file:diagramas/03-escenarios-casos-de-uso/03-cambiar-estado-pedido-avance-preparacion-01.md
- #file:diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-pedido-04.puml
- #file:diagramas/05-diagramas-secuencia/05-secuencia-cambiar-estado-pedido-avance-preparacion-04.puml
- #file:herramientas-agile/tarjetas-crc/ (tarjetas CRC de Pedido, Estado_pedido, Historial_cambio_pedido, Usuario, UsuarioCocina, UsuarioMostrador)

Genera el código PlantUML para el diagrama de clases específico del Caso de Uso 4 (Cambiar Estado de Pedido) en el archivo `diagramas/01-diagrama-clases/04-clases-cambiar-estado-pedido-04.puml` cumpliendo estrictamente estos requisitos:
1. Incluir ÚNICAMENTE las clases, interfaces o enumeraciones que participan en la resolución del Caso de Uso 4 (actualización del estado operativo del pedido siguiendo las transiciones RECIBIDO, EN_PREPARACION, LISTO y ENTREGADO).
2. Definir para cada clase: nombre exacto, atributos con modificadores de visibilidad (-, +, #) y tipos de datos, y métodos con visibilidad, parámetros tipados y tipo de retorno.
3. Asegurar coherencia total con las tarjetas CRC y el diagrama de secuencia del CU4: los participantes del diagrama de secuencia y los mensajes enviados deben corresponderse exactamente con los nombres de las clases, atributos y métodos de este diagrama.
4. Utilizar la simbología UML correcta en PlantUML para las relaciones (herencia `<|--`, composición `*--`, agregación `o--`, asociación `-->` y dependencia `..>`) indicando la multiplicidad en cada extremo.
```

### 1.3 Iteraciones relevantes y ajustes realizados al output de la IA

- **Iteración 1 (Generación inicial con Copilot Agent Mode):** La IA identificó los participantes del diagrama de secuencia y las tarjetas CRC (`Usuario`, `UsuarioCocina`, `UsuarioMostrador`, `PantallaPedidos`, `ServicioPedidos`, `Pedido`, `Historial_cambio_pedido` y `Estado_pedido`), generando un primer diagrama funcional pero con inconsistencias semánticas en las relaciones UML.
- **Corrección manual del carácter abstracto de `Usuario`:** En la primera salida, la IA declaró `class Usuario` como una clase concreta. Al revisar el boceto inicial y contrastarlo con el modelo del CU05, detecté la inconsistencia y lo modifiqué manualmente a `abstract class Usuario`, dado que en el dominio no se instancian usuarios genéricos sino sus especializaciones (`UsuarioCocina` y `UsuarioMostrador`).
- **Eliminación manual de relaciones redundantes entre actores y `Pedido`:** El output inicial de la IA trazaba dos asociaciones directas (`prepara` y `entrega`) desde `UsuarioCocina` y `UsuarioMostrador` hacia la clase `Pedido`, además de sus relaciones con `PantallaPedidos`. Al contrastar esto con el diagrama de secuencia del CU04, advertí que los actores no manipulan la entidad `Pedido` de forma directa, sino exclusivamente a través de `PantallaPedidos` y `ServicioPedidos`, por lo que eliminé esas dos flechas directas para respetar la arquitectura en capas del flujo.
- **Reemplazo manual de dependencias (`..>`) por asociaciones estructurales (`-->`):** Detecté que la IA había modelado con líneas punteadas de dependencia (`..>`) la relación entre `PantallaPedidos` y `ServicioPedidos`, así como la relación entre `Pedido` y `Estado_pedido`, a pesar de que `PantallaPedidos` posee el atributo `-servicio: ServicioPedidos` y `Pedido` posee el atributo `-estado: Estado_pedido`. Corregí ambas flechas reemplazándolas por asociaciones dirigidas continuas (`PantallaPedidos "0..*" --> "1" ServicioPedidos : utiliza` y `Pedido "0..*" --> "1" Estado_pedido : estado actual`).
- **Depuración de atributos duplicados y ajuste de relación en `Historial_cambio_pedido`:** En la versión inicial de la IA, la clase `Historial_cambio_pedido` incluía los atributos `-pedido: Pedido` y `-usuario: Usuario` dentro de la caja de texto y, al mismo tiempo, dibujaba las líneas de relación hacia `Pedido` y `Usuario`, duplicando la información estructural. Eliminé manualmente ambos atributos redundantes del compartimento de la clase para representarlos únicamente mediante sus asociaciones UML (`Pedido "1" <-- "0..*" Historial_cambio_pedido : pedido` e `Historial_cambio_pedido "0..*" --> "1" Usuario : actor`), cambiando además la composición inicial por una asociación dirigida acorde a lo especificado en su tarjeta CRC.
- **Estandarización visual en PlantUML:** Agregué las directivas `skinparam classAttributeIconSize 0` y `hide empty members` para mostrar explícitamente los símbolos estándar de visibilidad UML (`-`, `+`) y ocultar compartimentos vacíos en las subclases `UsuarioCocina` y `UsuarioMostrador`.

---

## 2. Diagrama de Clases - Caso de Uso 5: Consultar Pedidos Activos

### 2.1 Archivos de contexto referenciados
Para la generación del diagrama `diagramas/01-diagrama-clases/05-clases-consultar-pedidos-activos-05.puml`, se adjuntaron explícitamente en Copilot Agent Mode los siguientes artefactos del repositorio:

- **Boceto inicial de clases (A1):** `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- **Diagrama de caso de uso (A2):** `diagramas/02-casos-de-uso/02-caso-uso-consultar-pedidos-activos-05.puml`
- **Escenario de caso de uso (A2):** `diagramas/03-escenarios-casos-de-uso/03-consultar-pedidos-activos-prioridad-urgentes-01.md`
- **Diagrama de actividades (A3):** `diagramas/04-diagramas-actividades/04-actividad-consultar-pedidos-activos-05.puml`
- **Diagrama de secuencia (A3):** `diagramas/05-diagramas-secuencia/05-secuencia-consultar-pedidos-activos-prioridad-urgentes-05.puml`
- **Tarjetas CRC involucradas (A2):**
  - `herramientas-agile/tarjetas-crc/` (`Pedido`, `Estado_pedido`, `Item_pedido`, `Usuario`, `UsuarioCocina`, `UsuarioMostrador`)

### 2.2 Prompt utilizado

```text
Actúa como un Analista Funcional y Diseñador Orientado a Objetos experto en PlantUML.
Analiza explícitamente los siguientes archivos de contexto del proyecto SistemaPedidos:
- #file:diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
- #file:diagramas/02-casos-de-uso/02-caso-uso-consultar-pedidos-activos-05.puml
- #file:diagramas/03-escenarios-casos-de-uso/03-consultar-pedidos-activos-prioridad-urgentes-01.md
- #file:diagramas/04-diagramas-actividades/04-actividad-consultar-pedidos-activos-05.puml
- #file:diagramas/05-diagramas-secuencia/05-secuencia-consultar-pedidos-activos-prioridad-urgentes-05.puml
- #file:herramientas-agile/tarjetas-crc/ (tarjetas CRC involucradas: Pedido, Estado_pedido, Item_pedido, Usuario, UsuarioCocina, UsuarioMostrador)

Genera el código PlantUML para el diagrama de clases específico del Caso de Uso 5 (Consultar Pedidos Activos) en el archivo `diagramas/01-diagrama-clases/05-clases-consultar-pedidos-activos-05.puml` cumpliendo estrictamente estos requisitos:
1. Incluir ÚNICAMENTE las clases, interfaces o enumeraciones que participan en la resolución del Caso de Uso 5 (consulta, filtrado por estado y ordenamiento por prioridad/urgencia de pedidos activos).
2. Definir para cada clase: nombre exacto, atributos con modificadores de visibilidad (-, +, #) y tipos de datos, y métodos con visibilidad, parámetros tipados y tipo de retorno.
3. Mantener coherencia estricta con las tarjetas CRC y el diagrama de secuencia del CU5: respetar exactamente la convención de nombres del proyecto (como `Pedido`, `Estado_pedido`, `Item_pedido`, `Usuario`, `UsuarioCocina`, `UsuarioMostrador`), así como las propiedades (`estado`, `prioritario`, `numeroPedido`, `referenciaRetiro`, `fecha`) y las firmas de los mensajes enviados en el diagrama de secuencia.
4. No inventar atributos ni métodos ajenos a este caso de uso que no estén respaldados por el diagrama de secuencia, las tarjetas CRC o el boceto inicial.
5. Representar las relaciones UML con la simbología correcta de PlantUML (herencia `<|--`, composición `*--`, agregación `o--`, asociación `-->` y dependencia `..>`) indicando las multiplicidades correspondientes en cada extremo.
```

### 2.3 Iteraciones relevantes y ajustes realizados al output de la IA

- **Iteración 1 (Generación inicial con Copilot Agent Mode):** La IA generó una primera versión del diagrama incluyendo `Usuario`, `UsuarioCocina`, `UsuarioMostrador`, `PantallaPedidos`, `ServicioPedidos`, `Pedido`, `Item_pedido` y `Estado_pedido`, pero incorporó mensajes de respuesta como si fueran métodos y omitió atributos de referencia entre capas.
- **Incorporación manual del atributo `-servicio: ServicioPedidos` y corrección de asociaciones (`-->`):** Al comparar el output inicial del CU05 con el del CU04, noté que la IA había omitido el atributo `-servicio: ServicioPedidos` dentro de `PantallaPedidos` y había utilizado una flecha de dependencia (`..>`) tanto allí como entre `Pedido` y `Estado_pedido` (donde `Pedido` declara `-estado: Estado_pedido`). Agregué manualmente el atributo `-servicio: ServicioPedidos` en `PantallaPedidos` y cambié ambas relaciones a asociaciones dirigidas (`PantallaPedidos "0..*" --> "1" ServicioPedidos : consulta` y `Pedido "0..*" --> "1" Estado_pedido : estado`).
- **Depuración manual de mensajes de retorno mal catalogados como métodos (Iteración 2 de auditoría):** Al revisar en detalle las operaciones que la IA había puesto dentro de `PantallaPedidos` y `ServicioPedidos` contra el diagrama de secuencia `05-secuencia-consultar-pedidos-activos-prioridad-urgentes-05.puml`, detecté que `mostrarVistaMonitor`, `mostrarDetallePedido`, `mostrarVistaVacia`, `advertirDatosDesactualizados`, `retornarPedidosAgrupadosYOrdenados` y `retornarDetalle` figuraban en el diagrama de secuencia sobre flechas punteadas de retorno (`-->`) y no como invocaciones de métodos (`->`). Procedí a eliminarlos de las clases para que solo queden declaradas las operaciones realmente invocadas, representando las respuestas a través de los tipos de retorno (`Map<Estado_pedido, List<Pedido>>` y `List<Item_pedido>`).
- **Corrección de multiplicidad en la composición con `Item_pedido` y dependencias de `ServicioPedidos`:** En la primera salida de la IA, la composición entre `Pedido` e `Item_pedido` tenía multiplicidad `1` a `0..*`. La corregí manualmente a `"1" *-- "1..*"` dado que por regla de negocio todo pedido activo en curso contiene al menos un ítem. Asimismo, agregué las relaciones de dependencia `ServicioPedidos ..> Item_pedido : obtiene detalle` y `ServicioPedidos ..> Usuario : adapta vista`, ya que `ServicioPedidos` recibe al `Usuario` solicitante como parámetro y retorna colecciones de `Item_pedido`.
- **Verificación de ámbito de instancia en `Pedido.obtenerPedidosPorEstados()`:** Me aseguré de mantener el método `obtenerPedidosPorEstados(...)` como operación de instancia (sin el modificador `{static}`) para conservar trazabilidad exacta con la línea de vida `:Pedido` utilizada como receptor del mensaje en el diagrama de secuencia de A3.