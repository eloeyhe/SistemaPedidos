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

# Registro de Code Reviews asistidas con IA - Documentador y Coordinador

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
* **Decisión final en GitHub:** <Aprobada y fusionada en develop. La revisión inicial solicitó cambios; después de las correcciones, se aprobó y la PR se integró.>