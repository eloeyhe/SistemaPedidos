|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |Pago  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Registrar el cobro efectuado una vez aprobado por ProcesadorPago (soy el único pago original del pedido) | Pedido, MetodoPago | Soy la transacción financiera y capturo la forma de pago; no valido medios de pago, eso lo hace ProcesadorPago | metodoDePago |
| Calcular diferencias de dinero asociadas a la transacción | HistorialPago | Conozco el monto abonado para respaldar la orden | monto |
| Registrar cobros adicionales y devoluciones de una modificación como movimientos AJUSTE / DEVOLUCION | HistorialPago, TipoMovimiento | | |
|  |  |  |  |