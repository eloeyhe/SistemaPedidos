# Informe de Interacción con IA - Especialista en SRP 

**Rol:** Especialista en SRP 

**Herramienta utilizada:** GitHub Copilot Agent Mode (VS Code)


  --- 

  ## 1. Archivos de Contexto Referenciados: 
  * `anexos/introduccion.md\` 
  * `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw\`
  * `herramientas-agile/tarjetas-crc/\` 
   
   --- 
   ## 2. Prompt Utilizado:
   ```text
   anexos/introduccion.md
   diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
   Tarjetas CRC en herramientas-agile/tarjetas-crc/
   
   Lee los archivos de contexto indicados del Sistema de Pedidos. plantea sugerencias de 3 posibles clases del diseño actual que tengan múltiples responsabilidades (más de una razón para cambiar) y propone una refactorización aplicando el Principio de Responsabilidad Única (SRP), explicando qué nuevas clases o componentes especializados se deben extraer de cada una.
   no modifiques archivos
 ```

 
  ## 3. Output obtenido:

- Análisis de clases según el Principio de Responsabilidad Única (SRP)

Tomando el boceto como diseño actual —no como código ya implementado—, estas tres clases muestran responsabilidades que podrían cambiar por motivos distintos.

El **Principio de Responsabilidad Única (SRP)** no exige que cada clase tenga un solo método, sino que sus funciones pertenezcan a una misma responsabilidad y a una razón de cambio coherente.

| Clase | Responsabilidades que se mezclan | Refactorización propuesta |
| --- | --- | --- |
| **Pedido** | Administra el contenido de la orden (`agregar`, `modificar`, `cancelar`) y también su operación: transiciones de estado y prioridad (`cambiarEstado`, `priorizar`). | Mantener `Pedido` como agregado de la orden: identidad, referencia de retiro, fecha e ítems. Extraer un componente `PoliticaCicloPedido` que valide las transiciones permitidas, la cancelación según el estado y la regla de modificación solo en `RECIBIDO`. La prioridad puede delegarse a una `PoliticaPrioridadPedido` si sus reglas de asignación o visualización cambian de forma independiente. |
| **Pago** | Registra el cobro y calcula diferencias asociadas a modificaciones del pedido. El registro de una transacción y el cálculo del ajuste responden a reglas que pueden evolucionar por separado. | Mantener `Pago` como registro del cobro efectuado —monto, método y fecha—. Extraer `ServicioRegistroPago` para validar y registrar cobros con los medios admitidos, y `CalculadorAjustePago` para comparar el total previo con el nuevo y determinar si corresponde cobrar una diferencia o registrar una devolución. Reutilizar `Historial_pago` para conservar esos movimientos auditables. |
| **Item_pedido** | Calcula el subtotal y, a la vez, gestiona agregar o quitar personalizaciones. La fórmula de precios puede cambiar independientemente de las reglas de personalización. | Mantener en `Item_pedido` la cantidad y el precio unitario histórico de la línea. Delegar el cálculo a `CalculadorSubtotalItem`, usando el precio histórico y los adicionales de personalización; delegar la representación y el costo de cada ajuste a `Personalizacion_item`, que ya aparece como clase del diseño. |

- Justificación

La propuesta concuerda con los requisitos: el pedido debe conservar su identificador y bloquear modificaciones después de `RECIBIDO`; los cambios pueden producir un cobro adicional o una devolución; y los ítems deben preservar sus precios históricos.

Por eso, separar estas reglas reduce el acoplamiento sin perder trazabilidad ni alterar esas restricciones.

### Contexto revisado

- `anexos/introduccion.md`
- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- CRC de `Pedido`
- CRC de `Pago`
- CRC de `Item_pedido`



  ## 4. Ajustes realizados:

  * Se realizaron ajustes de diseño con el fin de simpllificar el modelo: Se elimino el atributo `idPedido` debido a que las clases no precisan de un identificador unico como "id". 