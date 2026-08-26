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
