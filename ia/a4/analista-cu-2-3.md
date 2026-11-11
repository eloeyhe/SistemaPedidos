
# Actividad Obligatoria N.º 4
## Registro de uso de IA — Analista Funcional CU02 y CU03

---


### 2.1. Prompt utilizado

- CU02 — Modificar Pedido
- CU03 — Cancelar Pedido

```text
Actuá como analista funcional especializado en Diseño Orientado a Objetos.


analizá los archivos de referencia otorgados posteriormente y basate en ellos para realizar los diagramas de los CU2 y CU3:

- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- `diagramas/02-casos-de-uso/02-caso-uso-modificar-pedido-02.puml`
- `03-modificar-pedido-modificacion-recibido-01.md`
- `diagramas/04-diagramas-actividades/04-actividad-modificar-pedido-02.puml`
- `05-secuencia-modificar-pedido-modificacion-recibido-02.puml`
- `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
- `herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md`
- `herramientas-agile/tarjetas-crc/03-tarjeta-crc-producto.md`
- `herramientas-agile/tarjetas-crc/04-tarjeta-crc-personalizacion-item.md`
- `herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md`
- `herramientas-agile/tarjetas-crc/10-tarjeta-crc-historial-pago.md`
- `herramientas-agile/tarjetas-crc/12-tarjeta-crc-historial-cambio-pedido.md`
- `herramientas-agile/tarjetas-crc/13-tarjeta-crc-usuario.md`
- `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`

### CU03 — Cancelar Pedido

- `diagramas/02-casos-de-uso/02-caso-uso-cancelar-pedido-03.puml`
- `03-cancelar-pedido-cancelacion-antes-entrega-01.md`
- `diagramas/04-diagramas-actividades/04-actividad-cancelar-pedido-03.puml`
- `05-secuencia-cancelar-pedido-cancelacion-antes-entrega-03.puml`
- `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
- `herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md`
- `herramientas-agile/tarjetas-crc/11-tarjeta-crc-auditoria-pedido.md`
- `herramientas-agile/tarjetas-crc/13-tarjeta-crc-usuario.md`
- `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`
- `herramientas-agile/tarjetas-crc/16-tarjeta-crc-usuario-encargado.md`


Verificá que exista coherencia entre los artefactos anteriores.

 NO modifiques archivos.

Mostrame en un archivo puml el codigo de creacion del diagrma:

1. Qué representa CU2 y CU3.
2. Qué archivos encontraste.
3. Qué clases necesita cada caso de uso.
4. Qué inconsistencias detectaste.

No inventes clases ni funcionalidades que no estén justificadas por los documentos de los archivos referenciados.
```


### 2.3. Archivos Referenciados

- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- `diagramas/02-casos-de-uso/02-caso-uso-modificar-pedido-02.puml`
- `03-modificar-pedido-modificacion-recibido-01.md`
- `diagramas/04-diagramas-actividades/04-actividad-modificar-pedido-02.puml`
- `05-secuencia-modificar-pedido-modificacion-recibido-02.puml`
- `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
- `herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md`
- `herramientas-agile/tarjetas-crc/03-tarjeta-crc-producto.md`
- `herramientas-agile/tarjetas-crc/04-tarjeta-crc-personalizacion-item.md`
- `herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md`
- `herramientas-agile/tarjetas-crc/10-tarjeta-crc-historial-pago.md`
- `herramientas-agile/tarjetas-crc/12-tarjeta-crc-historial-cambio-pedido.md`
- `herramientas-agile/tarjetas-crc/13-tarjeta-crc-usuario.md`
- `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`

### CU03 — Cancelar Pedido

- `diagramas/02-casos-de-uso/02-caso-uso-cancelar-pedido-03.puml`
- `03-cancelar-pedido-cancelacion-antes-entrega-01.md`
- `diagramas/04-diagramas-actividades/04-actividad-cancelar-pedido-03.puml`
- `05-secuencia-cancelar-pedido-cancelacion-antes-entrega-03.puml`
- `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
- `herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md`
- `herramientas-agile/tarjetas-crc/11-tarjeta-crc-auditoria-pedido.md`
- `herramientas-agile/tarjetas-crc/13-tarjeta-crc-usuario.md`
- `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`
- `herramientas-agile/tarjetas-crc/16-tarjeta-crc-usuario-encargado.md`



### 2.4. Output obtenido

- CU02 — Modificar Pedido

@startuml
title CU02 - Modificar Pedido
left to right direction
hide circle
skinparam classAttributeIconSize 0

actor "Usuario de Mostrador" as ActorMostrador

class InterfazPedido <<boundary>> {
  +buscarPedido(numeroOReferencia: String): Pedido
  +modificarItems(listaCambios: List<Item_pedido>): void
  +modificarPersonalizaciones(lista: List<Personalizacion_item>): void
  +confirmarModificacion(numeroPedido: Integer): Boolean
  +seleccionarMedioPago(medio: Metodo_pago): Boolean
  +confirmarDevolucion(medio: Metodo_pago): Boolean
}

abstract class Usuario {
  -nombre: String
  -usuario: String
  -passwordHash: String
  -activo: Boolean
}

class UsuarioMostrador {
  -numeroCaja: Integer
  +modificarPedido(pedido: Pedido, cambios: List<Item_pedido>): void
}

class Pedido {
  -fecha: Date
  -estado: Estado_pedido
  -prioritario: Boolean
  -numeroPedido: Integer
  -referenciaRetiro: String
  +buscarPedido(numeroOReferencia: String): Pedido
  +verificarEstado(numeroPedido: Integer): Estado_pedido
  +prepararModificacion(listaCambios: List<Item_pedido>): void
  +prepararPersonalizaciones(lista: List<Personalizacion_item>): void
  +calcularTotal(): Decimal
  +verificarEstadoAlConfirmar(numeroPedido: Integer): Estado_pedido
  +actualizarPedido(numeroPedido: Integer, listaItems: List<Item_pedido>, totalActualizado: Decimal): void
}

class Item_pedido {
  -cantidad: Integer
  -precioUnitario: Decimal
  -subtotal: Decimal
  +modificarCantidadYPrecio(cantidad: Integer, precioUnitario: Decimal): void
  +actualizarPersonalizaciones(lista: List<Personalizacion_item>): void
  +calcularSubtotal(): Decimal
  +agregarPersonalizacion(personalizacion: Personalizacion_item): void
  +quitarPersonalizacion(personalizacion: Personalizacion_item): void
}

abstract class Producto {
  -nombre: String
  -precio: Decimal
  +obtenerNombre(): String
  +obtenerPrecio(): Decimal
}

class Personalizacion_item {
  -descripcion: String
  -costoAdicional: Decimal
  +obtenerDescripcion(): String
  +obtenerCostoAdicional(): Decimal
  +calcularCosto(): Decimal
}

class Pago {
  -monto: Decimal
  -metodoDePago: Metodo_pago
  +calcularDiferencia(totalAnterior: Decimal, totalActualizado: Decimal): Decimal
  +validarMedioPago(medio: Metodo_pago): Boolean
  +registrarCobroAdicional(medio: Metodo_pago, monto: Decimal): void
  +registrarDevolucion(medio: Metodo_pago, monto: Decimal): void
}

class ProcesadorPago <<control>> {
  +validarMedioPago(medio: Metodo_pago): Boolean
}

class Historial_pago {
  -pago: Pago
  -tipo: Tipo_movimiento
  -monto: Decimal
  -motivo: String
  -fechaHora: DateTime
  -usuario: Usuario
  +registrarMovimiento(pago: Pago, tipo: Tipo_movimiento, monto: Decimal, motivo: String, usuario: Usuario): void
}

class Historial_cambio_pedido {
  -pedido: Pedido
  -campo: String
  -valorAnterior: String
  -valorNuevo: String
  -fechaHora: DateTime
  -usuario: Usuario
  +registrarCambio(campo: String, valorAnterior: String, valorNuevo: String, usuario: Usuario): void
}

enum Estado_pedido {
  RECIBIDO
  EN_PREPARACION
  LISTO
  ENTREGADO
  CANCELADO
}

enum Metodo_pago {
  EFECTIVO
  TRANSFERENCIA
}

enum Tipo_movimiento {
  COBRO_INICIAL
  AJUSTE
  DEVOLUCION
}

UsuarioMostrador --|> Usuario

ActorMostrador --> InterfazPedido
InterfazPedido ..> Pedido
InterfazPedido ..> Pago
Pago ..> ProcesadorPago

Pedido "1" *-- "1..*" Item_pedido : items
Item_pedido "0..*" --> "1" Producto : producto
Item_pedido "1" *-- "0..*" Personalizacion_item : personalizaciones

Pedido "1" -- "0..*" Pago : pagos
Pago "1" *-- "0..*" Historial_pago : movimientos
Pedido "1" *-- "0..*" Historial_cambio_pedido : cambios

Historial_pago "0..*" --> "1" Usuario : usuario
Historial_cambio_pedido "0..*" --> "1" Usuario : usuario

Pedido --> Estado_pedido
Pago --> Metodo_pago
Historial_pago --> Tipo_movimiento

@enduml

- CU03 — Cancelar Pedido

@startuml
 title CU03 - Cancelar Pedido
 top to bottom direction
 hide circle
 skinparam classAttributeIconSize 0
 skinparam defaultFontSize 14
 skinparam classFontSize 15
 skinparam classAttributeFontSize 13
 skinparam dpi 160

 abstract class Usuario {
   -nombre: String
   -usuario: String
   -passwordHash: String
   -activo: Boolean
   +autentificar(): Boolean
 }

 class UsuarioMostrador {
   -numeroCaja: Integer
 }

 class UsuarioEncargado {
   -claveSupervision: String
   +autorizarCancelacion(): Boolean
   +solicitarClaveSupervision(numeroPedido: Integer): Boolean
   +validarClaveSupervision(claveSupervision: String): Boolean
 }

 class PantallaPedidos {
   +solicitarCancelacionPedido(numeroPedido: Integer): void
   +confirmarCancelacion(motivo: String): void
   +responderSolicitudCancelacion(numeroPedido: Integer): void
 }

 class ServicioPedidos {
   +consultarPedidoYEstado(numeroPedido: Integer): Pedido
   +procesarCancelacion(numeroPedido: Integer, motivo: String, usuario: Usuario): void
   +procesarCancelacion(numeroPedido: Integer, motivo: String, usuario: Usuario, autorizante: UsuarioEncargado): void
   +solicitarAutorizacion(numeroPedido: Integer): void
 }

 class Pedido {
   -fecha: Date
   -estado: Estado_pedido
   -prioritario: Boolean
   -numeroPedido: Integer
   -referenciaRetiro: String
   +obtenerEstado(numeroPedido: Integer): Estado_pedido
   +cancelarPedido(motivo: String): void
   +cambiarEstado(nuevoEstado: Estado_pedido): void
 }

 class Pago {
   -monto: Decimal
   -metodoDePago: Metodo_pago
 }

 class Auditoria_pedido {
   -accion: String
   -detalles: String
   -fechayhora: DateTime
   +registrarAccion(pedido: Pedido, usuario: Usuario, accion: String, fechaHora: DateTime): void
   +registrarAccion(pedido: Pedido, usuario: Usuario, autorizante: UsuarioEncargado, accion: String, fechaHora: DateTime): void
   +registrarDetalles(motivo: String, estadoAnterior: Estado_pedido, estadoNuevo: Estado_pedido): void
 }

 enum Estado_pedido {
   recibido
   en_preparacion
   listo
   entregado
   cancelado
 }

 enum Metodo_pago {
   efectivo
   transferencia
 }

 UsuarioMostrador --|> Usuario
 UsuarioEncargado --|> Usuario
 UsuarioMostrador ..> PantallaPedidos
 UsuarioEncargado ..> PantallaPedidos
 PantallaPedidos ..> ServicioPedidos
 ServicioPedidos ..> Pedido
 ServicioPedidos ..> Auditoria_pedido
 ServicioPedidos ..> UsuarioEncargado
 Pedido "1" -- "0..*" Pago : pagos
 Pedido "1" -- "0..*" Auditoria_pedido : auditoria
 Auditoria_pedido "0..*" --> "1" Usuario : responsable
 Auditoria_pedido "0..*" --> "0..1" UsuarioEncargado : autorizante
 Pedido --> Estado_pedido
 Pago --> Metodo_pago
@enduml

### 2.5. Ajustes Realizados


* Modificar Pedido — CU02

- **Eliminación del actor gráfico:** se eliminó `ActorMostrador`, manteniendo la clase `UsuarioMostrador` y su relación con `Usuario`.

- **Modificación de `Usuario`:** se incorporó el método `autentificar(): Boolean`, conservando los atributos `nombre`, `usuario`, `passwordHash` y `activo`, para representar la autenticación del usuario.

- **Modificación de `UsuarioMostrador`:** se reemplazó `modificarPedido(pedido: Pedido, cambios: List<Item_pedido>): void` por `modificarPedido(): void`, simplificando su firma.

- **Modificación de `InterfazPedido`:** se agregó `seleccionarPedidoParaModificar(numeroPedido: Integer): void`. También se ajustaron los parámetros de los métodos relacionados con las personalizaciones y los medios de pago, y se modificaron los retornos de `confirmarModificacion()`, `seleccionarMedioPago()` y `confirmarDevolucion()` de `Boolean` a `void`.

- **Modificación de `Pedido`:** se incorporó `modificar(): void` y se eliminó `calcularTotal(): Decimal` del diagrama. 

- **Modificación de `Item_pedido`:** se amplió `calcularSubtotal()` para recibir cantidad, precio unitario y personalizaciones. También se agregó `recalcularPrecio(): Decimal`, para representar el recálculo de los importes.

- **Modificación de `Pago`:** se incorporó `consultarPagoOriginal(numeroPedido: Integer): Pago`, para consultar el pago existente antes de calcular diferencias económicas. 

- **Modificación de `Historial_pago`:** se mantuvieron los atributos y el método `registrarMovimiento()`, pero se ajustó el nombre del atributo temporal de `fechaHora` a `fechayhora`. Se conservó la asociación con `Usuario` para identificar al responsable.

- **Modificación de `Historial_cambio_pedido`:** se reemplazó `registrarCambio()` por `registrarCambioPedido()`. También se ajustaron los nombres de los atributos para registrar los valores anteriores y nuevos.

- **Revisión de `ProcesadorPago`:** se mantuvo el método `validarMedioPago()`, eliminando el estereotipo `<<control>>`. Se conservó su dependencia con `Pago`.

- **Eliminación de estereotipos:** se retiró `<<boundary>>` de `InterfazPedido` y `<<control>>` de `ProcesadorPago`, manteniendo una representación uniforme de las clases.


* Cancelar Pedido — CU03


- **Incorporación del atributo `motivo`:** se agregó `-motivo: String` a la clase `Auditoria_pedido`, para representar explícitamente el motivo de la cancelación y conservarlo como parte del registro de auditoría.

- **Identificación de las interacciones:** se agregó la etiqueta `interactua` a las dependencias de `UsuarioMostrador` y `UsuarioEncargado` hacia `PantallaPedidos`, para aclarar la participación de ambos usuarios en la interfaz.



