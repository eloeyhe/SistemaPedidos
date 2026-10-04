# Informe de Interacción con IA - Especialista en SRP 

**Rol:** Especialista en Inversión de Dependencias (DIP) 

**Herramienta utilizada:** GitHub Copilot Agent Mode (VS Code)


  --- 

  ## 1. Archivos de Contexto Referenciados: 
  * `anexos/introduccion.md\` 
  * `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw\`
  * `herramientas-agile/tarjetas-crc/\` 
   
   --- 
   ## 2. Prompt Utilizado:
   ```text
  Actúa como un Arquitecto de Software experto en principios SOLID, enfocado en el Principio de Inversión de Dependencias (DIP).

Por favor, analiza el contexto de mi proyecto leyendo los siguientes archivos de mi espacio de trabajo:
- `anexos/introduccion.md`
- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- Todas las tarjetas CRC dentro del directorio `herramientas-agile/tarjetas-crc/`

Teniendo en cuenta el contexto de nuestro sistema de pedidos, realiza las siguientes tareas:

1. Análisis de Dependencias: Identifica en el diseño actual dónde existen dependencias directas hacia clases concretas (fuerte acoplamiento).
2. Inversión de Dependencias (DIP): Propón las abstracciones necesarias (interfaces o clases abstractas) para invertir esas dependencias. 
3. Inyección de Dependencias: Indica de forma específica y práctica en qué clases y mediante qué mecanismo (por constructor, por setter, etc.) se debe aplicar la inyección de dependencias.
4. Coherencia de Dominio (Revisión crítica): Filtra tus propias propuestas. Asegúrate de que las abstracciones correspondan estrictamente a nuestro dominio de negocio. Ajusta los nombres para que sean representativos del sistema de pedidos y omite cualquier abstracción genérica que complique el diseño innecesariamente.

Presenta tu respuesta estructurada, justificando brevemente cada cambio propuesto. Con esto me refiero de que me devuelvas tu respuesta en formato texto aca en el chat, NO HAGAS NINGUN CAMBIO NI CREES NINGUN ARCHIVO NI NADA
 ```

 
  ## 3. Output obtenido:

```text
Análisis DIP del sistema de pedidos
Revisé la introducción y requisitos, el boceto de clases y las tarjetas CRC. La conclusión importante es que las asociaciones entre entidades del dominio no son automáticamente violaciones de DIP. El boceto tampoco define persistencia ni integraciones concretas; por eso no se puede afirmar que ya exista, por ejemplo, un acoplamiento a una base de datos específica. Sí aparecen algunas dependencias del diseño que conviene corregir o precisar.

1. Dependencias directas y observaciones
Item_pedido depende de Producto, pero los combos están modelados aparte. La tarjeta de Item Pedido lo vincula con Producto, mientras que Combo es una clase independiente. Esto no refleja RF1, que permite incluir productos y combos en un pedido.
Pedido colabora con Cliente. Esa relación aparece en Pedido y Cliente, pero contradice RF6 y la descripción del actor: no se registra una ficha de cliente; basta con la referencia de retiro.
UsuarioCocina se vincula directamente con ProductoPreparado. La tarjeta de Usuario Cocina lo presenta como colaborador. La operación descrita para cocina, en cambio, consiste principalmente en avanzar el estado del pedido y marcarlo listo; no requiere que ese usuario dependa de una clase concreta de producto preparado.
Otras asociaciones concretas —Pedido–Item_pedido, Item_pedido–Personalizacion_item y los historiales con Pedido— son relaciones propias del dominio. No propondría interfaces solo para reemplazarlas. Usuario, además, ya es una clase abstracta en Usuario.
Metodo_pago es una enumeración concreta, pero adecuada al alcance actual. La tarjeta de Pago y el diagrama muestran efectivo y transferencia, también en RF8. No hace falta introducir estrategias de pago ni clases separadas para QR o tarjetas: el requisito explícitamente excluye esos medios en esta fase.
La introducción ilustra cobros digitales y QR, pero los requisitos actuales acotan los medios a efectivo y transferencia. Para esta propuesta sigo el alcance de RF8, no el ejemplo ilustrativo de introducción. El boceto de clases y las tarjetas CRC sirven como base del modelo.
2. Abstracciones que sí propongo
ElementoMenu
Definiría la abstracción de dominio ElementoMenu, implementada por Producto y Combo. Expone solo los datos que una línea necesita para iniciar la venta, como nombre y precio vigente.

Así, el flujo que incorpora un elemento al pedido puede trabajar con el mismo concepto sin depender de dos clases concretas. Combo conserva su precio propio; la abstracción no implica que su precio se calcule sumando componentes.

Al crear la línea, Item_pedido debe guardar el precio unitario histórico y los datos necesarios para el detalle del pedido. Luego sus recálculos usan esa copia y el costo de las personalizaciones, no el precio actual del catálogo. Esto respeta los requisitos de conservación del precio histórico.

RepositorioPedidos
Definiría el puerto de persistencia RepositorioPedidos, orientado a operaciones del dominio, como guardar un pedido, buscarlo por identificador y consultar los pedidos activos. La capa de aplicación dependería de este contrato, no de una base de datos concreta.

CatalogoMenu
Definiría CatalogoMenu como el puerto de consulta del catálogo: el caso de uso obtiene por identificador un ElementoMenu disponible. La implementación concreta —base de datos u otra fuente— queda fuera del dominio.

Esta separación no reemplaza ElementoMenu: el primero permite resolver elementos del catálogo; el segundo representa qué puede venderse.

PublicadorComandasCocina
Definiría PublicadorComandasCocina para que el caso de uso de confirmación entregue el pedido al canal de cocina sin depender de una pantalla, conexión o tecnología concreta. La necesidad se desprende del envío automático previsto por RF2.

Para mantener la consistencia requerida por RNF4, la publicación debe ocurrir después de guardar con éxito el pedido. Si la arquitectura debe garantizar la entrega aun ante fallos de red o energía, se deberá resolver esa confiabilidad en la infraestructura; el boceto actual no especifica el mecanismo.

3. Dónde y cómo inyectar las dependencias
Las dependencias de infraestructura deben entrar en casos de uso o servicios de aplicación, con inyección por constructor. Por ejemplo:

ServicioTomarPedido(RepositorioPedidos, CatalogoMenu, PublicadorComandasCocina): obtiene los elementos del catálogo, construye el pedido, lo guarda y luego solicita su publicación a cocina.
ServicioModificarPedido(RepositorioPedidos, CatalogoMenu): recupera el pedido y resuelve los elementos que se agreguen o cambien. El pedido conserva su identificador original y sus precios históricos; el ajuste financiero se registra según el flujo existente.
ServicioConsultarPedidos(RepositorioPedidos): obtiene los pedidos activos para las vistas de mostrador y cocina.
ServicioCambiarEstadoPedido(RepositorioPedidos) y ServicioCancelarPedido(RepositorioPedidos): recuperan y guardan el pedido, mientras el propio objeto Pedido valida las reglas de estado y cancelación.
La inyección por constructor hace explícitos los requisitos de cada servicio y evita que estos creen internamente dependencias concretas. La configuración inicial del sistema —el punto de composición— conecta las implementaciones reales de los puertos.

No inyectaría servicios en Pedido, Item_pedido o Pago. Son entidades del dominio: reciben datos necesarios para construirse y hacen cumplir sus reglas, pero no deberían conocer repositorios, pantallas ni mecanismos de envío. El usuario responsable puede recibirse como argumento de la operación y registrarse en la auditoría, sin acoplar la operación a UsuarioMostrador, UsuarioCocina o UsuarioEncargado.

4. Revisión de coherencia y propuestas descartadas
Retiraría Cliente del modelo persistente del pedido si no existe otro requisito que lo justifique. La referencia de retiro ya cubre la identificación descrita por RF6.
Mantendría Pedido–Item_pedido y Item_pedido–Personalizacion_item como relaciones concretas. Son conceptos propios y estables del dominio; agregarles interfaces no aporta una inversión útil.
Mantendría Metodo_pago como enumeración mientras los únicos medios sean efectivo y transferencia. No añadiría ProcesadorPago, PagoDigital ni una abstracción genérica de pasarela sin una integración externa real.
No agregaría ahora una interfaz de reloj o generador de números. El número único debe resolverse en el mecanismo de persistencia elegido y no hay en el diseño evidencia de una dependencia concreta de reloj que amerite otro contrato.
No haría que el usuario de cocina dependa de ProductoPreparado. La transición del pedido y su progreso corresponden al flujo del pedido; conservaría la subclase solo si representa diferencias reales del catálogo o reglas del producto.
En síntesis, las inversiones que sí responden a necesidades identificables son ElementoMenu, RepositorioPedidos, CatalogoMenu y PublicadorComandasCocina. El resto de las relaciones del modelo debe mantenerse directo salvo que aparezca una variación concreta que justifique abstraerla.
```
  ## 4. Ajustes realizados:

  * Se ajustaron las firmas de las interfaces de infraestructura (`RepositorioPedidos`, `PublicadorComandasCocina`) para alinearlas con los requisitos funcionales del sistema de pedidos (RF1 y RF2), y se redactó la sección teórica formalizando los conceptos de clase abstracta e interfaz requeridos por la consigna del parcial.