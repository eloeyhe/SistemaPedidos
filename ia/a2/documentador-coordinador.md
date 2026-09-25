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

## Code Review 4: PR [#72] - [Modelador de diagramas de Casos de Uso]
* **Rama revisada:** `feature/modelador-diag-casos-uso-update-use-case-1`
* **Autor:** Agustin Ariel Feijo

### 1. Prompt utilizado
&gt;Realizá un Code Review de los cambios realizados por el rol
Modelador de Diagramas de Casos de Uso para la Actividad Obligatoria N.º 2.

Usá como principal archivo de contexto:
#file:anexos/introduccion.md

Revisá los archivos incorporados o modificados por este rol y verificá:

 Coherencia con los requisitos y casos de uso de la Actividad N.º 1.
 Correcta representación de casos de uso, actores y relaciones UML.
 Correcto uso de PlantUML.
 Estructura, naming y ubicación de los archivos.
 Coherencia de los índices y enlaces.
 Que no se hayan agregado elementos sin respaldo en la documentación.
 Que la documentación correspondiente en ia/a2 esté completa.

No modifiques ningún archivo.

Mostrame únicamente:
 Hallazgos que requieren corrección.
 Sugerencias de mejora.
 Aspectos correctos.

Para cada hallazgo indicá el archivo, la ubicación aproximada, el problema
detectado y la corrección recomendada.

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`



### 3. Output obtenido de Copilot

* Se reemplazaron dos casos de uso definidos en A1 en el archivo `02-caso-uso-priorizar-pedido-03.puml`, `02-caso-uso-registrar-pago-05.puml`   problema: A1 define `CU03: Cancelar Pedido` y `CU05: Consultar Pedidos Activos`, pero se modelaron `Priorizar Pedido` y `Registrar Pago`.  
  
  
* Faltan los diagramas de CU03 y CU05 en carpeta `02-casos-de-uso`.  
  Problema: no hay diagramas para cancelación ni consulta de pedidos activos.  
  
  
* Naming inconsistente con A1 en `02-caso-uso-crear-pedido-01.puml`  
  Problema: el caso se denomina `Crear Pedido`, mientras que A1 lo define como `Tomar Pedido`.  
  
  
* Actor incorrecto en el envío a cocina en `02-caso-uso-crear-pedido-01.puml`, asociación `Cocina -- Confirmar`.  
  Problema: RF2 indica que el envío a cocina ocurre automáticamente luego de la confirmación del Usuario de Mostrador. La asociación actual sugiere que Cocina confirma el pedido.  
  
  
* cliente modelado como actor directo del sistema en los diagramas de crear, modificar, priorizar y registrar pago.  
  Problema: A1 establece que el Cliente interactúa indirectamente mediante el Usuario de Mostrador y no posee usuario en el sistema.  
  
  
* Falta el actor `Encargado`en 02-caso-uso-priorizar-pedido-03.puml`, por ausencia del diagrama de cancelación.  
  Problema: A1 define al Encargado como responsable de cancelaciones especiales.  
  
  
* Elementos sin respaldo suficiente en la documentación en`02-caso-uso-priorizar-pedido-03.puml`  
  Problema: Solicitar prioridad` y `Validar factibilidad de prioridad` no aparecen como comportamientos definidos en A1. RF7 solo requiere que el Usuario de Mostrador marque manualmente un pedido como prioritario.  
  
  
* Relaciones UML ambiguas en CU04 en`02-caso-uso-cambiar-estado-pedido-04.puml`
Problema: ambos actores están asociados directamente con `Cambiar Estado de Pedido`, sin distinguir qué transiciones puede realizar cada uno.  


Índice con enlace a un archivo inexistente en diagramasUML.md
 Ubicación: enlace “Diagramas de Casos de Uso”.  
  Problema: apunta a `./02-casos-de-uso/diagramas_de_casos_de_uso.md`, archivo que no fue incorporado.  


- Documentación del rol incompleta.  
  Archivo: `modelador-diagramas-casos-uso.md`  
  Problema: el archivo contiene únicamente un carácter y no registra rol, prompt, contexto, archivos generados, criterios aplicados ni resultado.  



### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** 
* Se reemplazaron dos casos de uso definidos en A1 en el archivo `02-caso-uso-priorizar-pedido-03.puml`, `02-caso-uso-registrar-pago-05.puml`  
problema: A1 define `CU03: Cancelar Pedido` y `CU05: Consultar Pedidos Activos`, pero se modelaron `Priorizar Pedido` y `Registrar Pago`. 

* Naming inconsistente con A1 en `02-caso-uso-crear-pedido-01.puml`  
  Problema: el caso se denomina `Crear Pedido`, mientras que A1 lo define como `Tomar Pedido`.  
  
  
* Actor incorrecto en el envío a cocina en `02-caso-uso-crear-pedido-01.puml`, asociación `Cocina -- Confirmar`.  
  Problema: RF2 indica que el envío a cocina ocurre automáticamente luego de la confirmación del Usuario de Mostrador. La asociación actual sugiere que Cocina confirma el pedido.  

* cliente modelado como actor directo del sistema en los diagramas de crear, modificar, priorizar y registrar pago.  
  Problema: A1 establece que el Cliente interactúa indirectamente mediante el Usuario de Mostrador y no posee usuario en el sistema.  

* Elementos sin respaldo suficiente en la documentación en`02-caso-uso-priorizar-pedido-03.puml`  
  Problema: Solicitar prioridad` y `Validar factibilidad de prioridad` no aparecen como comportamientos definidos en A1. RF7 solo requiere que el Usuario de Mostrador marque manualmente un pedido como prioritario.  

* Relaciones UML ambiguas en CU04 en`02-caso-uso-cambiar-estado-pedido-04.puml`  
Problema: ambos actores están asociados directamente con `Cambiar Estado de Pedido`, sin distinguir qué transiciones puede realizar cada uno.  


* Documentación del rol incompleta.  
  Archivo: `modelador-diagramas-casos-uso.md`  
  Problema: el archivo contiene únicamente un carácter y no registra rol, prompt, contexto, archivos generados, criterios aplicados ni resultado.  



* **Sugerencias descartadas o modificadas:** 
* Faltan los diagramas de CU03 y CU05 en carpeta `02-casos-de-uso`.  
  Problema: no hay diagramas para cancelación ni consulta de pedidos activos.  
    + no se tuvo en cuenta la sugerencia debido a que ya se menciono anteriormente que se precentan casos de uso que no coinciden con la A1.
    los casos de uso deberian remplazarse por  " Cancelar Pedido" y "Consultar edidos Activos"

* Falta el actor `Encargado`en 02-caso-uso-priorizar-pedido-03.puml`, por ausencia del diagrama de cancelación.  
  Problema: A1 define al Encargado como responsable de cancelaciones especiales. 
  + no se incorporo, ya que, como se mencionoanteriormente al remplazar los casos de uso con los correspondientes a la A1, el actor "Encargado"
  se incorpora debido a que forma parte de los casos de uso de la A1.

* Índice con enlace a un archivo inexistente en diagramasUML.md, enlace “Diagramas de Casos de Uso”.  
 Problema: apunta a `./02-casos-de-uso/diagramas_de_casos_de_uso.md`, archivo que no fue incorporado. 
 + se modifico la sugerencia por el formato del indice, ya que dicho indice incorporaba las tarjetas CRC, cuando no correspondia su
 incorporacion, sino que deberia obtener los diagramas, casos de uso, y los escenarios de casos de uso.

---

## Code Review 5: PR [#72] - [Modelador de diagramas de Casos de Uso ]
* **Rama revisada:** `feature/modelador-diag-casos-uso-update-use-case-1`
* **Autor:** Agustin Ariel Feijo

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
`

### 3. Output obtenido de Copilot

 * Enlace roto en el índice
   Archivo: `diagramasUML.md`, sección **Diagramas de Casos de Uso**.
   El enlace apunta a `diagramas/02-casos-de-uso/diagramas_de_casos_de_uso.md`, pero ese archivo no existe en la PR.
   Los cinco `.puml` y sus imágenes sí están incluidos.


 * CU02 no explicita el medio de pago del ajuste
   Archivo: `diagramas/02-casos-de-uso/CU02-modificar-pedido.puml`, caso `Registrar ajuste de cobro`.
   `introduccion.md`, RF8 y CU02, requieren seleccionar efectivo o transferencia para registrar diferencias.
   El diagrama solo muestra `Registrar ajuste de cobro`, sin representar la selección/validación del medio de pago ni la condición de que el ajuste exista únicamente si cambia el total.

* CU03 no representa claramente las restricciones de cancelación
 Archivo: `diagramas/02-casos-de-uso/CU03-cancelar-pedido.puml`.
   El diagrama incluye `Verificar estado y permisos`, pero no explicita:
     que no se puede cancelar un pedido entregado;
     que cancelar en preparación requiere intervención del Encargado;
     que los estados LISTO y ENTREGADO requieren el tratamiento especial indicado en CU03 de `introduccion.md`.
    Además, `Encargado` está asociado directamente a `Cancelar Pedido`, pero no específicamente a `Autorizar cancelacion especial`.

  * CU05 omite vistas diferenciadas por actor
    Archivo: `diagramas/02-casos-de-uso/CU05-consultar-pedidos-activos.puml`.
    `introduccion.md`, CU05, especifica que Cocina visualiza RECIBIDO y EN_PREPARACION, mientras Mostrador visualiza todos los estados activos.
    El diagrama muestra el mismo caso `Mostrar estados RECIBIDO, EN_PREPARACION y LISTO` para ambos actores, sin representar esa diferencia.



### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** 

 * Archivo: `diagramasUML.md`, sección **Diagramas de Casos de Uso**.
 El enlace apunta a `diagramas/02-casos-de-uso/diagramas_de_casos_de_uso.md`, pero ese archivo no existe en la PR.
 Los cinco `.puml` y sus imágenes sí están incluidos.
 
 * CU05 omite vistas diferenciadas por actor
 Archivo: `diagramas/02-casos-de-uso/CU05-consultar-pedidos-activos.puml`.
 `introduccion.md`, CU05, especifica que Cocina visualiza RECIBIDO y EN_PREPARACION, mientras Mostrador visualiza todos los estados activos.
 El diagrama muestra el mismo caso `Mostrar estados RECIBIDO, EN_PREPARACION y LISTO` para ambos actores, sin representar esa diferencia.

* CU03 no representa claramente las restricciones de cancelación
Archivo: `diagramas/02-casos-de-uso/CU03-cancelar-pedido.puml`.
El diagrama incluye `Verificar estado y permisos`, pero no explicita:
     que no se puede cancelar un pedido entregado;
     que cancelar en preparación requiere intervención del Encargado;
     que los estados LISTO y ENTREGADO requieren el tratamiento especial indicado en CU03 de `introduccion.md`.
     Además, `Encargado` está asociado directamente a `Cancelar Pedido`, pero no específicamente a `Autorizar cancelacion especial`.

* CU02 no explicita el medio de pago del ajuste**
Archivo: `diagramas/02-casos-de-uso/CU02-modificar-pedido.puml`, caso `Registrar ajuste de cobro`.
`introduccion.md`, RF8 y CU02, requieren seleccionar efectivo o transferencia para registrar diferencias.
El diagrama solo muestra `Registrar ajuste de cobro`, sin representar la selección/validación del medio de pago ni la condición de que el ajuste exista únicamente si cambia el total.

* **Sugerencias descartadas o modificadas:** 

No se descartaron ni modificaron sugerencias

---

