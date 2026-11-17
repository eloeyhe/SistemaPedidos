|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** |ProcesadorPago  | | |
| **Superclase:** |Ninguna | | |
| **Subclase:** |Ninguna | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Validar que el medio de pago esté habilitado (efectivo o transferencia); soy el único validador, invocado por ServicioPedidos en CU1 y CU2 | ServicioPedidos, MetodoPago | Soy el intermediario con el medio de cobro: respondo si un pago se aprueba o se rechaza y por qué | Ninguna |
| Procesar el cobro (inicial o ajuste) e informar el motivo de rechazo cuando no se aprueba | ServicioPedidos | | |
|  |  |  |  |
