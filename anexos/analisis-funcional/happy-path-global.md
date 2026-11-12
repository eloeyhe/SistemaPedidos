# Pseudocódigo - Happy Path Global del Sistema

---

## 1. Escenario Elegido

**Escenario:** Un cliente pide en el mostrador un combo y un producto personalizado, paga por transferencia, agrega un producto antes de que se prepare y paga la diferencia. Cocina prepara el pedido y el mostrador lo entrega. Es el escenario más representativo porque recorre el ciclo de vida completo de un pedido, desde su registro hasta su entrega, y en cada paso intervienen los tres tipos de usuario del flujo normal y casi todas las clases del dominio.

**Casos de uso involucrados:** CU1 (Tomar Pedido), CU2 (Modificar Pedido) y CU4 (Cambiar Estado de Pedido)

**Clases participantes:**

- UsuarioMostrador
- UsuarioCocina
- SistemaPedidos
- PantallaCocina
- ProcesadorPago
- Pedido
- Item_pedido
- Producto (ProductoPreparado)
- Combo
- Personalizacion_item
- Pago
- Historial_pago
- Historial_cambio_pedido
- Estado_pedido
- Metodo_pago

---

## 2. Pseudocódigo

```text
INICIO Sistema de Pedidos - Happy Path Global

// ============================================================
// CU1 - Tomar Pedido
// ============================================================

// Un cliente llega al mostrador y el usuario de mostrador abre un pedido nuevo.
UsuarioMostrador mostrador = nuevo UsuarioMostrador("Laura", "lparedes", "hash123", verdadero, 2)
SistemaPedidos sistema = nuevo SistemaPedidos()
PantallaCocina pantallaCocina = nuevo PantallaCocina()
ProcesadorPago procesador = nuevo ProcesadorPago()

// El sistema genera el número único y registra la fecha y hora (RF6).
Pedido pedido = sistema.iniciarNuevoPedido()
numeroPedido = 1042            // número único asignado por el sistema
fechaHora = fechaHoraActual    // fecha y hora de registro

// El cliente pide un nombre para el llamado al retirar.
sistema.ingresarReferenciaRetiro("Carla")

// El cliente elige una hamburguesa (producto preparado) y un combo; el combo usa su precio propio (RF1).
ProductoPreparado hamburguesa = nuevo ProductoPreparado("Hamburguesa completa", 5500.00, 12)
Combo comboFamiliar = nuevo Combo("Combo familiar", 14000.00)
sistema.seleccionarProducto(hamburguesa, 1)
sistema.seleccionarCombo(comboFamiliar, 1)

// La hamburguesa se pide con un ingrediente extra, que suma un costo adicional.
sistema.ingresarPersonalizacion("Extra queso")

// Cada ítem calcula su subtotal y el pedido recalcula el total.
total = pedido.obtenerTotal()

// El cliente prefiere pagar por transferencia: se valida el medio antes de cobrar (RF8).
medio = Metodo_pago.TRANSFERENCIA
medioValido = procesador.validarMedioPago(medio)
SI medioValido ENTONCES
    Pago pago = nuevo Pago(total, medio, fechaHoraActual)
    cobroAprobado = pago.registrarCobro(medio, total)
    pedido.asociarPago(pago)
FIN SI

// El pedido queda registrado como RECIBIDO y se envía a la pantalla de Cocina (RF2).
listaItems = ítems cargados en el pedido (hamburguesa y combo)
registrado = pedido.registrarPedido(numeroPedido, fechaHora, "Carla", listaItems, total, pago)
estadoActual = Estado_pedido.RECIBIDO
pantallaCocina.enviarCabeceraPedido(numeroPedido, fechaHora, "Carla")
pantallaCocina.enviarDetallePedido(listaItems, total)
confirmado = pantallaCocina.informarPedidoConfirmado(numeroPedido)

// ============================================================
// CU2 - Modificar Pedido (el pedido sigue en estado RECIBIDO)
// ============================================================

// El cliente se arrepiente y quiere sumar una bebida antes de que Cocina empiece (RF4).
// Solo se puede modificar mientras el estado es RECIBIDO.
SI estadoActual == Estado_pedido.RECIBIDO ENTONCES
    mostrador.modificarPedido(pedido)
    Producto bebida = nuevo ProductoEnvasado("Gaseosa 500 cc", 1800.00, fechaElaboracion, fechaVencimiento)
    pedido.agregarItem(bebida, 1)

    // El sistema recalcula el total y obtiene la diferencia contra lo ya abonado (RF8).
    totalNuevo = pedido.obtenerTotal()
    diferencia = pago.calcularDiferencia(totalNuevo)

    // La diferencia es a favor del negocio: se cobra y queda registrada para auditoría.
    SI diferencia > 0 ENTONCES
        procesador.validarMedioPago(Metodo_pago.EFECTIVO)
        pago.registrarCobro(Metodo_pago.EFECTIVO, diferencia)
        Historial_pago historialPago = nuevo Historial_pago()
        historialPago.registrarMovimiento(pago, Tipo_movimiento.AJUSTE, diferencia, "Se agregó una bebida", mostrador)
    FIN SI

    // Queda trazado qué campo cambió, quién lo cambió y su valor anterior y nuevo.
    Historial_cambio_pedido historialCambios = nuevo Historial_cambio_pedido()
    historialCambios.registrarCambioPedido(pedido, "total", total, totalNuevo, mostrador)
FIN SI

// ============================================================
// CU4 - Cambiar Estado de Pedido
// ============================================================

// Cocina toma el pedido y empieza a prepararlo: pasa de RECIBIDO a EN_PREPARACION (RF3).
UsuarioCocina cocina = nuevo UsuarioCocina("Mario", "mrios", "hash456", verdadero, "Parrilla")
cocina.comenzarPreparacion(pedido)
hamburguesa.iniciarPreparacion()

// Cuando termina la elaboración, Cocina lo marca como LISTO.
cocina.marcarPedidoListo(pedido)

// El usuario de mostrador llama a "Carla", le entrega el pedido y lo marca como ENTREGADO.
mostrador.marcarComoEntregado(pedido)

// Estado final del sistema: el pedido queda guardado con todo su historial, pagos y cambios.
Retornar Estado_pedido.ENTREGADO

FIN
```

---

## 3. Trazabilidad del Pseudocódigo

| Bloque | Caso de uso | Clases involucradas | Diagrama de secuencia de referencia |
|--------|-------------|---------------------|-------------------------------------|
| Registro del pedido, cobro y envío a Cocina | CU1 | UsuarioMostrador, SistemaPedidos, Pedido, Item_pedido, Producto, Combo, Personalizacion_item, Pago, ProcesadorPago, PantallaCocina | [Secuencia - Tomar Pedido](../../diagramas/05-diagramas-secuencia/05-secuencia-tomar-pedido-registro-de-pedido-01.png) |
| Modificación del pedido, cobro de la diferencia y registro de cambios | CU2 | UsuarioMostrador, Pedido, Item_pedido, Producto, Pago, ProcesadorPago, Historial_pago, Historial_cambio_pedido | [Secuencia - Modificar Pedido](../../diagramas/05-diagramas-secuencia/05-secuencia-modificar-pedido-modificacion-recibido-02.png) |
| Preparación, listo y entrega | CU4 | UsuarioCocina, UsuarioMostrador, Pedido, ProductoPreparado, Estado_pedido | [Secuencia - Cambiar Estado de Pedido](../../diagramas/05-diagramas-secuencia/05-secuencia-cambiar-estado-pedido-avance-preparacion-04.png) |
