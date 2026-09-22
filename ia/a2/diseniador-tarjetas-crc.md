# Registro de Code Reviews asistidas con IA - Diseñador de Tarjetas CRC
## Code Review 1: PR #69 - [Diseñador de Tarjetas CRC /diseniador-tarjetas-crc-add-tarjeta-clase-1]
* **Rama trabajada:** `future/diseniador-tarjetas-crc-add-tarjeta-clase-1`
* **Autor:** Eloy Eyheramendy

### 1. Prompt utilizado
&gt; "Actúa como un Arquitecto de Software experto en Diseño Orientado a Objetos. Necesito que analices nuestro sistema leyendo los archivos de contexto: `anexos/introduccion.md` y `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`.

A partir de este análisis, crea las Tarjetas CRC para las 5 clases principales del sistema siguiendo estas REGLAS ESTRICTAS:

1. **Fuente de Verdad para Atributos y Relaciones:** Extráelos EXCLUSIVAMENTE del diagrama `01-boceto-inicial.excalidraw`. NO inventes propiedades (como 'estaPagado', 'saldo', etc.) que no estén textualmente dibujadas en las clases del diagrama, por más que se mencionen en el archivo de texto.
2. **Fuente de Verdad para Responsabilidades:** Usa el archivo `introduccion.md` únicamente como contexto de negocio para redactar las responsabilidades y el "Pensamiento del objeto", priorizando siempre la alta cohesión y el bajo acoplamiento.
3. **Formato:** Utiliza obligatoriamente la siguiente plantilla de tabla Markdown para cada clase. No uses listas ni cambies la estructura. Si los colaboradores o propiedades se acaban antes que las responsabilidades, deja las celdas en blanco (es lo correcto para evitar redundancia).

Plantilla a utilizar:

|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | Socio | | |
| **Superclase:** | | | |
| **Subclase:** | | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
| Poder registrarse al Gimnasio | Recepcion | Conozco mi DNI para registrarme | DNI |
| Poder reservar una clase | Clase | Conozco mi nroCarnet para reservar una clase y la clase a la que quiero inscribirme | nroCarnet |
| | | | |
| | | | |
| | | | |
| | | | |

Genera las 5 tablas por separado para que pueda copiarlas a sus archivos individuales."

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`

### 3. Output obtenido de Copilot
Tras ejecutar el prompt, Copilot identificó las clases principales del sistema y generó la estructura de las tarjetas CRC. Sin embargo, durante mi revisión crítica detecté que la herramienta sobre-infirió datos: mezcló requerimientos de los Casos de Uso presentes en la introducción con el boceto inicial, inventando atributos (como fechaCreacion o montoPagado) que no existían en el diagrama de Excalidraw. Además, duplicó la salida de algunas clases. Ante esto, asumí mi rol y realicé una revisión exhaustiva: descarté las alucinaciones de la IA, eliminé las redundancias, ajusté las responsabilidades para mantener una alta cohesión y garanticé que los atributos reflejen estrictamente el diagrama original. Conservé los tópicos más acertados, pero el contenido final es producto de un ajuste manual riguroso.

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** 
- eliminar la duplicación de la tarjeta Pedido donde también 02-tarjeta-crc-pedido.md:3, aparece Pedido. Clase faltante del dominio para completar las 5 clases clave exigidas por la consigna
- elimina a Pago de los colaboradores de ItemPedido. El pago se relación únicamente con Historial_pago y tiene como clase hija metodo_pago, no tiene relacion con ItemPedido
- Pago incorrectamente asociado a PersonalizacionItem:
remové a Pago de la lista de colaboradores. Pago se relaciona únicamente con Historial_pago . PersonalizacionItem, se relaciona unicamente con item_pedido
* **Sugerencias descartadas o modificadas:** Las únicas sugerencias descartadas fueron los datos mezclados que utilizó la ia al momento de generar las estructuras (explicado anteriormente).

---