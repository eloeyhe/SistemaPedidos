# Registro de Code Reviews asistidas con IA - Coordinador y DevOps

**Rol:** Coordinador y DevOps

**Autor:** Santiago Medel Marquez

**Herramienta utilizada:** Claude (modelo Claude Opus 5.5) en modo agente

> **Aclaración:** la consigna indica GitHub Copilot Agent Mode. Se utilizó Claude en modo agente, que lee los archivos de la PR y del repositorio como contexto. Se documenta la herramienta real utilizada.

---

## 1. Tareas de coordinación

- **Correcciones de la Actividad Obligatoria N°4 (tarea crítica):** se resolvieron todos los Request Changes del docente sobre `release/actividad-obligatoria-4` mediante ramas `fix/` (PRs #266 a #273). La release se aprobó con LGTM y se mergeó a `master` (PR #265).
- **Backport a `develop`:** se creó `backport/release-actividad-obligatoria-4` desde `master` actualizada y se integró en `develop` (PR #274, aprobada por otro integrante). Así `develop` quedó idéntica a `master` como base del Segundo Parcial.
- **Estructura de la Parte 1:** se comparó el repositorio, archivo por archivo, contra el árbol de archivos esperados de la consigna y se corrigió lo que no coincidía:
  - `01_boceto_inicial.png` → `01-boceto-inicial.png` e `ia/a2/diseniador-tarjetas-crc.md` → `disenador-tarjetas-crc.md` (PR #276).
  - Los cinco escenarios terminaban en `-01`; se renumeraron del `-01` al `-05` en secuencia por caso de uso, igual que los demás diagramas (PR #281).
  - Las PRs #276 y #281 no vinculaban sus issues con `Closes`. Por eso las Issues #275 y #282 se cerraron a mano, con un comentario que apunta a la PR que las resolvió, y se borraron las ramas `fix/` ya mergeadas.
  - El índice de diagramas de clases se mantiene como `diagramas_de_clases.md`: así figura en el árbol de la Parte 1, es el nombre aprobado en la Actividad Obligatoria N°4 y el docente lo confirmó en la consulta del 20/11 (la mención a `diagramas_clases.md` era un error de la consigna, ya corregido).
  - Se tuvieron en cuenta las observaciones del docente de la Actividad Obligatoria N°4: títulos literales de las PRs en el changelog, nombres de clase en PascalCase, tarjetas CRC para todas las clases usadas, ningún archivo fuera de la estructura pedida, issues cerradas y solo las ramas pedidas al momento de la entrega.
- **Consulta al docente (20/11) y decisiones tomadas:**
  - *Ramas para las correcciones de la Parte 1:* el docente indicó que van en la feature del Coordinador (`feature/coord-devops-update-docs`) y que las ramas `fix/` quedan reservadas para la release. Las PRs #276 y #281 ya se habían integrado como ramas `fix/` hacia `develop` antes de la respuesta. Como el historial no se reescribe, se dejan documentadas así, y toda corrección posterior de la Parte 1 se hace en `feature/coord-devops-update-docs`.
  - *Matrices CLAE y análisis JAD:* el docente confirmó que quedaron en la consigna por error y que no se entregan, porque el análisis funcional de la Actividad Obligatoria N°4 ya cubre ese trabajo.
  - *`ia/a4/analista-cu-1.md`:* se mantiene; su ausencia en el árbol era un error de tipeo de la consigna.
  - *Code reviews:* el mínimo de 4 se cumple con las PRs de los tres especialistas más la revisión de una PR de correcciones. Además, se pidió que la PR del Coordinador también la revise con IA otro integrante, y no solo la apruebe.
- **Índices de navegación:** se creó `anexos/patrones-diseno/patrones_diseno.md` y se actualizaron `diagramas/01-diagrama-clases/diagramas_de_clases.md`, `diagramas/diagramasUML.md`, `anexos/anexos.md` y `README.md` con los tres patrones de diseño.

---

## 2. Prompt base para las code reviews del Segundo Parcial

Este prompt se utiliza en cada PR de los especialistas, cambiando la categoría del patrón, la rama y los archivos revisados:

```text
Actúa como un Senior Software Engineer realizando code review profesional.
Utiliza como referencia anexos/introduccion.md para el dominio del proyecto (Sistema de Pedidos de Sabor Kiosco).
Estás analizando una Pull Request activa del Segundo Parcial: aplicación de un patrón de diseño GoF de la categoría
<CATEGORIA> sobre el diagrama de clases final del sistema.

Archivos de la PR:
- anexos/patrones-diseno/patron-de-diseno-<categoria>.md
- diagramas/01-diagrama-clases/01-patron-<categoria>-<nombre>.puml y .png
- ia/segundo-parcial/especialista-patron-<categoria>.md
- changelog.md

Usa como contexto:
- diagramas/01-diagrama-clases/06-clases-diagrama-final.puml (diagrama base)
- diagramas/01-diagrama-clases/01-solid-01-srp.puml a 01-solid-05-dip.puml
- herramientas-agile/tarjetas-crc/ (nombres y responsabilidades de las clases)

Revisa especialmente:
- Que el patrón elegido pertenezca realmente a la categoría indicada y resuelva un problema concreto del sistema.
- Que el documento tenga todas las secciones de la plantilla, en orden: relación de la categoría con SOLID,
  Propósito y Tipo, Motivación, Estructura de Clases (imagen incrustada y enlace al diagrama) y Justificación Técnica.
- Que no queden textos de la plantilla sin completar (paréntesis de instrucciones, "NombrePatrónElegido", etc.).
- Que las clases del dominio usadas en el diagrama existan en el diagrama final con el mismo nombre (PascalCase)
  y que sus métodos sean coherentes con los del diagrama final.
- Que el diagrama muestre los roles del patrón (por ejemplo Context/Strategy, Creator/Product, Component/Composite)
  con atributos tipados, métodos con parámetros y retorno, y relaciones UML correctas con multiplicidades.
- Que el .png corresponda al .puml y que la imagen y el enlace del documento funcionen.
- Que cada clase nueva del patrón (que no esté en 06-clases-diagrama-final.puml) esté descrita en la Justificación
  Técnica. Si no tiene tarjeta CRC, informarlo solo como recomendación Baja: la consigna no lo exige.
- Que cada hallazgo indique si lo exige la consigna o si es una recomendación, y no bloquear la PR por recomendaciones.
- Que los "Closes #N" de la PR apunten a las issues del especialista para esta entrega y estén abiertas.
- Que el registro de IA tenga el prompt en bloque de código, los archivos de contexto, los ajustes y las iteraciones.
- Que la rama siga el formato feature/esp-patron-<categoria>-add-patron-<nombre> y que el changelog tenga la entrada
  con el título literal de la PR, la rama, el Issue y el autor con su rol.

INSTRUCCIONES IMPORTANTES:
- Identifica problemas reales en los archivos de la PR
- Enumera los hallazgos (1, 2, 3…)
- Sé claro, técnico y concreto
- No inventes problemas sin evidencia en los archivos
- No modifiques ningún archivo

Para cada hallazgo usa esta estructura:
HALLAZGO #<número> — Severidad: Alta | Media | Baja
Archivo:
Línea:
Problema:
Sugerencia:

Al final, indica la decisión sugerida para la PR (Approve o Request changes) y qué hallazgos la bloquean.
```

---

## 3. Code Reviews realizadas

| # | PR | Rama | Autor | Resultado |
|---|----|------|-------|-----------|
| 1 | [#289](https://github.com/eloeyhe/SistemaPedidos/pull/289) | `feature/esp-patron-estructural-add-patron-decorator` | @nachovelesquen7-rgb | [Request changes](https://github.com/eloeyhe/SistemaPedidos/pull/289#pullrequestreview-5987153385) (9 hallazgos) |
| 2 | [#298](https://github.com/eloeyhe/SistemaPedidos/pull/298) | `feature/esp-patron-creacional-add-patron-builder` | @Leandro2107 | [Request changes](https://github.com/eloeyhe/SistemaPedidos/pull/298#pullrequestreview-6002643258) (4 hallazgos) |
| 3 | Pendiente | `feature/esp-patron-comportamiento-add-patron-<nombre>` | @eloeyhe | Pendiente |
| 4 | [#289](https://github.com/eloeyhe/SistemaPedidos/pull/289) (correcciones, commit `b0e7b71`) | `feature/esp-patron-estructural-add-patron-decorator` | @nachovelesquen7-rgb | [Request changes](https://github.com/eloeyhe/SistemaPedidos/pull/289#pullrequestreview-6003111880) (3 hallazgos nuevos; 8 de 11 anteriores corregidos) |

### Code Review 1 — PR #289 (Patrón estructural Decorator)

**Prompt utilizado:** el prompt base de la sección 2, con `<CATEGORIA>` = estructural y los archivos de la PR:

```text
<CATEGORIA> = estructural
Rama: feature/esp-patron-estructural-add-patron-decorator
Archivos de la PR:
- patrones-diseno/patron-de-diseño-estructural.md
- diagramas/01-diagrama-clases/01-patron-estructural-decorator.puml y .png
- ia/segundo-parcial/especialista-patron-estructural.md
- changelog.md
Compara además la ruta y el nombre de cada archivo contra la estructura de carpetas de la consigna
y contra las Issues #283 a #288 de la PR.
```

**Output generado por la IA (resumen):**
1. *Alta.* El anexo está en `patrones-diseno/` (raíz) y con "ñ" en el nombre. La consigna y la Issue #286 piden `anexos/patrones-diseno/patron-de-diseno-estructural.md`.
2. *Alta.* Los enlaces `../diagramas/...` se rompen al mover el archivo.
3. *Media.* Los títulos no siguen la plantilla de la consigna (Propósito y Tipo, Motivación, Estructura de Clases, Justificación Técnica).
4. *Media.* Falta el enlace "Ver diagrama en tamaño completo" debajo de la imagen.
5. *Media.* El problema se plantea como hipotético ("si en el futuro…", "el diagrama no demuestra errores").
6. *Media.* Falta la explicación del flujo estructural paso a paso.
7. *Media.* Changelog: `## Added` en lugar de `### Added`, título no literal (punto final), falta la Issue #283 y el rol está sin tilde.
8. *Media.* La PR no vincula las Issues #283–#288 con `Closes`.
9. *Baja.* El `.puml` empieza con una línea vacía y tiene un nombre que no coincide con el archivo; incluye clases que no participan del patrón; el registro de IA cita `01-diagrama-clases-final.puml.`, que no existe.

Decisión sugerida: Request changes (los hallazgos 1 a 5 bloquean el merge).

**Ajustes aplicados antes de publicar el comentario:**
- Se verificó cada hallazgo contra el contenido de la rama y se agregaron los números de línea reales.
- Se verificó que el `.png` corresponde al `.puml`. La IA no lo había comprobado y no hubo hallazgo sobre eso.
- Se bajó a *Baja* y se marcó como opcional el pedido de quitar del diagrama las clases que no participan del patrón: no rompe la consigna, solo la mejora.
- Se agregó al hallazgo 5 una redacción alternativa concreta, basada en `ItemPedido.calcularSubtotal(cantidad, precioBase, costosAdicionales)` del diagrama final, para que el especialista no tenga que reinventar el problema.

**Ampliación de la review 1 (20/11) — hallazgos que la primera pasada no detectó:**

Al auditar el repositorio con los criterios de corrección del docente (rúbrica del rol y Request Changes de la Actividad Obligatoria N°4), se volvió a revisar la PR #289 sin código nuevo del especialista (último commit `0d5e993`). Se agregaron dos hallazgos y se publicaron como comentario en la PR:

10. *Alta (luego corregido a Baja, ver abajo).* Las cuatro clases nuevas del patrón (`IElementoMenu`, línea 13; `PrecioBaseHistorico`, línea 50; `DecoradorElementoMenu`, línea 56; `DecoradorPersonalizacion`, línea 61 de `01-patron-estructural-decorator.puml`) no tienen tarjeta CRC. En la Actividad Obligatoria N°4 el docente exigió tarjeta CRC indexada y enlazada para toda clase usada en un diagrama (RC11 a RC15). Se asignaron las tarjetas 22 a 25.
11. *Media.* La descripción de la PR incluye `Closes #236`, una issue de la Actividad Obligatoria N°4 que ya está cerrada, y no vincula la issue principal del especialista (#283).

**Por qué no se detectaron en la primera pasada:** el prompt base no pedía verificar tarjetas CRC para las clases nuevas ni contrastar los `Closes` con las issues del especialista. Se agregaron esas dos verificaciones al prompt base para las reviews siguientes.

**Corrección del hallazgo 10 (20/11):** al contrastar el hallazgo con la consigna del Segundo Parcial, se comprobó que ni las responsabilidades del especialista ni su rúbrica piden tarjetas CRC para las clases nuevas del patrón. En el árbol de archivos, `herramientas-agile/tarjetas-crc/` no figura como nuevo ni actualizado. El hallazgo venía del criterio que el docente aplicó en la Actividad Obligatoria N°4 (RC11 a RC15), no de la consigna. Por eso se bajó a *Baja*, como recomendación opcional y no bloqueante, y se informó en la PR. El contenido de las tarjetas 22 a 25 queda redactado, por si el docente lo pide en la corrección de la release.

### Code Review 2 — PR #298 (Patrón creacional Builder)

**Prompt utilizado:** el prompt base de la sección 2, ya con las verificaciones agregadas después de la review 1. Valores usados:

```text
<CATEGORIA> = creacional
Rama: feature/esp-patron-creacional-add-patron-builder
Archivos de la PR:
- anexos/patrones-diseno/patron-de-diseno-creacional.md
- diagramas/01-diagrama-clases/01-patron-creacional-builder.puml y .png
- ia/segundo-parcial/especialista-patron-creacional.md
- changelog.md
Compara cada firma de las clases existentes en el diagrama del patrón contra
06-clases-diagrama-final.puml y señala si el anexo justifica cada cambio.
Verifica que los Closes apunten a las Issues #291 a #297 del especialista.
```

**Output generado por la IA (resumen):**
1. *Media.* La entrada del changelog no reproduce literalmente el título de la PR.
2. *Media.* `iniciarNuevoPedido()` pasa de devolver `Pedido` a devolver `void`, y `marcarPrioritario(numeroPedido: int)` pasa a `marcarPrioritario()`, sin justificación en el anexo. En el diagrama final, `marcarPrioritario` actúa sobre un pedido ya registrado y `PantallaPedido` la sigue invocando con el número.
3. *Baja.* `**Propósito**` y `**Tipo**` no llevan los dos puntos de la plantilla.
4. *Baja.* El paso 5 del flujo no indica quién crea el `Pago` ni quién invoca `conPago(pago)`.

Decisión sugerida: Request changes (hallazgos 1 y 2).

**Ajustes aplicados antes de publicar el comentario:**
- Se comprobó visualmente que el `.png` coincide con el `.puml`, y no hubo hallazgo sobre eso.
- Se marcó en cada hallazgo si lo exige la consigna o si es una recomendación. Es la lección del hallazgo 10 de la review 1.
- En el hallazgo 2 se agregó una alternativa concreta: conservar `marcarPrioritario(numeroPedido: int)` y nombrar distinto la operación de construcción.
- En el hallazgo 4 se tomó como referencia la decisión vigente del CU1: el `Pago` se crea cuando se aprueba el cobro.
- La IA había propuesto como hallazgo que las Issues no se cerrarían solas por ir a `develop`. Se pasó a "nota informativa": no es un error del especialista, y el cierre manual lo hace el coordinador después del merge.

### Code Review 4 — PR #289, re-revisión de las correcciones (commit `b0e7b71`)

**Prompt utilizado:**

```text
Re-revisá la PR #289 después del commit b0e7b71 "Realizo correcciones solicitadas por el coordinador".
Para cada hallazgo de la review anterior (1 a 11) indicá si quedó Corregido, Parcial o Pendiente,
citando la línea actual. Después buscá hallazgos nuevos introducidos por las correcciones con el
mismo formato (Severidad / Archivo / Línea / Problema / Sugerencia) y verificá que los enlaces
de los índices (patrones_diseno.md, README) apunten al nombre real del anexo.
```

**Output generado por la IA (resumen):**
- **Corregidos:** 2, 4, 5, 6, 7, 8/11 y 9 (a) y (c). El punto 9 (b) y el 10 eran opcionales.
- **Parciales:** el 1 (el anexo se movió, pero el nombre sigue con "ñ") y el 3.
- **Nuevos:**
  - 12. *Alta.* Por el nombre con "ñ" se rompe el enlace del índice `patrones_diseno.md` y del README.
  - 13. *Media.* "Problema identificado" está duplicado, queda la numeración vieja en "3.3. Solución propuesta" y faltan los apartados **Propósito:** y **Tipo:**.
  - 14. *Baja.* La lista de archivos de la descripción de la PR está desactualizada y #283 figura como `Related to`.

**Ajustes aplicados antes de publicar el comentario:**
- Se comprobó que el `.png` no necesitaba regenerarse: el cambio del `.puml` solo quitó la línea vacía y el nombre del `@startuml`, y ninguno de los dos afecta al render.
- La IA marcaba como hallazgo el "si en el futuro…" de la línea 192. Se descartó, porque ahí se usa para ilustrar OCP y no para plantear el problema, que ya es concreto.
- Se agregó al hallazgo 12 el comando `git mv` exacto.
