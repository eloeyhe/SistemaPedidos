# Registro de Code Reviews asistidas con IA - Documentador y Coordinador

## Code Review 1: PR [#69] - [Diseñador de Tarjetas CRC]
* **Rama revisada:** `future/diseniador-tarjetas-crc-add-tarjeta-clase-1`
* **Autor:** Eloy Eyheramendy

### 1. Prompt utilizado
 &gt; Actuá como un revisor técnico para el repositorio del proyecto SistemaPedidos:
Leé de forma obligatoria el archivo #file:anexos/introduccion.md como contexto general del proyecto y analizá los archivos modificados en esta Pull Request.

Realizá las siguientes verificaciones:

Coherencia de dominio:
Confirmá que las clases, diagramas, escenarios o tarjetas CRC desarrolladas sean coherentes con el dominio especificado en #file:anexos/introduccion.md.

Completitud:
Verificá que los archivos entregados estén completos y que no falten campos requeridos por la actividad.

Tarjetas CRC:
Verificá específicamente que las tarjetas CRC incluidas en esta PR tengan clases, responsabilidades y colaboradores coherentes con el modelo del proyecto y entre sí.

No modifiques ningún archivo.
Entregá un reporte estructurado con:

Errores e inconsistencias detectados, indicando archivo y sección específica.


### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `herramientas-agile/tarjetas-crc/01-tarjeta-crc-nombre-clase1.md`
* `herramientas-agile/tarjetas-crc/02-tarjeta-crc-pedido.md`
* `herramientas-agile/tarjetas-crc/03-tarjeta-crc-item-pedido.md`
* `herramientas-agile/tarjetas-crc/04-tarjeta-crc-producto.md`
* `herramientas-agile/tarjetas-crc/05-tarjeta-crc-personalizacion-item.md`
* `diagramas/01-diagrama-clases/01-boceto-inicial.png`


### 3. Output obtenido de Copilot
 * **Errores e inconsistencias detectados:**
 * Tarjeta CRC duplicada y entrega incompleta: `01-tarjeta-crc-nombre-clase1.md\` y `02-tarjeta-crc-pedido.md\` describen la misma clase (`Pedido\`), difiriendo en atributos (`fecha\`/`fechaCreacion\`, `estado\`/`estadoActual\`, `prioritario\`/`prioridad\`). La PR anuncia 5 tarjetas pero entrega solo 4 clases distintas.


 * Combo no está alineado entre el diagrama y la tarjeta: `04-tarjeta-crc-producto.md\` declara `Combo\` como subclase de `Producto\`, pero el diagrama UML lo muestra separado sin una generalización explícita hacia `Producto\`, violando el RF1\. 

 * Colaborador incorrecto en `PersonalizacionItem\`: En `05-tarjeta-crc-personalizacion-item.md\`, `Pago\` figura como colaborador del cálculo del costo adicional, cuando la personalización afecta a `ItemPedido\` y `Pago\` interviene posteriormente en el cobro del `Pedido\` completo (RF1, CU01).

 * Relación directa entre `ItemPedido\` y `Pago\` sin justificación:`03-tarjeta-crc-item-pedido.md\` incluye a `Pago\` como colaborador de `ItemPedido\`, mientras que el dominio establece una relación 1 a 1 entre `Pago\` y el `Pedido\` completo.

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:**  
     * Tarjeta CRC duplicada y entrega incompleta: `01-tarjeta-crc-nombre-clase1.md\` y `02-tarjeta-crc-pedido.md\` describen la misma clase (`Pedido\`), difiriendo en atributos (`fecha\`/`fechaCreacion\`, `estado\`/`estadoActual\`, `prioritario\`/`prioridad\`). La PR anuncia 5 tarjetas pero entrega solo 4 clases distintas.
* **Sugerencias descartadas o modificadas:** 
    * Combo no está alineado entre el diagrama y la tarjeta: `04-tarjeta-crc-producto.md\` declara `Combo\` como subclase de `Producto\`, pero el diagrama UML lo muestra separado sin una generalización explícita hacia `Producto\`, violando el RF1\. 
        + se la descarto debido a que si se la declara a `Combo\` como subclase.

    * Relación directa entre `ItemPedido\` y `Pago\` sin justificación:`03-tarjeta-crc-item-pedido.md\` incluye a `Pago\` como colaborador de `ItemPedido\`, mientras que el dominio establece una relación 1 a 1 entre `Pago\` y el `Pedido\` completo.
        + se mantuvo la eliminacion de `Pago\` de los colaboradores de `ItemPedido\`, pero se modifico el argumento ya que `Pago\` no establece una realcion 1 a 1 con `Pedido\` ni con `ItemPedido\`

    * Colaborador incorrecto en `PersonalizacionItem\`: En `05-tarjeta-crc-personalizacion-item.md\`, `Pago\` figura como colaborador del cálculo del costo adicional, cuando la personalización afecta a `ItemPedido\` y `Pago\` interviene posteriormente en el cobro del `Pedido\` completo (RF1, CU01).
        + se sugirio removér a Pago de la lista de colaboradores. ya que, PersonalizacionItem, se relaciona unicamente con item_pedido




---

## Code Review 2: PR [#70] - [ Especialista en Escenarios de Casos de Uso]
* **Rama revisada:** `feature/espec-escenarios-casos-uso-add-escenario-1`
* **Autor:** Leandro Dominguez

### 1. Prompt utilizado
&gt; Actuá como un revisor técnico para el repositorio del proyecto SistemaPedidos.

Leé de forma obligatoria el archivo #file:anexos/introduccion.md como contexto general del proyecto y analizá los archivos modificados en esta Pull Request.

Realizá las siguientes verificaciones:

Coherencia de dominio:

Confirmá que los casos de uso, escenarios y actores desarrollados sean coherentes con el dominio especificado en #file:anexos/introduccion.md.

Completitud:

Verificá que los archivos entregados estén completos y que no falten campos requeridos por la actividad.

Casos de uso:

Verificá específicamente que los casos de uso y escenarios incluidos en esta PR tengan actores, flujo principal, flujos alternativos y pasos coherentes con el modelo del proyecto y entre sí.

No modifiques ningún archivo.


### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `diagramas/03-escenarios-casos-de-uso/escenarios_de_casos_de_uso.md`
* `diagramas/03-escenarios-casos-de-uso/03-tomar-pedido-registro-pedido-01.md`
* `diagramas/03-escenarios-casos-de-uso/03-modificar-pedido-modificacion-recibido-01.md`
* `diagramas/03-escenarios-casos-de-uso/03-cancelar-pedido-cancelacion-antes-entrega-01.md`
* `diagramas/03-escenarios-casos-de-uso/03-cambiar-estado-pedido-avance-preparacion-01.md`
* `diagramas/03-escenarios-casos-de-uso/03-consultar-pedidos-activos-prioridad-urgentes-01.md`

### 3. Output obtenido de Copilot
* **Errores e inconsistencias detectados:**
  * Uso prematuro de precios históricos en 03-tomar-pedido-registro-pedido-01.md
  El paso 6 indica que el sistema calcula subtotales utilizando “precios históricos de los productos”. Sin embargo, en introduccion.md, CU01 establece que:
  Primero se calcula el subtotal utilizando el precio vigente del catálogo.
  Luego, al confirmar el pedido, se congela y guarda el precio histórico del ítem y sus personalizaciones.

  * Falta explicitar el registro histórico de los ajustes de pago en 03-modificar-pedido-modificacion-recibido-01.md
  Se registra la diferencia de pago, pero falta aclarar que el pago original se conserva y cada cobro adicional o devolución queda registrado por separado.
  
  * No hay una sección explícita de flujos alternativos en los escenarios
  Archivos: los cinco escenarios individuales.
  Los cinco escenarios incluyen situaciones excepcionales dentro de “Aspectos sobresalientes”, pero no tienen una sección específica de flujos alternativos.

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** 
    * Uso prematuro de precios históricos en 03-tomar-pedido-registro-pedido-01.md
    El paso 6 indica que el sistema calcula subtotales utilizando “precios históricos de los
    productos”. Sin embargo, en introduccion.md, CU01 establece que:
    Primero se calcula el subtotal utilizando el precio vigente del catálogo.
    Luego, al confirmar el pedido, se congela y guarda el precio histórico del ítem y sus personalizaciones.

   * Falta explicitar el registro histórico de los ajustes de pago en 03-modificar-pedido-modificacion-recibido-01.md
   En el paso 7, se registra la diferencia de pago, pero falta aclarar que el pago original se conserva y cada cobro adicional o devolución queda registrado por separado.

* **Sugerencias descartadas o modificadas:** 
    * No hay una sección explícita de flujos alternativos en los escenarios
    Los cinco escenarios incluyen situaciones excepcionales dentro de “Aspectos sobresalientes”, pero no tienen una sección específica de flujos alternativos.
       + La sugerencia fue descartada debido a que no se establece como un requisito obligatorio. 

---

## Code Review 3: PR [#69] - [Diseñador de Tarjetas CRC]
* **Rama revisada:** `future/diseniador-tarjetas-crc-add-tarjeta-clase-1`
* **Autor:** Eloy Eyheramendy

### 1. Prompt utilizado
 &gt; Actuá como un revisor técnico para el repositorio del proyecto SistemaPedidos:

Leé de forma obligatoria el archivo #file:anexos/introduccion.md como contexto general del proyecto y analizá los archivos modificados en esta Pull Request.

Realizá las siguientes verificaciones:

Coherencia de dominio:
Confirmá que las clases, diagramas, escenarios o tarjetas CRC desarrolladas sean coherentes con el dominio especificado en #file:anexos/introduccion.md.

Completitud:
Verificá que los archivos entregados estén completos y que no falten campos requeridos por la actividad.

Tarjetas CRC:
Verificá específicamente que las tarjetas CRC incluidas en esta PR tengan clases, responsabilidades y colaboradores coherentes con el modelo del proyecto y entre sí.

No modifiques ningún archivo.
Entregá un reporte estructurado con:

Errores e inconsistencias detectados, indicando archivo y sección específica.


### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
* `herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md`
* `herramientas-agile/tarjetas-crc/03-tarjeta-crc-producto.md`
* `diagramas/01-diagrama-clases/01_boceto_inicial.png`

### 3. Output obtenido de Copilot
* Combo está declarado incorrectamente como subclase de Producto en 03-tarjeta-crc-producto.md:4
Problema: La tarjeta declara Combo como subclase de Producto. En el diagrama, ProductoPreparado y ProductoEnvasado heredan de Producto.

* Pago aparece como colaborador incorrecto de Pedido en 01-tarjeta-crc-pedido.md:8
 Problema: Pago figura como colaborador de la responsabilidad “Cambiar mi estado operativo y gestionar la prioridad del pedido. El cambio de estado se relaciona con Estado_pedido; Pago no participa en la prioridad ni en la máquina de estados.

* El nombre de la clase no coincide exactamente con el diagrama en 02-tarjeta-crc-item-pedido.md:3
 Problema: La tarjeta usa item_Pedido, mientras el diagrama muestra Item_pedido.

* La documentación de IA referencia una rama incorrecta en diseniador-tarjetas-crc.md:3
 Problema: Se documenta la rama future/diseniador-tarjetas-crc-add-tarjeta-clase-1, pero la rama real es feature/diseniador-tarjetas-crc-add-tarjeta-clase-1.


### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** 
* Pago aparece como colaborador incorrecto de Pedido en 01-tarjeta-crc-pedido.md:8
 Problema: Pago figura como colaborador de la responsabilidad “Cambiar mi estado operativo y gestionar la prioridad del pedido. El cambio de estado se relaciona con Estado_pedido; Pago no participa en la prioridad ni en la máquina de estados.

 * El nombre de la clase no coincide exactamente con el diagrama en 02-tarjeta-crc-item-pedido.md:3
 Problema: La tarjeta usa item_Pedido, mientras el diagrama muestra Item_pedido.

 * La documentación de IA referencia una rama incorrecta en diseniador-tarjetas-crc.md:3
 Problema: Se documenta la rama future/diseniador-tarjetas-crc-add-tarjeta-clase-1, pero la rama real es feature/diseniador-tarjetas-crc-add-tarjeta-clase-1.

* **Sugerencias descartadas o modificadas:** 
* Combo está declarado incorrectamente como subclase de Producto en 03-tarjeta-crc-producto.md:4
Problema: La tarjeta declara Combo como subclase de Producto. En el diagrama, ProductoPreparado y ProductoEnvasado heredan de Producto.
  + Producto solo tiene como subclase a combo. Producto preparado y producto envasado tienen relación con pedido, no con producto.
---

## Code Review 4: PR #[Número] - [Rol / Nombre de la Feature]
* **Rama revisada:** `future/...`
* **Autor:**

### 1. Prompt utilizado
&gt; "[prompt exacto utlizado]"

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `[Ruta del archivo subido en la PR, ej: herramientas-agile/tarjetas-crc/01-tarjeta-crc-clase.md]`
* `[Plantilla correspondiente o diagrama de referencia]`

### 3. Output obtenido de Copilot
[Resumen de las observaciones o sugerencias que devolvió la IA sobre el contenido, nombrado, formato o coherencia]

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** [Indicar qué observaciones se cargaron como Request Changes o comentarios en el diff de GitHub]
* **Sugerencias descartadas o modificadas:** [Explicar si se descartó alguna sugerencia de la IA por no ajustarse al dominio del sistema o a las pautas del docente]

---


