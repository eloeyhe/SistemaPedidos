# Anexo - Introducción al Diseño Orientado Objetos 
El __Paradigma Orientado a Objetos (POO)__ es un enfoque de ingeniería de software que organiza el diseño en torno a modelos de objetos físicos o simulados del mundo real, en lugar de estructurarlo como una secuencia de tareas o funciones secuenciales.

Mientras que el enfoque procedimental o estructurado separa los datos de los procesos, el paradigma orientado a objetos une ambos mundos. Su principio fundamental es combinar o encapsular en una sola unidad (el objeto) tanto los datos (atributos) como el comportamiento (métodos) que opera sobre esos datos.

En el software, cada objeto posee 3 caracteristicas esenciales: 

- __Estado:__ El conjunto de valores que toman sus atributos en un instante específico de tiempo.
- __Comportamiento:__ El conjunto de operaciones o métodos que el objeto puede realizar y a los cuales responde mediante el envío de mensajes.
- __Identidad:__ Lo que permite diferenciar a un objeto de otro de forma no ambigua, incluso si todos sus atributos son idénticos, ya que cada uno ocupa su propia posición en la memoria

__¿Por qué es importante?__

La importancia de adoptar este paradigma radica en las ventajas técnicas que aporta para el desarrollo de software moderno y complejo:

1. __Estructura sólida para los sistemas complejos:__ Permite modelar problemas grandes y complejos dividiendo el software en unidades cohesivas, autónomas y fáciles de comprender.
2. __Reutilización de codigo (Reusabilidad):__ Al definir clases (que funcionan como plantillas o moldes de construcción) y utilizar la herencia, se pueden crear nuevos objetos basados en clases existentes sin necesidad de reescribir código desde cero.
3. __Ocultamiento y seguridad de datos (Encapsulamiento):__ El estado interno de un objeto está protegido de accesos externos no autorizados. Si un agente externo quiere leer o modificar un dato, está obligado a hacerlo exclusivamente a través de los métodos públicos definidos por el objeto, garantizando que las reglas del negocio se cumplan siempre.
4. __Mantenimiento ágil ante cambios continuos:__ En sistemas complejos, los requerimientos cambian constantemente. Al encapsular los datos y comportamientos dentro de objetos aislados, un cambio en el diseño de un objeto específico tiene un impacto mínimo en el resto del sistema, evitando tener que reescribir o dañar todo el software.

## Los cuatro fundamentos de POO

### Abstracción
Se aíslan únicamente las propiedades críticas de una comanda para el negocio, ignorando detalles físicos innecesarios del cliente en el mostrador.

Representación visual: 
```plaintext
+------------------------------------+
|               Pedido               |
+------------------------------------+
| - estado: String                   |
| - estaPagado: boolean              |
+------------------------------------+
| + registrarPago()                  |
| + actualizarEstado(nuevoEstado)    |
+------------------------------------+
```

### 2. Encapsulamiento
El estado del pago está protegido de modificaciones externas directas. Solo se puede alterar de manera segura invocando el método público de registro de cobro.

Representación visual: 
```plaintext
+------------------------------------+
|               Pedido               |
+------------------------------------+
| - estado: String                   | <--- Atributos ocultos (Privados)
| - estaPagado: boolean              |
+------------------------------------+
| + registrarPago()                  | <--- Interfaz de control (Pública)
| + actualizarEstado(nuevoEstado)    |
+------------------------------------+
```

### 3. Herencia
Se extraen los atributos comunes a una superclase general ("Producto") y se extienden comportamientos especializados para los alimentos de cocina y los artículos de góndola.

Representación visual: 
```plaintext
+------------------------------------+
            |              Producto              | <--- Superclase (General)
            +------------------------------------+
            | - codigo: String                   |
            | - nombre: String                   |
            | - precio: double                   |
            +------------------------------------+
                              ^
                              | (hereda de / es un)
             +----------------+----------------+
             |                                 |
+-------------------------+       +-------------------------+
|    ProductoPreparado    |       |    ProductoEnvasado     | <--- Subclases
+-------------------------+       +-------------------------+      (Especializadas)
| - tiempoPreparacion: int|       | - codigoBarras: String  |
+-------------------------+       +-------------------------+
```

### 4. Polimorfismo
Un mismo mensaje de cobro se ejecuta de manera completamente diferente según el canal de pago seleccionado por el cliente en el mostrador.

Representación visual: 
```plaintext
[ Mensaje común enviado: pago.procesarCobro() ]
                                 |
                 +---------------+---------------+
                 |                               |
                 v                               v
          [ PagoEfectivo ]               [ PagoDigital ]
                 |                               |
                 v                               v
     "Registra ingreso en caja y      "Sincroniza con pasarela QR
     calcula vuelto en efectivo"      y valida la acreditación online"
```

<<<<<<< HEAD
## Casos de uso

### CU01: Tomar Pedido
 __Actores involucrados:__ Usuario de Mostrador.

__Descripción breve:__
Permite al Usuario de Mostrador registrar un nuevo pedido, seleccionando productos o combos, agregando personalizaciones, registrando el medio de pago y confirmando el pedido para enviarlo a la pantalla de cocina.

![Diagramas CU1](/Diagramas/01-Diagrama-Clases/CU1.png)

 __Flujo principal de eventos:__

1. El Usuario de Mostrador inicia la creación de un nuevo pedido.
2. El sistema genera automáticamente un número de pedido único y registra la fecha y hora actual.
3. El usuario ingresa un nombre o referencia de retiro para identificar el pedido.
4. El usuario selecciona los productos o combos y especifica las cantidades.
5. El usuario agrega, si corresponde, las personalizaciones de cada ítem.
6. El sistema calcula el total del pedido en tiempo real, considerando extras, eliminaciones y el precio de catálogo de los combos.
7. El usuario selecciona la forma de pago: Efectivo o Transferencia.
8. El sistema registra el pago y el usuario confirma el pedido.
9. El sistema guarda el precio histórico de cada ítem y establece el estado del pedido como RECIBIDO.
10. El sistema envía automáticamente el pedido a la pantalla de cocina.

 __Precondiciones:__

- El Usuario de Mostrador debe tener acceso al sistema.
- Deben existir productos o combos disponibles para seleccionar.

 __Postcondiciones:__

- El pedido queda registrado con un identificador único.
- El pedido conserva el precio histórico de sus ítems.
- El pedido queda en estado RECIBIDO.
- El pedido queda disponible para ser visualizado por Cocina.
- El pago queda registrado con la forma seleccionada.

### CU02: Modificar Pedido

__Actores involucrados:__ Usuario de Mostrador.

__Descripción breve:__
Permite modificar un pedido existente mientras se encuentre en estado RECIBIDO, manteniendo su identificador y registro original.

![Diagramas CU2](/Diagramas/01-Diagrama-Clases/CU2.png)

__Flujo principal de eventos:__

1. El Usuario de Mostrador busca el pedido mediante su número o referencia de retiro.
2. El sistema verifica que el pedido se encuentre en estado RECIBIDO.
3. El usuario selecciona la opción de modificar el pedido.
4. El usuario agrega, elimina o modifica productos y cantidades.
5. El usuario edita las personalizaciones de los ítems cuando sea necesario.
6. El sistema recalcula el total del pedido.
7. Si el pedido ya fue pagado, el sistema calcula la diferencia a cobrar o ajustar.
8. El usuario registra el ajuste del pago, cuando corresponda.
9. El usuario confirma las modificaciones.
10. El sistema actualiza el pedido manteniendo su identificador y registro original.

__Precondiciones:__

- El pedido debe existir.
- El pedido debe encontrarse en estado RECIBIDO.

__Postcondiciones:__

- El pedido queda actualizado con las modificaciones realizadas.
- El identificador original del pedido se mantiene.
- El total del pedido queda actualizado.
- Cualquier ajuste de pago queda registrado.

### CU03: Cancelar Pedido

__Actores involucrados:__ Usuario de Mostrador / Encargado.

__Descripción breve:__
Permite cancelar un pedido que todavía no haya sido entregado, conservando su registro histórico para fines de auditoría.

![Diagramas CU3](/Diagramas/01-Diagrama-Clases/CU3.png)

__Flujo principal de eventos:__

1. El Usuario de Mostrador o Encargado selecciona el pedido que desea cancelar.
2. El sistema verifica el estado actual del pedido.
3. El sistema solicita confirmación de la cancelación.
4. El usuario confirma la cancelación.
5. El sistema cambia el estado del pedido a CANCELADO.
6. El sistema conserva el registro del pedido en la base de datos.
7. El sistema registra la cancelación como parte del historial del pedido.

__Precondiciones:__

- El pedido debe existir.
- El pedido no debe haber sido entregado.
- El usuario debe tener permisos para realizar la cancelación.

__Postcondiciones:__

- El pedido queda en estado CANCELADO.
- El pedido no es eliminado de la base de datos.
- El registro histórico del pedido queda disponible para auditoría.

### CU04: Cambiar Estado de Pedido

__Actores involucrados:__ Cocina / Usuario de Mostrador.

__Descripción breve:__
Permite actualizar el estado de un pedido durante su ciclo de vida, de acuerdo con las transiciones permitidas por el sistema.

![Diagramas CU4](/Diagramas/01-Diagrama-Clases/CU4.png)

__Flujo principal de eventos:__

1. El actor selecciona el pedido correspondiente en la interfaz.
2. El sistema muestra el estado actual del pedido.
3. Cocina cambia el estado de RECIBIDO a EN_PREPARACION al comenzar la preparación.
4. Cocina cambia el estado de EN_PREPARACION a LISTO al finalizar la preparación.
5. El Usuario de Mostrador cambia el estado de LISTO a ENTREGADO cuando el cliente retira el pedido.
6. El sistema valida que la transición solicitada sea válida según el ciclo de vida del pedido.
7. El sistema actualiza el estado único del pedido.

__Precondiciones:__

- El pedido debe existir.
- El actor debe tener permisos para realizar la transición correspondiente.
- El pedido debe encontrarse en un estado que permita la transición solicitada.

__Postcondiciones:__

- El pedido queda actualizado con su nuevo estado.
- El pedido mantiene un único estado vigente.
- El sistema impide transiciones no permitidas por el ciclo de vida del pedido.

### CU05: Consultar Pedidos Activos

__Actores involucrados:__ Cocina / Usuario de Mostrador.

__Descripción breve:__
Permite consultar los pedidos activos y visualizarlos según su estado actual, facilitando el seguimiento y la preparación de los pedidos.

![Diagramas CU5](/Diagramas/01-Diagrama-Clases/CU5.png)

__Flujo principal de eventos:__

1. El actor accede a la pantalla de monitoreo de pedidos.
2. El sistema obtiene los pedidos activos registrados.
3. El sistema muestra los pedidos agrupados o filtrados según su estado.
4. El sistema muestra los pedidos en los estados RECIBIDO, EN_PREPARACION y LISTO.
5. El sistema ordena visualmente los pedidos marcados como Urgentes/Prioritarios para que aparezcan primero.
6. El actor consulta la información de los pedidos según sus necesidades.
7. El sistema garantiza que cada pedido figure en un único estado a la vez.

__Precondiciones:__

- El actor debe tener acceso al sistema.
- Deben existir pedidos activos para consultar.

__Postcondiciones:__

- Los pedidos activos quedan disponibles para su consulta.
- Cada pedido se muestra en un único estado actual.
- Los pedidos prioritarios aparecen destacados en el orden correspondiente.


=======

# Requisitos iniciales del sistema
## Requisitos funcionales :


**RF1: Toma de pedidos con personalizaciones y combos** El sistema debe permitir al personal del mostrador registrar pedidos especificando productos individuales, combos predefinidos (los cuales tienen su propia lógica de precio y no corresponden a la simple suma de sus partes) e ítems con personalizaciones individuales de ingredientes (quitar ingredientes o agregar adicionales con costo extra), reflejando de forma precisa el desglose y el precio total recalculado en pantalla al momento del registro.

**RF2: Envío automático de comandas a la cocina** El sistema debe transmitir de forma automática e inmediata cada pedido confirmado desde el mostrador a la pantalla de la cocina, eliminando el traslado físico de comandas impresas o manuscritas y evitando pérdidas de información en el proceso.

**RF3: Seguimiento y visualización del estado del pedido** El sistema debe permitir al personal visualizar sincronizadamente los pedidos activos y controlar su ciclo de vida mediante un único estado secuencial y restringido: *Recibido*, *En preparación*, *Listo*, *Entregado* y *Cancelado*, garantizando una única fuente de verdad para evitar duplicaciones.

**RF4: Modificación de pedidos activos (antes de preparación)** El sistema debe permitir al usuario modificar los ítems, cantidades y personalizaciones de un pedido activo (manteniendo el mismo identificador y número de pedido original), única y exclusivamente mientras este se encuentre en estado *Recibido*; una vez que la cocina cambia el estado del pedido a *En preparación*, el sistema debe bloquear cualquier intento de modificación.

**RF5: Cancelación completa del pedido** El sistema debe posibilitar la cancelación completa de un pedido activo antes de que sea entregado, asegurando que por razones de auditoría interna de caja el registro nunca se elimine físicamente de la base de datos y que pase de forma definitiva y permanente al estado *Cancelado*.

**RF6: Identificación del pedido para el retiro** El sistema debe generar un identificador numérico único para cada pedido y permitir asociar de manera obligatoria un nombre o referencia de retiro (ej. "Matías", "Carla") en la comanda, facilitando el llamado en el mostrador para su entrega sin necesidad de registrar datos personales del cliente ni crear una ficha de cliente formal.

**RF7: Priorización manual** El sistema debe permitir marcar manualmente un pedido como prioritario (urgente) desde el mostrador al momento de tomarlo, visualizándose de forma destacada en la pantalla de cocina para que el personal altere el orden de preparación según su criterio.

**RF8: Registro de pago** El sistema debe permitir asociar y registrar el cobro de cada pedido utilizando únicamente efectivo o transferencia bancaria (dejando fuera QR y tarjetas en esta fase); asimismo, si un pedido previamente pagado es modificado en estado *Recibido*, el sistema debe recalcular el total y permitir registrar el cobro o ajuste de la diferencia económica generada.

---

## Requisitos NO funcionales

**RNF1: Escalabilidad y Extensibilidad (Soporte Multi-local)** El diseño de la base de datos y la arquitectura del modelo de dominio del sistema deben ser extensibles para permitir en el futuro la incorporación de un esquema multi-local sin requerir la reescritura de su núcleo conceptual, operando el MVP estrictamente para una única sucursal física.

**RNF2: Usabilidad y Simplicidad Operativa** La interfaz del usuario debe diseñarse bajo un estricto criterio de agilidad y máxima simplicidad, permitiendo al operario interactuar rápidamente con el sistema sin requerir flujos de navegación complejos o sobreingeniería visual para adaptarse al ritmo acelerado de atención del local.

**RNF3: Restricción de Tiempo de Entrega (Time-to-Market)** El Producto Mínimo Viable (MVP) que contiene el alcance acordado de los requisitos funcionales (RF1 a RF8) debe estar completamente funcional y desplegado en su entorno operativo para fines de julio.

**RNF4: Integridad de Datos y Consistencia (Fiabilidad)** El sistema debe asegurar la consistencia total del estado de los pedidos y de los cobros previniendo fallos transaccionales ante cortes de energía o de red, garantizando que no se dupliquen visualizaciones ni existan desfases entre lo que muestra el mostrador y la pantalla de la cocina.

**RNF5: Seguridad y Auditoría Básica (Trazabilidad)** El sistema debe garantizar la trazabilidad de la operación mediante el registro inalterable de qué usuario creó o modificó cada pedido, sin imponer restricciones rígidas de acceso o bloqueos basados en roles que entorpezcan la dinámica del negocio donde cualquiera de los operarios puede cobrar o tomar pedidos según la necesidad.

--- 


### Información analizada

- Correos electrónicos
- Audios
- Imágenes
- Notas proporcionadas por el cliente

### Cuaderno de NotebookLM
[Cuaderno de NotebookLM del análisis de requisitos](https://notebook.google.com/notebook/caeefeff-0269-4c85-af5b-95f379f0f4e4/preview)
##
>>>>>>> develop
