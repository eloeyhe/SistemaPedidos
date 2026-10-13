# Informe de Interacción con IA - Especialista en SRP 

**Rol:** Especialista en Principios de Extensión (OCP + LSP) 

**Herramienta utilizada:** GitHub Gemini Extension Terminal


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
1. Principio de Sustitución de Liskov (LSP - Liskov Substitution Principle)

> «Si S es un subtipo de T, los objetos de tipo T pueden ser sustituidos por
objetos de tipo S sin alterar ninguna de las propiedades deseables del programa
(corrección, tarea realizada, etc.).»
> 

Analizando las jerarquías de herencia propuestas en el diagrama y en las tarjetas
CRC, se obtienen las siguientes conclusiones:

Jerarquía 1: Producto → ProductoPreparado, ProductoEnvasado

- Estructura:
- Superclase: Producto {abstract} (nombre, precio, cambiarPrecio()).
- Subclase: ProductoPreparado (añade tiempoPreparacion, iniciarPreparacion()).
- Subclase: ProductoEnvasado (añade fechaElaboracion, fechaVencimiento).
- Verificación de Sustituibilidad:
- En el contexto comercial y de cálculo (Item_pedido / Pedido):
- Cumple LSP: Tanto ProductoPreparado como ProductoEnvasado responden a las propiedades de la superclase Producto (nombre, precio, cambiarPrecio()). Un Item_pedido puede recibir indistintamente cualquiera de los dos y calcular su subtotal sin conocer el subtipo concreto.
- En el contexto operativo y de elaboración (Cocina / Pantalla de Comandas):
- Riesgo latente de violación de LSP:
- En la tarjeta 07-tarjeta-crc-producto-preparado md, la responsabilidad asignada es "Iniciar la preparación e indicar su duración", mientras que un ProductoEnvasado no requiere elaboración (está listo para entregar).
- Si la superclase Producto llegara a incorporar un contrato operativo del tipo iniciarPreparacion(), ProductoEnvasado no podría cumplirlo y se vería obligado a lanzar una excepción (ej. UnsupportedOperationException) o devolver valores ficticios, violando flagrantemente LSP.
- Por otro lado, si iniciarPreparacion() existe únicamente en ProductoPreparado, cualquier componente que manipule una lista genérica de Producto se verá forzado a romper el polimorfismo recurriendo a instanceof y downcasting:

1         if (producto instanceof ProductoPreparado) {
2             ((ProductoPreparado) producto).iniciarPreparacion();
3         }

- Recomendación para blindar LSP:
- Segregar responsabilidades mediante interfaces (ISP). Mantener en Producto exclusivamente los atributos del catálogo comercial.
- Para los ítems que requieran cocina, implementar una interfaz de capacidad: Elaborable o PreparableEnCocina con getTiempoPreparacion(). De este modo,
Cocina opera sobre abstracciones Preparable y no fuerza a ProductoEnvasado a rechazar contratos heredados.

Jerarquía 2: Usuario → UsuarioMostrador, UsuarioCocina, UsuarioEncargado

- Estructura: 
- Superclase: Usuario {abstract} (nombre, usuario, passwordHash, activo, autentificar()).
- Subclase: UsuarioMostrador (añade numeroCaja, registrarPedido(), modificarPedido(), marcarComoEntregado()).
- Subclase: UsuarioCocina (añade estacionTrabajo, comenzarPreparacion(), marcarPedidoListo()).
- Subclase: UsuarioEncargado (añade claveSupervision, autorizarCancelacion(), registrarDevolucionManual()).
- Verificación de Sustituibilidad:
- Falla estructural grave respecto a LSP:
1. Asimetría de contratos entre subtipos:
- Las operaciones de negocio (registrarPedido, comenzarPreparacion, autorizarCancelacion) se colocaron como métodos exclusivos de cada subclase y no están presentes en la superclase Usuario.
- Si un módulo recibe un Usuario genérico (por ejemplo, en Auditoria_pedido.usuario: Usuario o en la sesión activa), no puede invocar ninguna operación de negocio polimórficamente sin hacer conversiones de tipo forzadas (downcasting).
2. Imposibilidad de sustitución bajo un contrato unificado:
- Si se intentara elevar dichos métodos a la superclase Usuario para permitir el polimorfismo, las subclases violarían directamente LSP: UsuarioCocina rompería el contrato de registrarPedido() (lanzando excepciones de denegación o realizando operaciones vacías), y UsuarioMostrador no podría cumplir comenzarPreparacion().
3. Conflicto directo con el Requisito No Funcional RNF5:
- El documento de introducción especifica en RNF5: «...sin imponer restricciones rígidas de acceso o bloqueos basados en roles que entorpezcan la dinámica del negocio donde cualquiera de los operarios puede cobrar o tomar pedidos según la necesidad».
- Modelar los roles como subclases fijas de Usuario hace imposible que unmismo empleado tome pedidos en mostrador y luego colabore en la preparación sin alterar la identidad del objeto en tiempo de ejecución.
- Recomendación para corregir LSP:
- Eliminar la jerarquía de herencia de Usuario: Aplicar composición en lugar de herencia (favor composition over inheritance).
- Usuario debe ser una única clase concreta orientada a autenticación y trazabilidad.
- Las capacidades o roles se modelan a través de una colección de roles o permisos (Rol: Mostrador, Cocina, Encargado).
- Las acciones operativas (registrarPedido, marcarListo, etc.) no deben ser métodos de la entidad Usuario, sino responsabilidades de controladores/servicios de dominio del sistema (ej. PedidoService), los cuales simplemente verifican si el usuario autenticado posee el permiso requerido.
```

## 4. Ajustes realizados:

  * La única modificación fue de sintaxis. La IA agregaba el prefijo 'id' delante de los atributos, lo cual no es correcto para nuestro caso, por lo que se eliminó. 