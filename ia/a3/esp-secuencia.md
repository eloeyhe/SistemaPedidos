# Registro de Uso de IA - Especialista en Diagramas de Secuencia

En este documento se registra el proceso de colaboración con **GitHub Copilot** para la generación de los 5 diagramas de secuencia correspondientes a los escenarios de casos de uso definidos en la Actividad N°2.

---

## 1. Diagrama de Secuencia 1: Tomar Pedido - Registro de Pedido

* **Archivo generado:** `diagramas/05-diagramas-secuencia/05-secuencia-tomar-pedido-registro-de-pedido-01.puml`

### Contexto de Entrada Utilizado

Para otorgar contexto preciso a GitHub Copilot, se mantuvieron abiertos y referenciados los siguientes archivos de la Actividad N°2:

* `diagramas/03-escenarios-casos-de-uso/03-tomar-pedido-registro-pedido-01.md`
* `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
* `herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md`
* `herramientas-agile/tarjetas-crc/03-tarjeta-crc-producto.md`
* `herramientas-agile/tarjetas-crc/04-tarjeta-crc-personalizacion-item.md`
* `herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md`

### Prompt Enviado a Copilot

```text
Actúa como un Especialista en Diseño Orientado a Objetos y experto en UML. Tu tarea es generar el código PlantUML para un Diagrama de Secuencia.

Para hacerlo, debes basarte ESTRICTAMENTE en el flujo principal y alternativo del escenario descrito en el archivo diagramas/03-escenarios-casos-de-uso/03-tomar-pedido-registro-pedido-01.md y en las responsabilidades, colaboraciones y propiedades definidas en las tarjetas CRC adjuntas: herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md, herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md, herramientas-agile/tarjetas-crc/03-tarjeta-crc-producto.md, herramientas-agile/tarjetas-crc/04-tarjeta-crc-personalizacion-item.md y herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md. Las tarjetas CRC describen responsabilidades, colaboradores y propiedades, no una lista literal de métodos: deriva de ellas los mensajes necesarios sin presentar nombres de métodos inventados como si estuvieran definidos en las tarjetas. La clase MetodoPago no tiene tarjeta CRC propia: si representas la validación del medio de pago, trátala como una interacción externa o colaborador del Pago y no le atribuyas responsabilidades CRC no documentadas. Respeta la regla de que el pedido no se registra si se rechaza el pago por transferencia y conserva los precios históricos de productos y personalizaciones.

Por favor, cumple estrictamente con los siguientes requisitos técnicos para el código PlantUML:

1. Participantes: Incluye al menos 4 participantes (actores u objetos). El primer participante (a la izquierda) DEBE ser el actor que inicia el escenario o la interfaz.
2. Interacciones: Cada participante debe enviar o recibir un mínimo de 3 mensajes.
3. Nomenclatura de mensajes: Los mensajes en las flechas deben estar escritos en `camelCase` y sus argumentos deben derivarse de las responsabilidades, colaboraciones, propiedades y datos del escenario descritos en las tarjetas CRC, sin atribuirles métodos literales que no enumeran.
4. Notación UML: Utiliza la notación correcta para nombrar a los participantes según corresponda: solo Clase (`Clase`), solo objeto (`:objeto`) o ambos (`Clase:objeto`).
5. Ciclo de vida: Si en el escenario se crea un objeto temporal que luego no persiste (ej. una Notificacion), debes indicar explícitamente su destrucción al final de su línea de vida utilizando el comando `destroy` de PlantUML (para que aparezca la X al final de la línea).
```


## 2. Diagrama de Secuencia 2: Modificar Pedido - Modificación en Estado Recibido

- **Archivo generado:** `diagramas/05-diagramas-secuencia/05-secuencia-modificar-pedido-modificacion-recibido-02.puml`

### Contexto de Entrada Utilizado

Para otorgar contexto preciso a GitHub Copilot, se mantuvieron abiertos y referenciados los siguientes archivos de la Actividad N°2:

- `diagramas/03-escenarios-casos-de-uso/03-modificar-pedido-modificacion-recibido-01.md`
- `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
- `herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md`
- `herramientas-agile/tarjetas-crc/03-tarjeta-crc-producto.md`
- `herramientas-agile/tarjetas-crc/04-tarjeta-crc-personalizacion-item.md`
- `herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md`
- `herramientas-agile/tarjetas-crc/10-tarjeta-crc-historial-pago.md`
- `herramientas-agile/tarjetas-crc/12-tarjeta-crc-historial-cambio-pedido.md`
- `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`

### Prompt Enviado a Copilot

```text
Actúa como un Especialista en Diseño Orientado a Objetos y experto en UML. Tu tarea es generar el código PlantUML para un Diagrama de Secuencia.

Para hacerlo, debes basarte ESTRICTAMENTE en el flujo principal y alternativo del escenario descrito en el archivo diagramas/03-escenarios-casos-de-uso/03-modificar-pedido-modificacion-recibido-01.md y en las responsabilidades, colaboraciones y propiedades definidas en las tarjetas CRC adjuntas: herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md, herramientas-agile/tarjetas-crc/02-tarjeta-crc-item-pedido.md, herramientas-agile/tarjetas-crc/03-tarjeta-crc-producto.md, herramientas-agile/tarjetas-crc/04-tarjeta-crc-personalizacion-item.md, herramientas-agile/tarjetas-crc/05-tarjeta-crc-pago.md, herramientas-agile/tarjetas-crc/10-tarjeta-crc-historial-pago.md, herramientas-agile/tarjetas-crc/12-tarjeta-crc-historial-cambio-pedido.md y herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md. Las tarjetas CRC describen responsabilidades, colaboradores y propiedades, no una lista literal de métodos: deriva de ellas los mensajes necesarios sin presentar nombres de métodos inventados como si estuvieran definidos en las tarjetas. Representa la validación del estado RECIBIDO también al confirmar la modificación; conserva el identificador original y el pago original, y registra por separado cualquier cobro adicional o devolución vinculada al pago original cuando corresponda.

Por favor, cumple estrictamente con los siguientes requisitos técnicos para el código PlantUML:

1. Participantes: Incluye al menos 4 participantes (actores u objetos). El primer participante (a la izquierda) DEBE ser el actor que inicia el escenario o la interfaz.
2. Interacciones: Cada participante debe enviar o recibir un mínimo de 3 mensajes.
3. Nomenclatura de mensajes: Los mensajes en las flechas deben estar escritos en `camelCase` y sus argumentos deben derivarse de las responsabilidades, colaboraciones, propiedades y datos del escenario descritos en las tarjetas CRC, sin atribuirles métodos literales que no enumeran.
4. Notación UML: Utiliza la notación correcta para nombrar a los participantes según corresponda: solo Clase (`Clase`), solo objeto (`:objeto`) o ambos (`Clase:objeto`).
5. Ciclo de vida: Si en el escenario se crea un objeto temporal que luego no persiste, debes indicar explícitamente su destrucción al final de su línea de vida utilizando el comando `destroy` de PlantUML (para que aparezca la X al final de la línea).
```

## 3. Diagrama de Secuencia 3: Cancelar Pedido - Cancelación de pedido antes de la entrega

**Archivo generado:** `diagramas/05-diagramas-secuencia/05-secuencia-cancelar-pedido-cancelacion-antes-entrega-03.puml`

### Contexto de Entrada Utilizado

Para otorgar contexto preciso a GitHub Copilot, se mantuvieron abiertos y referenciados los siguientes archivos de la Actividad N°2:

* `diagramas/03-escenarios-casos-de-uso/03-cancelar-pedido-cancelacion-antes-entrega-01.md`
* `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
* `herramientas-agile/tarjetas-crc/11-tarjeta-crc-auditoria-pedido.md`
* `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`
* `herramientas-agile/tarjetas-crc/16-tarjeta-crc-usuario-encargado.md`

### Prompt Enviado a Copilot

```text
Actúa como un Especialista en Diseño Orientado a Objetos y experto en UML. Tu tarea es generar el código PlantUML para un Diagrama de Secuencia. Para hacerlo, debes basarte ESTRICTAMENTE en el flujo principal y alternativo del escenario descrito en el archivo diagramas/03-escenarios-casos-de-uso/03-cancelar-pedido-cancelacion-antes-entrega-01.md y en las responsabilidades, colaboraciones y propiedades definidas en las tarjetas CRC adjuntas: herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md, herramientas-agile/tarjetas-crc/11-tarjeta-crc-auditoria-pedido.md, herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md y herramientas-agile/tarjetas-crc/16-tarjeta-crc-usuario-encargado.md. Las tarjetas CRC describen responsabilidades, colaboradores y propiedades, no una lista literal de métodos: deriva de ellas los mensajes necesarios sin presentar nombres de métodos inventados como si estuvieran definidos en las tarjetas. Para la ruta principal, solo cancela pedidos en estado RECIBIDO. Si el estado es EN_PREPARACION, exige autorización del Encargado antes de continuar y representa tanto la autorización como su denegación; si se deniega, no confirmes ni registres la cancelación. No canceles automáticamente pedidos LISTO o ENTREGADO. Registra al responsable y, si corresponde, al Encargado que autorizó. La notificación puede representarse como una interacción de sistema, pero no le atribuyas responsabilidades de una tarjeta CRC inexistente.

Por favor, cumple estrictamente con los siguientes requisitos técnicos para el código PlantUML:

1. Participantes: Incluye al menos 4 participantes (actores u objetos). El primer participante (a la izquierda) DEBE ser el actor que inicia el escenario o la interfaz.
2. Interacciones: Cada participante debe enviar o recibir un mínimo de 3 mensajes.
3. Nomenclatura de mensajes: Los mensajes en las flechas deben estar escritos en `camelCase` y sus argumentos deben derivarse de las responsabilidades, colaboraciones, propiedades y datos del escenario descritos en las tarjetas CRC, sin atribuirles métodos literales que no enumeran.
4. Notación UML: Utiliza la notación correcta para nombrar a los participantes según corresponda: solo Clase (`Clase`), solo objeto (`:objeto`) o ambos (`Clase:objeto`).
5. Ciclo de vida: Si en el escenario se crea un objeto temporal que luego no persiste (ej. una Notificacion), debes indicar explícitamente su destrucción al final de su línea de vida utilizando el comando `destroy` de PlantUML (para que aparezca la X al final de la línea).
```

## 4. Diagrama de Secuencia 4: Cambiar Estado Pedido - Avance de Preparación

**Archivo generado:** `diagramas/05-diagramas-secuencia/05-secuencia-cambiar-estado-pedido-avance-preparacion-04.puml`

### Contexto de Entrada Utilizado

Para otorgar contexto preciso a GitHub Copilot, se mantuvieron abiertos y referenciados los siguientes archivos de la Actividad N°2:

* `diagramas/03-escenarios-casos-de-uso/03-cambiar-estado-pedido-avance-preparacion-01.md`
* `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
* `herramientas-agile/tarjetas-crc/12-tarjeta-crc-historial-cambio-pedido.md`
* `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`
* `herramientas-agile/tarjetas-crc/15-tarjeta-crc-usuario-cocina.md`

### Prompt Enviado a Copilot

```text
Actúa como un Especialista en Diseño Orientado a Objetos y experto en UML. Tu tarea es generar el código PlantUML para un Diagrama de Secuencia. Para hacerlo, debes basarte ESTRICTAMENTE en el flujo principal y alternativo del escenario descrito en el archivo diagramas/03-escenarios-casos-de-uso/03-cambiar-estado-pedido-avance-preparacion-01.md y en las responsabilidades, colaboraciones y propiedades definidas en las tarjetas CRC adjuntas: herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md, herramientas-agile/tarjetas-crc/12-tarjeta-crc-historial-cambio-pedido.md, herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md y herramientas-agile/tarjetas-crc/15-tarjeta-crc-usuario-cocina.md. Las tarjetas CRC describen responsabilidades, colaboradores y propiedades, no una lista literal de métodos: deriva de ellas los mensajes necesarios sin presentar nombres de métodos inventados como si estuvieran definidos en las tarjetas. Muestra exclusivamente las transiciones RECIBIDO -> EN_PREPARACION -> LISTO -> ENTREGADO: Cocina realiza las dos primeras y Usuario de Mostrador registra la entrega. Valida cada transición y no cambies el estado si no es válida o el actor no está autorizado.

Por favor, cumple estrictamente con los siguientes requisitos técnicos para el código PlantUML:

1. Participantes: Incluye al menos 4 participantes (actores u objetos). El primer participante (a la izquierda) DEBE ser el actor que inicia el escenario o la interfaz.
2. Interacciones: Cada participante debe enviar o recibir un mínimo de 3 mensajes.
3. Nomenclatura de mensajes: Los mensajes en las flechas deben estar escritos en `camelCase` y sus argumentos deben derivarse de las responsabilidades, colaboraciones, propiedades y datos del escenario descritos en las tarjetas CRC, sin atribuirles métodos literales que no enumeran.
4. Notación UML: Utiliza la notación correcta para nombrar a los participantes según corresponda: solo Clase (`Clase`), solo objeto (`:objeto`) o ambos (`Clase:objeto`).
5. Ciclo de vida: Si en el escenario se crea un objeto temporal que luego no persiste (ej. una Notificacion), debes indicar explícitamente su destrucción al final de su línea de vida utilizando el comando `destroy` de PlantUML (para que aparezca la X al final de la línea).
```

## 5. Diagrama de Secuencia 5: Consultar Pedidos Activos - Prioridad Urgentes

**Archivo generado:** `diagramas/05-diagramas-secuencia/05-secuencia-consultar-pedidos-activos-prioridad-urgentes-05.puml`

### Contexto de Entrada Utilizado

Para otorgar contexto preciso a GitHub Copilot, se mantuvieron abiertos y referenciados los siguientes archivos de la Actividad N°2:

* `diagramas/03-escenarios-casos-de-uso/03-consultar-pedidos-activos-prioridad-urgentes-01.md`
* `herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md`
* `herramientas-agile/tarjetas-crc/13-tarjeta-crc-usuario.md`
* `herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md`
* `herramientas-agile/tarjetas-crc/15-tarjeta-crc-usuario-cocina.md`

### Prompt Enviado a Copilot

```text
Actúa como un Especialista en Diseño Orientado a Objetos y experto en UML. Tu tarea es generar el código PlantUML para un Diagrama de Secuencia. Para hacerlo, debes basarte ESTRICTAMENTE en el flujo principal y alternativo del escenario descrito en el archivo diagramas/03-escenarios-casos-de-uso/03-consultar-pedidos-activos-prioridad-urgentes-01.md y en las responsabilidades, colaboraciones y propiedades definidas en las tarjetas CRC adjuntas: herramientas-agile/tarjetas-crc/01-tarjeta-crc-pedido.md, herramientas-agile/tarjetas-crc/13-tarjeta-crc-usuario.md, herramientas-agile/tarjetas-crc/14-tarjeta-crc-usuario-mostrador.md y herramientas-agile/tarjetas-crc/15-tarjeta-crc-usuario-cocina.md. Las tarjetas CRC describen responsabilidades, colaboradores y propiedades, no una lista literal de métodos: deriva de ellas los mensajes necesarios sin presentar nombres de métodos inventados como si estuvieran definidos en las tarjetas. Incluye el acceso tanto de Cocina como de Usuario de Mostrador al monitor. Consulta únicamente pedidos en estados RECIBIDO, EN_PREPARACION y LISTO; agrúpalos o fíltralos por estado y prioriza los urgentes dentro de cada mismo estado. Adapta la vista al actor y contempla la vista vacía y el aviso de posible desactualización ante una falla de conexión. No atribuyas responsabilidades CRC a clases auxiliares que no tengan una tarjeta; si las representas, trátalas solo como componentes de control o presentación del sistema.

Por favor, cumple estrictamente con los siguientes requisitos técnicos para el código PlantUML:

1. Participantes: Incluye al menos 4 participantes (actores u objetos). El primer participante (a la izquierda) DEBE ser el actor que inicia el escenario o la interfaz.
2. Interacciones: Cada participante debe enviar o recibir un mínimo de 3 mensajes.
3. Nomenclatura de mensajes: Los mensajes en las flechas deben estar escritos en `camelCase` y sus argumentos deben derivarse de las responsabilidades, colaboraciones, propiedades y datos del escenario descritos en las tarjetas CRC, sin atribuirles métodos literales que no enumeran.
4. Notación UML: Utiliza la notación correcta para nombrar a los participantes según corresponda: solo Clase (`Clase`), solo objeto (`:objeto`) o ambos (`Clase:objeto`).
5. Ciclo de vida: Si en el escenario se crea un objeto temporal que luego no persiste (ej. una Notificacion), debes indicar explícitamente su destrucción al final de su línea de vida utilizando el comando `destroy` de PlantUML (para que aparezca la X al final de la línea).
```