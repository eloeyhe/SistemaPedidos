# Informe de Interacción con IA - Especialista en SRP 

**Rol:** Especialista en Principios de Extensión (OCP + LSP) 

**Herramienta utilizada:** GitHub Gemini Extensión Terminal


  --- 

  ## 1. Archivos de Contexto Referenciados: 
  * `anexos/introduccion.md\` 
  * `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw\`
  * `herramientas-agile/tarjetas-crc/\` 
   
   --- 
   ## 2. Prompt Utilizado:
   ```text
   Lee los siguientes archivos de mi proyecto: anexos/introduccion.md, diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw y todas las tarjetas CRC dentro de herramientas-agile/tarjetas-crc/. 

1. Para OCP: Identifica clases con lógica condicional que podría reemplazarse con herencia o polimorfismo, proponiendo extensiones sin modificar código existente. 
2. Para LSP: Analiza las jerarquías de herencia propuestas y verifica que las subclases puedan sustituir a sus superclases sin alterar el comportamiento esperado.
 ```

 
## 3. Output obtenido:

```text
A partir del análisis exhaustivo de anexos/introduccion.md, el boceto de clases en
diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw y las 16 tarjetas CRC en
herramientas-agile/tarjetas-crc/, se presentan las evaluaciones detalladas para
los principios OCP y LSP.

---

1. Principio de Abierto/Cerrado (OCP - Open/Closed Principle)

> «Las entidades de software deben estar abiertas a la extensión, pero cerradas a
la modificación.»
> 

Se identificaron cuatro clases críticas donde actualmente existe (o se requerirá)
lógica condicional basada en tipos o estados, la cual viola OCP y debe
reemplazarse por abstracciones y polimorfismo:

A. Clase Pago (Bifurcación por Metodo_pago)

- Problema identificado:
    - En el diagrama y en 05-tarjeta-crc-pago.md, Pago almacena un atributo
    metodoDePago: Metodo_pago (enum con valores efectivo, transferencia).
    - En sus métodos registrarPago() y calcularDiferencia(), la lógica de cobro
    (calcular vuelto, controlar apertura de caja física en efectivo vs. validar
    acreditación y registrar comprobante en transferencia) requiere bifurcaciones
    condicionales (switch (metodoDePago) o if-else).
    - El requisito RF8 anticipa que en próximas fases se incorporarán tarjetas y
    códigos QR. Mantener un enum obligará a modificar permanentemente la clase
    Pago y sus condicionales cada vez que se agregue una pasarela.
- Solución polimórfica (Patrón Strategy):
    - Definir una abstracción común: interfaz o clase abstracta EstrategiaPago (o
    MetodoPago) con operaciones como procesarCobro(monto: decimal): ResultadoPago
    y procesarAjuste(diferencia: decimal): void.
    - Crear subclases concretas: PagoEfectivo y PagoTransferencia.
    - Pago delega la ejecución a su estrategia inyectada.
    - Extensión sin modificar código: Nuevos canales como PagoQR o PagoTarjeta se
    añadirán como nuevas clases que implementen la interfaz, sin alterar Pago ni
    las estrategias ya probadas. (Nótese que esta abstracción polimórfica ya
    había sido planteada conceptualmente en la sección 4 de introduccion.md, pero
    no se plasmó en el diagrama ni en la tarjeta CRC).

---

B. Clase Pedido (Lógica condicional según Estado_pedido)

- Problema identificado:
    - En 01-tarjeta-crc-pedido.md y el boceto, Pedido gestiona directamente estado:
    Estado_pedido (recibido, en_preparacion, listo, entregado, cancelado) y
    métodos operativos como modificar(), cancelar(), cambiarEstado() y
    priorizar().
    - Según los casos de uso CU02, CU03 y CU04, cada método tiene validaciones de
    estado fuertemente acopladas:
        - modificar(): if (estado != RECIBIDO) throw Error.
        - cancelar(): if (estado == EN_PREPARACION) requiereSupervisor(); else if
        (estado == LISTO) prohibir();.
        - cambiarEstado(): lógica de control de transiciones secuenciales permitidas.
    - Si se introducen nuevos estados del ciclo de vida (ej. en_reparto,
    en_espera_stock, devuelto), se deberán alterar todos los métodos de Pedido
    para incluir nuevas ramas condicionales.
- Solución polimórfica (Patrón State):
    - Crear una interfaz o clase abstracta EstadoPedido con métodos:
    modificar(pedido: Pedido), cancelar(pedido: Pedido, usuario: Usuario),
    avanzar(pedido: Pedido), etc.
    - Crear clases concretas: EstadoRecibido, EstadoEnPreparacion, EstadoListo,
    EstadoEntregado, EstadoCancelado.
    - Pedido mantiene una referencia al objeto estado actual y delega en él la
    ejecución de la regla de negocio.
    - Extensión sin modificar código: Nuevas etapas o flujos se introducen mediante
    nuevas clases de estado que definen sus propias transiciones y restricciones
    sin tocar la clase central Pedido.

---

C. Catálogo de Artículos y Líneas de Pedido (Item_pedido frente a Producto y
Combo)

- Problema identificado:
    - En el diagrama, Combo es una entidad aislada que no comparte superclase con
    Producto.
    - Sin embargo, según RF1 y CU01, tanto un producto individual como un combo
    pueden ser agregados a un pedido dentro de un Item_pedido.
    - Actualmente, Item_pedido solo se asocia con Producto. Para soportar combos
    bajo el modelo actual, Item_pedido necesitaría campos opcionales (producto,
    combo) y condicionales en calcularSubtotal(): if (producto != null) ... else
    if (combo != null) ....
- Solución polimórfica (Patrón Composite / Interfaz de Catálogo):
    - Crear una abstracción común: interfaz Vendible o ElementoCatalogo con métodos
    getPrecio(): decimal y getDescripcion(): string.
    - Tanto Producto (y sus derivados) como Combo implementan Vendible. De hecho,
    un Combo puede implementar el patrón Composite conteniendo una lista de
    Producto.
    - Item_pedido colabora únicamente con Vendible.
    - Extensión sin modificar código: Nuevos formatos comerciales (como promociones
    especiales, packs de degustación o menús infantiles) implementarán Vendible
    sin requerir modificaciones en Item_pedido ni en la lógica de cálculo del
    subtotal.

---

D. Variantes de Personalizacion_item

- Problema identificado:
    - En RF1, las personalizaciones comprenden: quitar ingredientes (sin costo) o
    agregar adicionales (con costo extra).
    - En 04-tarjeta-crc-personalizacion-item.md, la clase tiene costoAdicional y
    calcularCosto(). Si se usa una única clase concreta con flags o tipos de
    personalización, se requerirá evaluar if (tipo == AGREGAR) sumar() else if
    (tipo == QUITAR) noCobrar().
- Solución polimórfica:
    - Abstracción Personalizacion con métodos calcularCosto(): decimal y
    obtenerInstruccion(): string.
    - Subclases: AdicionalConCosto (aplica importe adicional) y
    ModificacionIngrediente (costo cero, solo instrucción operativa).
    - Extensión sin modificar código: Permite incorporar sustituciones de
    ingredientes con recargo diferencial o personalizaciones complejas mediante
    nuevas subclases sin impactar en Item_pedido.

```

  ## 4. Ajustes realizados:

  * "La única modificación fue en cuanto a sintaxis, ya que la IA agrega 'id' delante de los atributos, lo cual en nuestro caso no sería la forma correcta. Se eliminó el mismo. 