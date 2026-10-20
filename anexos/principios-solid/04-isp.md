# Principio de Segregación de Interfaces (ISP)

## Propósito y Tipo del Principio SOLID
El **Principio de Segregación de Interfaces (Interface Segregation Principle - ISP)** es el cuarto principio dentro de los principios SOLID de diseño orientado a objetos.
Su propósito principal es establecer que **ninguna clase debe verse obligada a depender de métodos o interfaces que no utiliza**. En lugar de diseñar interfaces monolíticas ("gordas" o *fat interfaces*), el principio promueve la creación de interfaces pequeñas, cohesivas y especializadas por rol.

---

## Motivación
En el modelo del **Sistema de Pedidos**, observamos diversos tipos de usuarios operativos que heredan de la clase abstracta `Usuario` (`UsuarioMostrador`, `UsuarioCocina`, `UsuarioEncargado`) y diversas entidades que gestionan registros y trazabilidad (`Historial_pago`, `Auditoria_pedido`, `Historial_cambio_pedido`).

### Problema Identificado en el Proyecto
Si definiéramos una única interfaz monolítica de gestión operativa (por ejemplo, `IProcesamientoSistema` o `IGestionOperativa`) para ser implementada por los usuarios del sistema, esta debería contener métodos como:
- `registrarPedido()`
- `modificarPedido()`
- `cancelarPedido()`
- `marcarComoEntregado()`
- `consultarPedidosListos()`
- `comenzarPreparacion()`
- `marcarPedidoListo()`
- `consultarComandasActivas()`
- `autorizarCancelacion()`
- `registrarDevolucionManual()`

Al intentar aplicar esta interfaz "gorda":
1. **`UsuarioCocina`** solo necesita preparar los pedidos y consultar las comandas activas (`comenzarPreparacion()`, `marcarPedidoListo()`, `consultarComandasActivas()`). Al obligarlo a implementar una interfaz monolítica, se vería forzado a incluir métodos vacíos o lanzar excepciones para las otras 7 operaciones, como `autorizarCancelacion()` o `registrarPedido()`.
2. **`UsuarioMostrador`** se encarga de la atención al cliente y cobro (`registrarPedido()`, `modificarPedido()`, `cancelarPedido()`, `marcarComoEntregado()`) y de consultar los pedidos listos para entregar (`consultarPedidosListos()`), pero no tiene la responsabilidad de autorizar cancelaciones ni supervisar estaciones de cocina, por lo que quedaría obligado a implementar otras 5 operaciones que no le corresponden.
3. **`UsuarioEncargado`** participa en la cancelación de pedidos (`cancelarPedido()`) y realiza tareas avanzadas de supervisión como `autorizarCancelacion()` y `registrarDevolucionManual()`, y tendría que implementar las otras 7 operaciones sin necesitarlas.

### Ejemplo del problema (pseudocódigo)
Con la interfaz monolítica, `UsuarioCocina` terminaría escribiendo código como el siguiente:

```text
clase UsuarioCocina implementa IGestionOperativa:

    comenzarPreparacion(idPedido)       -> lógica real
    marcarPedidoListo(idPedido)         -> lógica real
    consultarComandasActivas()          -> lógica real

    registrarPedido(...)                -> lanzar NotImplementedException
    modificarPedido(...)                -> lanzar NotImplementedException
    cancelarPedido(...)                 -> lanzar NotImplementedException
    marcarComoEntregado(...)            -> lanzar NotImplementedException
    consultarPedidosListos()            -> lanzar NotImplementedException
    autorizarCancelacion(...)           -> lanzar NotImplementedException
    registrarDevolucionManual(...)      -> lanzar NotImplementedException
```

Cualquier código que reciba una `IGestionOperativa` podría invocar uno de esos métodos y provocar un error en tiempo de ejecución. Segregar las interfaces elimina ese riesgo desde el diseño.

### Comparación: antes y después

| Clase | Métodos exigidos por la interfaz gorda | Métodos que realmente usa | Métodos innecesarios | Con interfaces segregadas |
| :---- | :-: | :-: | :-: | :------------------------ |
| `UsuarioMostrador` | 10 | 5 | 5 | Implementa `IAtencionCliente`, `ICancelacionPedido` e `IConsultaEntregas` (5 métodos) |
| `UsuarioCocina` | 10 | 3 | 7 | Implementa `IPreparacionCocina` e `IConsultaComandas` (3 métodos) |
| `UsuarioEncargado` | 10 | 3 | 7 | Implementa `ICancelacionPedido` e `ISupervision` (3 métodos) |

### Registros y trazabilidad: por qué no se unifican
Las clases `Historial_pago`, `Auditoria_pedido` e `Historial_cambio_pedido` registran hechos distintos: movimientos de dinero, acciones realizadas sobre un pedido y cambios de campos de un pedido, respectivamente. Aunque las tres exponen una operación de registro, **compartir un nombre de método no justifica una interfaz común**. Unificarlas bajo un contrato genérico (por ejemplo, `IRegistrable`) no aportaría beneficio alguno, porque ningún componente del sistema necesita tratarlas de forma polimórfica, y las obligaría a depender de un contrato que no refleja la naturaleza distinta de cada registro.

Por eso, **no se crea una interfaz para estas clases**: cada una conserva su propia operación cohesiva. Si en el futuro surgiera un consumidor real que necesite usar varias de ellas mediante el mismo contrato, se podría introducir una interfaz específica en ese momento, y no antes.

---

## Explicación de Interfaces
En la Programación Orientada a Objetos (POO), una **interfaz** es un contrato abstracto que define *qué* operaciones debe ofrecer un objeto que la implemente, sin imponer cómo debe llevarlas a cabo.

Para cumplir con el principio ISP en nuestro sistema, las interfaces operan como **roles específicos**. En lugar de obligar a que las subclases de `Usuario` asuman contratos generales con métodos irrelevantes, diseñamos interfaces segregadas agrupando comportamientos cohesivos. Además, ISP no implica crear una interfaz por cada clase: una abstracción solo se justifica cuando existe una necesidad real de usar distintas clases mediante el mismo contrato.

---

## Estructura de Clases

Para resolver la rigidez del diseño, se segregaron las responsabilidades de los usuarios en interfaces pequeñas y especializadas:
- **`IAtencionCliente`**: Define las operaciones de atención en caja o mostrador (`registrarPedido()`, `modificarPedido()`, `marcarComoEntregado()`).
- **`ICancelacionPedido`**: Define la operación de cancelación de un pedido (`cancelarPedido()`), que el caso de uso *Cancelar pedido* (CU03) asigna tanto al usuario de mostrador como al encargado. Se separó de `IAtencionCliente` y de `ISupervision` para que cada actor dependa únicamente de la cancelación, sin cargar con las demás funciones de atención al cliente ni de supervisión.
- **`IConsultaEntregas`**: Permite al mostrador consultar los pedidos que ya están listos para entregar (`consultarPedidosListos()`).
- **`IPreparacionCocina`**: Contiene exclusivamente las operaciones de preparación en cocina (`comenzarPreparacion()`, `marcarPedidoListo()`).
- **`IConsultaComandas`**: Permite a la cocina consultar las comandas activas pendientes de preparación