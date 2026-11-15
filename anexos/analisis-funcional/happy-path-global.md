# Pseudocódigo - Happy Path Global del Sistema

---

## 1. Escenario Elegido

**Escenario:** Un cliente pide en el mostrador un combo y un producto personalizado, paga por transferencia, agrega un producto antes de que se prepare y paga la diferencia. Cocina prepara el pedido y el mostrador lo entrega. Es el escenario más representativo porque recorre el ciclo de vida completo de un pedido, desde su registro hasta su entrega, y en cada paso intervienen los dos tipos de usuario del flujo normal (Usuario de Mostrador y Usuario de Cocina) y casi todas las clases del dominio.

**Casos de uso involucrados:** CU1 (Tomar Pedido), CU2 (Modificar Pedido) y CU4 (Cambiar Estado de Pedido)

**Clases participantes:**

- UsuarioMostrador
- UsuarioCocina
- ServicioPedidos
- PantallaCocina
- ProcesadorPago
- Pedido
- ItemPedido
- ProductoPreparado (subclase de Producto)
- ProductoEnvasado (subclase de Producto)
- Combo
- PersonalizacionItem
- Pago
- HistorialPago
- HistorialCambioPedido
- EstadoPedido
- MetodoPago
- TipoMovimiento

---

## 2. Pseudocódigo

```text
INICIO Sistema de Pedidos - Happy Path Global

// ============================================================
// CU1 - Tomar Pedido
// ============================================================

// Un cliente llega al mostrador y el usuario de mostrador abre un pedido nuevo.
UsuarioMostrador mostrador = nuevo UsuarioMostrador("Laura", "lparedes", "hash123", verdadero, 2)
ServicioPedidos servicio = nuevo ServicioPedidos()
PantallaCocina pantallaCocina = nuevo PantallaCocina()
ProcesadorPago procesador = nuevo ProcesadorPago()

// El sistema crea el pedido, genera el número único y registra la fecha y hora (RF6).
Pedido pedido = servicio.iniciarNuevoPedido()
numeroPedido = 1042                // número único generado por el servicio
pedido.asignarIdentificador(numeroPedido, fechaHoraActual)

// El cliente pide un nombre para el llamado al retirar.
servicio.ingresarReferenciaRetiro("Carla")
pedido.asignarReferenciaRetiro("Carla")

// El cliente elige una hamburguesa (producto preparado) y un combo; el combo usa su precio propio (RF1).
ProductoPreparado hamburguesa = nuevo ProductoPreparado("Hamburguesa completa", 5500.00, 12)
Combo comboFamiliar = nuevo Combo("Combo familiar", 14000.00)
servicio.seleccionarProducto(hamburguesa, 1)
servicio.seleccionarCombo(comboFamiliar, 1)
ItemPedido itemHamburguesa = pedido.agregarItem(hamburguesa, 1)
ItemPedido itemCombo = pedido.agregarItemCombo(comboFamiliar, 1)

// La hamburguesa se pide con un ingrediente extra, que suma un costo adicional.
servicio.ingresarPersonalizacion("Extra queso")
PersonalizacionItem extra = itemHamburguesa.agregarPersonalizacion("Extra queso")
costoExtra = extra.calcularCosto()

// Cada ítem calcula su subtotal y el pedido recalcula el total.
itemHamburguesa.calcularSubtotal()
itemCombo.calcularSubtotal()
total = pedido.actualizarTotal()

// El cliente prefiere pagar por transferencia: se valida el medio antes de cobrar (RF8).
medio = MetodoPago.TRANSFERENCIA
medioValido = procesador.validarMedioPago(medio)
SI medioValido ENTONCES
    cobroAprobado = procesador.procesarCobro(medio, total)
    Pago pago = nuevo Pago(total, medio, fechaHoraActual)
    pago.registrarCobro(medio, total)
    HistorialPago historialPago = nuevo HistorialPago()
    historialPago.registrarMovimiento(pago, TipoMovimiento.COBRO_INICIAL, total, "Cobro del pedido", mostrador)
FIN SI

// El pedido queda confirmado en estado RECIBIDO y se envía a la pantalla de Cocina (RF2).
confirmado = servicio.confirmarPedido(medio)
estadoActual = pedido.obtenerEstado()          // EstadoPedido.RECIBIDO
pantallaCocina.enviarCabeceraPedido(numeroPedido, fechaHoraActual, "Carla")
pantallaCocina.enviarDetallePedido(pedido.obtenerDetalle(), total)
pantallaCocina.informarPedidoConfirmado(numeroPedido)

// ============================================================
// CU2 - Modificar Pedido (el pedido sigue en estado RECIBIDO)
// ============================================================

// El cliente se arrepiente y quiere sumar una bebida antes de que Cocina empiece (RF4).
// Solo se puede modificar mientras el estado es RECIBIDO.
SI estadoActual == EstadoPedido.RECIBIDO ENTONCES
    mostrador.modificarPedido(pedido)
    servicio.modificarPedido(numeroPedido)
    pedido.modificar()

    ProductoEnvasado bebida = nuevo ProductoEnvasado("Gaseosa 500 cc", 1800.00, fechaElaboracion, fechaVencimiento)
    pedido.agregarItem(bebida, 1)

    // El sistema recalcula el total y obtiene la diferencia contra lo ya abonado (RF8).
    totalNuevo = pedido.actualizarTotal()
    diferencia = pago.calcularDiferencia(totalNuevo)

    // La diferencia es a favor del negocio: se cobra y queda registrada para auditoría.
    SI diferencia > 0 ENTONCES
        procesador.validarMedioPago(MetodoPago.EFECTIVO)
        procesador.procesarCobro(MetodoPago.EFECTIVO, diferencia)
        pago.registrarCobroAdicional(MetodoPago.EFECTIVO, diferencia)
        historialPago.registrarMovimiento(pago, TipoMovimiento.AJUSTE, diferencia, "Se agregó una bebida", mostrador)
    FIN SI

    // Queda trazado qué campo cambió, quién lo cambió y su valor anterior y nuevo.
    HistorialCambioPedido historialCambios = nuevo HistorialCambioPedido()
    historialCambios.registrarCambioPedido(pedido, "total", total, totalNuevo, mostrador)
FIN SI

// ============================================================
// CU4 - Cambiar Estado de Pedido
// ============================================================

// Cocina toma el pedido y empieza a prepararlo: pasa de RECIBIDO a EN_PREPARACION (RF3).
UsuarioCocina cocina = nuevo UsuarioCocina("Mario", "mrios", "hash456", verdadero, "Parrilla")
transicionValida = pedido.validarTransicion(EstadoPedido.EN_PREPARACION, cocina)
SI transicionValida ENTONCES
    servicio.solicitarCambioEstado(numeroPedido, EstadoPedido.EN_PREPARACION, cocina)
    cocina.iniciarPreparacion(pedido)
    pedido.cambiarEstado(EstadoPedido.EN_PREPARACION)
    historialCambios.registrarCambioEstado(pedido, EstadoPedido.RECIBIDO, EstadoPedido.EN_PREPARACION, cocina)
    hamburguesa.iniciarPreparacion()
FIN SI

// Cuando termina la elaboración, Cocina lo marca como LISTO.
cocina.marcarPedidoListo(pedido)
pedido.cambiarEstado(EstadoPedido.LISTO)
historialCambios.registrarCambioEstado(pedido, EstadoPedido.EN_PREPARACION, EstadoPedido.LISTO, cocina)

// El usuario de mostrador llama a "Carla", le entrega el pedido y lo marca como ENTREGADO.
mostrador.marcarComoEntregado(pedido)
pedido.cambiarEstado(EstadoPedido.ENTREGADO)
historialCambios.registrarCambioEstado(pedido, EstadoPedido.LISTO, EstadoPedido.ENTREGADO, mostrador)

// Estado final del sistema: el pedido queda guardado con todo su historial, pagos y cambios.
Retornar pedido.obtenerEstado()                // EstadoPedido.ENTREGADO

FIN
```

---

## 3. Trazabilidad del Pseudocódigo

| Bloque | Caso de uso | Clases involucradas | Diagrama de secuencia de referencia |
|--------|-------------|---------------------|-------------------------------------|
| Registro del pedido, cobro y envío a Cocina | CU1 | UsuarioMostrador, ServicioPedidos, Pedido, ItemPedido, ProductoPreparado, Combo, PersonalizacionItem, Pago, HistorialPago, ProcesadorPago, PantallaCocina | [Secuencia - Tomar Pedido](../../diagramas/05-diagramas-secuencia/05-secuencia-tomar-pedido-registro-de-pedido-01.png) |
| Modificación del pedido, cobro de la diferencia y registro de cambios | CU2 | UsuarioMostrador, ServicioPedidos, Pedido, ProductoEnvasado, Pago, ProcesadorPago, HistorialPago, HistorialCambioPedido | [Secuencia - Modificar Pedido](../../diagramas/05-diagramas-secuencia/05-secuencia-modificar-pedido-modificacion-recibido-02.png) |
| Preparación, listo y entrega | CU4 | UsuarioCocina, UsuarioMostrador, ServicioPedidos, Pedido, ProductoPreparado, EstadoPedido, HistorialCambioPedido | [Secuencia - Cambiar Estado de Pedido](../../diagramas/05-diagramas-secuencia/05-secuencia-cambiar-estado-pedido-avance-preparacion-04.png) |
