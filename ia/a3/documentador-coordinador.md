# Registro de Code Reviews asistidas con IA - Documentador y Coordinador

## Code Review 1: PR [#182] - Especialista en Diagramas de Secuencia
* **Rama revisada:** `feature/esp-secuencia-add-diagrama-secuencia-1`
* **Autor:** Eloy Eyheramendy

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
Utiliza como referencia anexos/introduccion.md para el dominio del proyecto.
Estás analizando los cambios de una Pull Request activa: diagramas de secuencia en PlantUML de la Actividad Obligatoria N°3 (rama feature/esp-secuencia-*, carpeta diagramas/05-diagramas-secuencia/).
Para verificar la trazabilidad, usa también como referencia el escenario de caso de uso correspondiente en diagramas/03-escenarios-casos-de-uso/ y las tarjetas CRC de las clases involucradas en herramientas-agile/tarjetas-crc/.

Revisa especialmente:
- Que cada diagrama tenga al menos 4 participantes, con el actor o la interfaz que inicia el escenario como primer participante a la izquierda.
- Que cada participante tenga al menos 3 mensajes con otros participantes, en orden cronológico claro.
- Que los mensajes estén en camelCase e indiquen sus argumentos (por ejemplo notificarCocina(pedido, items)).
- Que los participantes usen la notación UML correcta: Clase, :objeto o Clase:objeto.
- Que los objetos temporales tengan destroy (X al final de la línea de vida).
- Que los pasos, actores, clases y mensajes sean coherentes con el escenario de A2, las tarjetas CRC y el dominio de introduccion.md, con la misma nomenclatura.
- Que existan el .puml y el .png con la nomenclatura 05-secuencia-nombre-caso-uso-nombre-escenario-NN, que el .png coincida con el .puml, y que el índice diagramas_de_secuencias.md y el changelog.md estén actualizados.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales en los archivos de la PR
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en los archivos
- No incluyas sugerencias de tests
- No modifiques ningún archivo

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(notación UML | trazabilidad | completitud | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Al final, agrega una línea con la decisión final sugerida (aprobar o solicitar cambios) y su justificación breve.
```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `diagramas/03-escenarios-casos-de-uso/<COMPLETAR: escenario del diagrama revisado>`
* `herramientas-agile/tarjetas-crc/<COMPLETAR: tarjetas CRC de las clases que aparecen en el diagrama>`

### 3. Output obtenido de Copilot

### HALLAZGO #1

* **Archivo:** `05-secuencia-tomar-pedido-registro-de-pedido-01.puml`
* **Línea:** 9, 21-23
* **Tipo de problema:** Trazabilidad
* **Severidad:** Media
* **Explicación técnica:** El flujo modela únicamente `Producto` y un ciclo de selección de productos. Sin embargo, el escenario A2 incluye productos o combos, y el dominio especifica que un combo tiene precio propio, distinto de la suma de sus componentes. Así, el diagrama no representa una opción prevista en el caso de uso ni cómo se obtiene su precio. Esto se refleja en `03-tomar-pedido-registro-pedido-01.md`, `introduccion.md` y `09-tarjeta-crc-combo.md`.
* **Sugerencia de mejora:** Representar la selección de productos y combos como alternativas del flujo, consultando el precio propio del `Combo` cuando corresponda.

---

### HALLAZGO #2

* **Archivo:** `05-secuencia-cancelar-pedido-cancelacion-antes-entrega-03.puml`
* **Línea:** 7-8, 14
* **Tipo de problema:** Trazabilidad
* **Severidad:** Media
* **Explicación técnica:** El diagrama inicia la solicitud exclusivamente desde `UsuarioMostrador`. `UsuarioEncargado` aparece solo para responder a una solicitud en estado `EN_PREPARACION`. En cambio, el escenario A2 establece que el Usuario de Mostrador o el Encargado puede seleccionar el pedido y solicitar la cancelación, incluyendo el flujo principal. La secuencia no contempla el inicio por el Encargado, aunque esa responsabilidad también es coherente con su tarjeta CRC: `03-cancelar-pedido-cancelacion-antes-entrega-01.md` y `16-tarjeta-crc-usuario-encargado.md`.
* **Sugerencia de mejora:** Incorporar el inicio de la solicitud por `UsuarioEncargado` y conectarlo con el flujo de consulta, confirmación y cancelación que corresponda.

---

### HALLAZGO #3

* **Archivo:** `05-secuencia-consultar-pedidos-activos-prioridad-urgentes-05.puml`
* **Línea:** 26-28, 55-57
* **Tipo de problema:** Completitud
* **Severidad:** Baja
* **Explicación técnica:** `FiltroUrgentes` intercambia solo dos mensajes con otro participante en cada alternativa: recibe una solicitud de `ServicioPedidos` y devuelve el resultado. El tercer mensaje es una llamada a sí mismo (`ordenarUrgentesPorEstado`). Por lo tanto, no alcanza el requisito de al menos tres mensajes con otros participantes para cada participante; que el flujo aparezca duplicado en alternativas excluyentes no suma interacciones dentro de una ejecución.
* **Sugerencia de mejora:** Añadir una tercera interacción relevante de `FiltroUrgentes` con otro participante, o ajustar el diagrama para que cada participante cumpla el mínimo requerido sin contar mensajes a sí mismo.

---

### HALLAZGO #4

* **Archivo:** `diagramas_de_actividades.md`
* **Línea:** 1
* **Tipo de problema:** Completitud
* **Severidad:** Baja
* **Explicación técnica:** El archivo contiene un índice de diagramas de secuencia, pero su nombre indica diagramas de actividades. No existe en la PR el índice con la nomenclatura solicitada `diagramas_de_secuencias.md`, por lo que la documentación no queda disponible bajo el nombre esperado.
* **Sugerencia de mejora:** Renombrar el archivo a `diagramas_de_secuencias.md` y mantener allí los enlaces a los PNG.

---

### HALLAZGO #5

* **Archivo:** `changelog.md`
* **Línea:** 7
* **Tipo de problema:** Completitud
* **Severidad:** Baja
* **Explicación técnica:** La sección `[Unreleased]` permanece vacía y la PR no modifica el changelog, por lo que no queda registrado el agregado de los cinco diagramas y sus recursos asociados, pese a que el archivo indica que se actualiza con cada PR.
* **Sugerencia de mejora:** Añadir en `[Unreleased]` una entrada que describa los diagramas de secuencia incorporados.

---

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** <Acepté los cinco hallazgos y los comenté en la PR #182. Se incorporó la alternativa de selección de combos con su precio propio; se permitió que UsuarioEncargado también inicie la cancelación; se agregaron interacciones de FiltroUrgentes con ServicioPedidos; se corrigió el nombre del índice a diagramas_de_secuencias.md; y se registró la PR en changelog.md bajo [Unreleased].>
* **Sugerencias descartadas o modificadas:** <No descarté sugerencias. El hallazgo 5 requirió un ajuste posterior: la entrada del changelog se había agregado bajo un encabezado de release y luego se corrigió para que quedara en [Unreleased].>
* **Decisión final en GitHub:** <Aprobada. La PR se aprobó y se integró en develop. La revisión inicial contenía la solicitud de cambios, aunque GitHub la registró con estado COMMENTED; posteriormente se envió una revisión APPROVED.>

## Code Review 2: PR [#190] - Especialista en Diagramas de Actividades - Casos de uso 1 y 2
* **Rama revisada:** `feature/esp-actividades-1-2-add-diagrama-actividad-1`
* **Autor:** Santiago Marquez

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
Utiliza como referencia anexos/introduccion.md para el dominio del proyecto.
Estás analizando los cambios de una Pull Request activa: diagramas de actividades en PlantUML de la Actividad Obligatoria N°3 (rama feature/esp-actividades-1-2-*, casos de uso 1 y 2, carpeta diagramas/04-diagramas-actividades/).
Para verificar la trazabilidad, usa también como referencia el diagrama de caso de uso correspondiente en diagramas/02-casos-de-uso/ (actores y límite del sistema) y el escenario en diagramas/03-escenarios-casos-de-uso/ (pasos del flujo paso a paso).

Revisa especialmente:
- Que cada diagrama tenga al menos 10 actividades con nombres descriptivos y conectadas lógicamente.
- Que cada diagrama tenga al menos 3 swimlanes (carriles) con las responsabilidades distribuidas correctamente entre los actores o componentes (por ejemplo Usuario de Mostrador/Encargado, SistemaPedidos, Cocina, Cliente, según el caso de uso).
- Que incluya control de flujo: nodos de decisión, bifurcaciones, caminos alternativos, e inicio y fin correctamente marcados.
- Que los pasos del escenario y los actores del caso de uso estén reflejados fielmente en los carriles y las actividades, con la misma nomenclatura de A1 y A2.
- Que los archivos .puml y .png existan con la nomenclatura 04-actividad-nombre-caso-uso-NN, que el .png coincida con el .puml y no muestre advertencias de PlantUML.
- Que el índice diagramas_de_actividades.md tenga el enlace con el formato "Diagrama de actividad - nombre-caso-uso".
- Que la rama use el naming feature/esp-actividades-1-2-*, que la PR vincule su Issue y use el template, que el changelog.md tenga una única entrada para esta PR, y que el proceso esté documentado en ia/a3/esp-actividades-1-2.md (prompt en bloque de código, archivos de contexto, ajustes e iteraciones).

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales en los archivos de la PR
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en los archivos
- No incluyas sugerencias de tests
- No modifiques ningún archivo

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(notación UML | trazabilidad | completitud | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Al final, agrega una línea con la decisión final sugerida (aprobar o solicitar cambios) y su justificación breve.
```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `diagramas/03-escenarios-casos-de-uso/<COMPLETAR: escenario del diagrama revisado>`
* `herramientas-agile/tarjetas-crc/<COMPLETAR: tarjetas CRC de las clases que aparecen en el diagrama>`

### 3. Output obtenido de Copilot

### HALLAZGO #1

* **Archivo:** `04-actividad-tomar-pedido-01.puml`
* **Línea:** 53
* **Tipo de problema:** Trazabilidad
* **Severidad:** Media
* **Explicación técnica:** El único camino para un pedido que no llega a pago aprobado es descartarlo. El escenario A2 también contempla que el Cliente cancele la operación durante la carga, antes del pago; ese caso no aparece como decisión ni actividad diferenciada en el diagrama.
* **Sugerencia de mejora:** Incorporar una alternativa explícita de cancelación previa al pago, iniciada por el Cliente y gestionada por el Usuario de Mostrador, que termine sin registrar el pedido.

---

### HALLAZGO #2

* **Archivo:** `04-actividad-modificar-pedido-02.puml`
* **Línea:** 43
* **Tipo de problema:** Trazabilidad
* **Severidad:** Media
* **Explicación técnica:** Para una diferencia a cobrar, la actividad asigna al Cliente el abono y la elección del medio de pago. El escenario A2 indica que el Usuario de Mostrador selecciona el medio de pago para registrar el ajuste; ese paso y su responsable no están representados.
* **Sugerencia de mejora:** Mostrar al Usuario de Mostrador seleccionando el medio de pago y registrando el cobro adicional; reflejar al Cliente como quien entrega o abona el importe.

---

### HALLAZGO #3

* **Archivo:** `04-actividad-modificar-pedido-02.puml`
* **Línea:** 36
* **Tipo de problema:** Trazabilidad
* **Severidad:** Baja
* **Explicación técnica:** Si la edición deja el pedido sin ítems, el flujo impide guardarlo y vuelve al ciclo de edición. A2 especifica que, en ese caso, el sistema debe sugerir cancelar el pedido mediante el `CU03`; no se representa esa alternativa.
* **Sugerencia de mejora:** Añadir la sugerencia de cancelación mediante `CU03` al camino de pedido sin ítems, además de impedir su guardado.

---

### HALLAZGO #4

* **Archivo:** `diagramas_de_actividades.md`
* **Línea:** 3
* **Tipo de problema:** Completitud
* **Severidad:** Baja
* **Explicación técnica:** Los enlaces del índice usan el rótulo «Diagrama de Actividad - Tomar Pedido», que no sigue el formato solicitado «Diagrama de actividad - nombre-caso-uso»: difiere en mayúsculas y el nombre del caso no está expresado con la nomenclatura `kebab-case`. El mismo patrón aparece en la línea 4 para Modificar Pedido.
* **Sugerencia de mejora:** Ajustar ambos rótulos al formato requerido, por ejemplo «Diagrama de actividad - tomar-pedido» y «Diagrama de actividad - modificar-pedido».

---

### HALLAZGO #5

* **Archivo:** Descripción de la PR #190
* **Línea:** Cuerpo de la descripción
* **Tipo de problema:** Completitud
* **Severidad:** Baja
* **Explicación técnica:** La descripción de la PR incluye un resumen y los Issues vinculados, pero no sigue la estructura del template de PR del repositorio: omite secciones como datos del estudiante, rama, archivos agregados y checklist. Por lo tanto, no cumple el requisito de usar el template.
* **Sugerencia de mejora:** Reemplazar la descripción por el contenido del template de PR aplicable, conservando sus encabezados y completando sus campos para esta entrega.

---

### Decisión final sugerida
Solicitar cambios, porque persisten omisiones en los flujos alternativos y discrepancias con A2, además de incumplimientos en el formato del índice y la descripción de la PR.

---

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** <Acepté los cinco hallazgos de la revisión y los publiqué en la PR #190. Se incorporó al diagrama de Tomar Pedido la alternativa de cancelación del Cliente antes del pago; en Modificar Pedido, se asignó al Usuario de Mostrador la selección del medio de pago y el registro del ajuste; se agregó la sugerencia de cancelar mediante CU03 si la edición deja el pedido sin ítems; y se ajustaron los rótulos del índice al formato solicitado.>
* **Sugerencias descartadas o modificadas:** <No descarté hallazgos. El hallazgo #5 se precisó: la descripción inicial ya incluía varias secciones de la plantilla, así que el problema no era la ausencia general de la estructura. Se actualizó la descripción de la PR para completar y respetar la plantilla del repositorio.>
* **Decisión final en GitHub:** <Aprobada y fusionada en develop. La revisión inicial solicitó cambios; después de las correcciones, se aprobó y la PR se integro>


## Code Review 3: PR [#206] - Especialista en Diagramas de Actividades - Casos de uso 3, 4 y 5
* **Rama revisada:** `feature/esp-actividades-3-4-5-add-diagrama-actividad3`
* **Autor:** Ignacio Velesquen

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
Utiliza como referencia anexos/introduccion.md para el dominio del proyecto.
Estás analizando los cambios de una Pull Request activa: diagramas de actividades en PlantUML de la Actividad Obligatoria N°3 (rama feature/esp-actividades-3-4-5-*, casos de uso 3, 4 y 5, carpeta diagramas/04-diagramas-actividades/).
Para verificar la trazabilidad, usa también como referencia los diagramas de casos de uso 3, 4 y 5 en diagramas/02-casos-de-uso/ (actores y límite del sistema) y sus escenarios en diagramas/03-escenarios-casos-de-uso/ (pasos del flujo paso a paso).

Revisa especialmente:
- Que la PR incluya los 3 diagramas (casos de uso 3, 4 y 5) y no solo el indicado en el nombre de la rama.
- Que cada diagrama tenga al menos 10 actividades con nombres descriptivos y conectadas lógicamente.
- Que cada diagrama tenga al menos 3 swimlanes con las responsabilidades distribuidas correctamente entre los actores o componentes que participan en ese caso de uso (el carril de Cocina puede no aparecer en algunos casos, como Registrar Pago o Modificar Pedido, pero sí es central en Cambiar Estado de Pedido).
- Que incluya control de flujo: nodos de decisión, bifurcaciones, caminos alternativos, e inicio y fin correctamente marcados.
- Que los pasos del escenario y los actores del caso de uso estén reflejados fielmente en los carriles y las actividades, con la misma nomenclatura de A1 y A2.
- Que los archivos .puml y .png existan para cada diagrama con la nomenclatura 04-actividad-nombre-caso-uso-03, -04 y -05, que cada .png coincida con su .puml y no muestre advertencias de PlantUML.
- Que el índice diagramas_de_actividades.md esté actualizado (no recreado) con los enlaces de los casos de uso 3, 4 y 5 en el formato "Diagrama de actividad - nombre-caso-uso", sin pisar los enlaces de los casos 1 y 2.
- Que la rama use el naming feature/esp-actividades-3-4-5-*, que la PR vincule su Issue y use el template, que el changelog.md tenga una única entrada para esta PR, y que el proceso esté documentado en ia/a3/esp-actividades-3-4-5.md (prompt en bloque de código, archivos de contexto, ajustes e iteraciones).

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales en los archivos de la PR
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en los archivos
- No incluyas sugerencias de tests
- No modifiques ningún archivo

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(notación UML | trazabilidad | completitud | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Al final, agrega una línea con la decisión final sugerida (aprobar o solicitar cambios) y su justificación breve.

```

### 3. Output obtenido de Copilot

### HALLAZGO #1

* **Archivo:** `04-actividad-cancelar-pedido-03.puml`
* **Línea:** 45-50
* **Tipo de problema:** Trazabilidad
* **Severidad:** Media
* **Explicación técnica:** El flujo de cancelación cambia el estado a `CANCELADO` y conserva los pagos, pero no representa el registro del motivo, el procesamiento de reembolso/devolución ni la liberación de la comanda, incluidos explícitamente en el caso de uso `CU03` (`02-caso-uso-cancelar-pedido-03.puml`, líneas 19-21 y 39-41). Por ello, el diagrama no cubre todas las responsabilidades que declara ese caso de uso.
* **Sugerencia de mejora:** Agregar las actividades para registrar el motivo, procesar el reembolso o devolución y confirmar la cancelación liberando la comanda; incluir también la alternativa de rechazo del reembolso.

---

### HALLAZGO #2

* **Archivo:** `04-actividad-cambiar-estado-pedido-04.puml`
* **Línea:** 5-7
* **Tipo de problema:** Trazabilidad
* **Severidad:** Baja
* **Explicación técnica:** La selección del pedido y el nodo inicial están asignados exclusivamente al carril `Cocina`, aunque el escenario indica que tanto `Cocina` como `UsuarioMostrador` pueden iniciar el cambio de estado (`03-cambiar-estado-pedido-avance-preparacion-01.md`, línea 8). El carril de `UsuarioMostrador` aparece únicamente para registrar la entrega, así que el diagrama no refleja su participación al iniciar esa transición.
* **Sugerencia de mejora:** Modelar la selección como una acción iniciable por cualquiera de los actores autorizados, o representar las alternativas de inicio en sus respectivos carriles.

---

### HALLAZGO #3

* **Archivo:** `04-actividad-consultar-pedidos-activos-05.puml`
* **Línea:** 4-6
* **Tipo de problema:** Trazabilidad
* **Severidad:** Baja
* **Explicación técnica:** El diagrama ubica el inicio y la actividad de acceso al monitor únicamente en el carril `Cocina`, pero `CU05` identifica también a `UsuarioMostrador` como actor iniciador (`03-consultar-pedidos-activos-prioridad-urgentes-01.md`, líneas 5 y 8). Aunque luego aparece un carril para Mostrador, su actividad está limitada a consultar pedidos `LISTO`; no se muestra cómo inicia la consulta.
* **Sugerencia de mejora:** Representar el acceso al monitor como una acción disponible para ambos actores, o mostrar explícitamente las alternativas de inicio desde `Cocina` y `UsuarioMostrador`.

---

### HALLAZGO #4

* **Archivo:** `diagramas_de_actividades.md`
* **Línea:** 5-7
* **Tipo de problema:** Legibilidad
* **Severidad:** Baja
* **Explicación técnica:** Los enlaces nuevos usan nombres en texto corrido y con mayúsculas («Cancelar Pedido», «Cambiar Estado de Pedido», «Consultar Pedidos Activos»), mientras que la nomenclatura solicitada es `Diagrama de actividad - nombre-caso-uso`, y las entradas existentes para los casos 1 y 2 usan nombres en `kebab-case`. Esto deja el índice inconsistente con el formato requerido.
* **Sugerencia de mejora:** Usar como texto de enlace `Diagrama de actividad - cancelar-pedido`, `Diagrama de actividad - cambiar-estado-pedido` y `Diagrama de actividad - consultar-pedidos-activos`, sin alterar los enlaces de los casos 1 y 2.

---

### HALLAZGO #5

* **Archivo:** `changelog.md`
* **Línea:** 31
* **Tipo de problema:** Otro
* **Severidad:** Media
* **Explicación técnica:** La entrada de esta rama enlaza la PR #117, pero la PR activa revisada es la #206. El enlace y el autor consignado (`@nachovelesquen7-rgb`) corresponden a otra referencia, por lo que el `changelog.md` no permite rastrear correctamente esta contribución.
* **Sugerencia de mejora:** Actualizar la referencia a la PR activa y los datos de autor correspondientes, manteniendo una única entrada para esta contribución.

---

### Decisión final sugerida
Solicitar cambios, porque hay brechas de trazabilidad en dos diagramas y la entrada del changelog.md apunta a una PR incorrecta.

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** <Acepté los hallazgos sobre la falta de trazabilidad de los actores en los diagramas CU04 y CU05, el registro incorrecto de la PR en el changelog y las omisiones del flujo CU03. Los comenté en la PR #206 y, tras las correcciones, revisé que se representaran ambos actores iniciadores, se actualizara el motivo de cancelación y se incluyera el camino cuando el usuario desiste. También acepté ajustar los textos de los enlaces del índice a kebab-case, sin reemplazar los de los casos 1 y 2.>
* **Sugerencias descartadas o modificadas:** <No descarté hallazgos. Ajusté el seguimiento del hallazgo CU03 después de una revisión intermedia: señalé que aún faltaban el motivo de cancelación y la rama de no confirmación; ambos se incorporaron luego. No pedí cambios en los casos CU04, CU05 ni en el changelog tras verificar que esas correcciones estaban hechas.>
* **Decisión final en GitHub:** <Aprobada. La PR #206 recibió mi aprobación y fue fusionada el 22 de octubre de 2026.>

## Code Review 4: PR [#207] - Especialista en Diagramas de Secuencia
* **Rama revisada:** `feature/esp-secuencia-add-diagrama-secuencia-1`
* **Autor:** Eloy Eyheramendy

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
Utiliza como referencia #file:anexos/introduccion.md para el dominio del proyecto.
Estás analizando los cambios de una Pull Request activa: diagramas de secuencia en PlantUML de la Actividad Obligatoria N°3 (rama feature/esp-secuencia-*, carpeta diagramas/05-diagramas-secuencia/).
Para verificar la trazabilidad, usa también como referencia el escenario de caso de uso correspondiente en diagramas/03-escenarios-casos-de-uso/ y las tarjetas CRC de las clases involucradas en herramientas-agile/tarjetas-crc/.

Revisa especialmente:
- Que cada diagrama tenga al menos 4 participantes, con el actor o la interfaz que inicia el escenario como primer participante a la izquierda.
- Que cada participante tenga al menos 3 mensajes con otros participantes, en orden cronológico claro.
- Que los mensajes estén en camelCase e indiquen sus argumentos (por ejemplo notificarCocina(pedido, items)).
- Que los participantes usen la notación UML correcta: Clase, :objeto o Clase:objeto.
- Que los objetos temporales tengan destroy (X al final de la línea de vida).
- Que los pasos, actores, clases y mensajes sean coherentes con el escenario de A2, las tarjetas CRC y el dominio de introduccion.md, con la misma nomenclatura.
- Que existan el .puml y el .png con la nomenclatura 05-secuencia-nombre-caso-uso-nombre-escenario-NN, que el .png coincida con el .puml, y que el índice diagramas_de_secuencias.md y el changelog.md estén actualizados.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales en los archivos de la PR
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en los archivos
- No incluyas sugerencias de tests
- No modifiques ningún archivo

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(notación UML | trazabilidad | completitud | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Al final, agrega una línea con la decisión final sugerida (aprobar o solicitar cambios) y su justificación breve.
```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`
* `diagramas/03-escenarios-casos-de-uso/<COMPLETAR: escenario del diagrama revisado>`
* `herramientas-agile/tarjetas-crc/<COMPLETAR: tarjetas CRC de las clases que aparecen en el diagrama>`

### 3. Output obtenido de Copilot

### HALLAZGO #1

* **Archivo:** `05-secuencia-tomar-pedido-registro-de-pedido-01.puml`
* **Línea:** 77, 78, 79
* **Tipo de problema:** Notación UML
* **Severidad:** Media
* **Explicación técnica:** En el rechazo del pago se destruyen `personalizacion`, `itemProducto` y `itemCombo` sin que necesariamente se hayan creado. Esos objetos se crean dentro de alternativas y bucles: puede no haber personalizaciones o solo haberse seleccionado productos o combos. La destrucción incondicional representa un fin de vida para instancias ausentes.
* **Sugerencia de mejora:** Ajustar las destrucciones a las instancias efectivamente creadas en cada rama, o estructurar el flujo de modo que cada objeto se destruya únicamente donde se creó.

---

### HALLAZGO #2

* **Archivo:** `05-secuencia-modificar-pedido-modificacion-recibido-02.puml`
* **Línea:** 54, 61, 72, 81
* **Tipo de problema:** Trazabilidad
* **Severidad:** Alta
* **Explicación técnica:** El diagrama registra cobros o devoluciones antes de verificar el estado del pedido al confirmar la modificación. Si en esa verificación el estado ya no es `RECIBIDO`, la modificación se rechaza, pero el ajuste financiero ya se registró. Esto contradice la regla RF4 de bloquear modificaciones cuando el pedido deja de estar `RECIBIDO` y puede dejar un pago o devolución asociado a un cambio no aplicado.
* **Sugerencia de mejora:** Validar y asegurar el estado antes de ejecutar el ajuste financiero, o representar el cobro/devolución y la modificación como una operación atómica que no persista el ajuste si se rechaza el cambio.

---

### HALLAZGO #3

* **Archivo:** `05-secuencia-consultar-pedidos-activos-prioridad-urgentes-05.puml`
* **Línea:** 12
* **Tipo de problema:** Trazabilidad
* **Severidad:** Media
* **Explicación técnica:** El diagrama introduce `FiltroUrgentes` como participante de clase y le asigna el agrupamiento y ordenamiento de pedidos, pero no existe una tarjeta CRC para esa clase. En cambio, la tarjeta CRC de `Pedido` ya le asigna la responsabilidad de gestionar la prioridad. El participante queda sin respaldo en las referencias de diseño solicitadas.
* **Sugerencia de mejora:** Alinear esas operaciones con las responsabilidades modeladas en las tarjetas CRC o incorporar una tarjeta CRC para `FiltroUrgentes` que defina su responsabilidad y colaboradores.

---

### Decisión final sugerida
Solicitar cambios, porque el flujo de modificación puede registrar un ajuste de dinero aunque la modificación se rechace, y hay inconsistencias en el ciclo de vida y la trazabilidad de participantes.

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:** <Acepté los tres hallazgos de la revisión y los comenté en la PR #207: el ciclo de vida de los objetos temporales al rechazarse el pago, la posibilidad de registrar ajustes financieros antes de confirmar que el pedido seguía RECIBIDO, y la falta de respaldo CRC y de trazabilidad de actores/prioridades en la consulta de pedidos activos.>
* **Sugerencias descartadas o modificadas:** <No descarté hallazgos. El seguimiento del hallazgo sobre el ajuste financiero se dio por resuelto cuando el cobro y la devolución quedaron condicionados a que el pedido siguiera RECIBIDO. Los otros dos requirieron una segunda corrección: se acotaron las destrucciones a objetos temporales creados y se eliminó ResumenConsulta, representando la agrupación, el orden por prioridad y ambos actores en el flujo de consulta.>
* **Decisión final en GitHub:** <La PR #207 fue aprobada y fusionada el 22 de octubre de 2026.>