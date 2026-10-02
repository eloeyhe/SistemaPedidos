# Registro de uso de Copilot Agent Mode - Modelador de Diagramas de Casos de Uso

**Rol:** Modelador de Diagramas de Casos de Uso UML
**Autor:** Feijo Agustin

## 1. Contexto utilizado

Se utilizo `anexos/introduccion.md` como Anexo A1. El documento contiene los actores, requisitos funcionales y casos de uso aprobados del sistema de gestion de pedidos.

## 2. Prompt utilizado

> Actua como un Modelador de Diagramas de Casos de Uso UML senior para un sistema de gestion de pedidos en un local gastronomico.
>
> Lee `anexos/introduccion.md` como contexto. Genera el codigo PlantUML completo para cinco casos de uso: CU01 Tomar Pedido, CU02 Modificar Pedido, CU03 Cancelar Pedido, CU04 Cambiar Estado Pedido y CU05 Consultar Pedidos Activos.
>
> Usa los actores Cliente, Usuario de Mostrador, Cocina y Encargado segun corresponda. El Cliente interactua de manera indirecta a traves del Usuario de Mostrador: no lo asocies directamente a ningun caso de uso y representalo solo como actor externo relacionado conceptualmente con el Mostrador.
>
> Integra la priorizacion manual dentro de CU01 y su visualizacion/ordenamiento dentro de CU05. Integra el registro de pago dentro de CU01 y los ajustes de cobro dentro de CU02. No generes casos de uso independientes llamados Priorizar Pedido ni Registrar Pago.
>
> En CU04 diferencia explicitamente las transiciones realizadas por Cocina (RECIBIDO -> EN_PREPARACION y EN_PREPARACION -> LISTO) de la realizada por Usuario de Mostrador (LISTO -> ENTREGADO). Considera CANCELADO como estado final y conserva el registro para auditoria.
>
> Para cada diagrama entrega un bloque `@startuml ... @enduml`, con asociaciones de actores y relaciones `<<include>>`/`<<extend>>` coherentes.

## 3. Respuesta: codigo PlantUML generado

Los cinco bloques completos se encuentran en sus archivos fuente versionados y se reproducen a continuacion.

- [02-caso-uso-tomar-pedido-01.puml](../../diagramas/02-casos-de-uso/02-caso-uso-tomar-pedido-01.puml)
- [02-caso-uso-modificar-pedido-02.puml](../../diagramas/02-casos-de-uso/02-caso-uso-modificar-pedido-02.puml)
- [02-caso-uso-cancelar-pedido-03.puml](../../diagramas/02-casos-de-uso/02-caso-uso-cancelar-pedido-03.puml)
- [02-caso-uso-cambiar-estado-pedido-04.puml](../../diagramas/02-casos-de-uso/02-caso-uso-cambiar-estado-pedido-04.puml)
- [02-caso-uso-consultar-pedidos-activos-05.puml](../../diagramas/02-casos-de-uso/02-caso-uso-consultar-pedidos-activos-05.puml)

- Se conservaron los actores aprobados y sus responsabilidades: Usuario de Mostrador, Cocina y Encargado como actores del sistema, y Cliente como actor externo indirecto.
- Se mantuvieron los cinco casos de uso del alcance A1 y los estados RECIBIDO, EN_PREPARACION, LISTO, ENTREGADO y CANCELADO.
- Se conservaron las relaciones `<<include>>` para pasos obligatorios y `<<extend>>` para alternativas, errores o condiciones opcionales.
- Se mantuvieron los flujos de seleccion de productos, personalizaciones, recalculo y validacion de estado.

### Se adapto o cambio

- `Crear Pedido` se renombro `Tomar Pedido` para coincidir con CU01 aprobado.
- `Personal de Cocina` se normalizo a `Cocina`.
- La asociacion directa del Cliente con casos de uso se elimino. Se dejo solamente una relacion conceptual Cliente--Mostrador etiquetada como solicitud indirecta.
- `Registrar pago` se integro dentro de CU01 como `Registrar cobro`; sus ajustes se integraron dentro de CU02 como `Registrar ajuste de cobro`.
- La priorizacion se integro dentro de CU01 como una extension opcional y su visualizacion/ordenamiento se incorporo a CU05.
- CU04 ahora separa explicitamente las transiciones de Cocina y Mostrador mediante asociaciones distintas.
- CU03 explicita el cambio definitivo a CANCELADO y la conservacion del registro para auditoria.

### Se descarto

- Se descartaron los casos de uso independientes `Priorizar Pedido` y `Registrar Pago`, porque A1 los define como subprocesos integrados.
- Se descartaron las asociaciones del Cliente a `Tomar Pedido`, `Modificar Pedido` y cualquier otro caso de uso.
- Se descarto el actor generico `Admin`, que no forma parte de los actores aprobados.
- Los cinco fuentes PlantUML anteriores quedaron reemplazados para que no permanezcan diagramas obsoletos con nombres y responsabilidades contradictorias.

### Correspondencia entre actor, clase y escenario (RC5)

En CU01 y CU02 se documenta la correspondencia sin equiparar los conceptos:

- **Actor (casos de uso):** `Cliente` es una persona externa que pide cambios a traves de Mostrador, sin acceso directo al sistema ni asociacion a casos de uso. El actor representa un rol de interaccion, no una clase.
- **Clase (boceto inicial):** `Cliente` **si existe** en `01-boceto-inicial.excalidraw`, con `nombre` y operaciones `crearPedido()`, `cancelarPedido()` y `modificarPedido()`. Ese boceto de diseno no convierte al Cliente en usuario del sistema: RF6 indica que el MVP identifica el pedido por referencia de retiro y no exige una ficha persistida ni datos personales del cliente. La correspondencia entre la clase y la implementacion MVP queda sujeta a la evolucion del diseno; no se infiere persistencia por el solo hecho de figurar en el boceto.
- **Escenarios:** CU01 y CU02 indican que el Cliente solicita la operacion indirectamente y que Mostrador es quien interactua con el sistema.

La nota junto a `Cliente` en CU01 y CU02 resume esta distincion en las imagenes.

### Autorizacion de cancelacion especial (RC7)

En CU03, el actor `Encargado` representa al personal cuya responsabilidad `autorizarCancelacion()` figura en la clase `UsuarioEncargado` del boceto. Mostrador inicia la cancelacion; si el estado es `EN_PREPARACION`, solo el Encargado autoriza continuar antes de confirmar y registrar `CANCELADO`. Si deniega, no se modifica el pedido. Los estados `LISTO` y `ENTREGADO` no se cancelan por el flujo automatico; una devolucion manual del Encargado es un proceso separado. El escenario CU03-01 explicita esta secuencia y el diagrama anota la condicion de la extension.
