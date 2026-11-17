# Registro de Code Reviews asistidas con IA - Coordinador de Repositorio

> **Nota de vigencia de nombres (RC10):** a partir de las correcciones de la release de A4 (PRs #266 y #269), las clases del modelo se nombran en PascalCase: `ItemPedido`, `PersonalizacionItem`, `EstadoPedido`, `MetodoPago`, `TipoMovimiento`, `HistorialPago`, `HistorialCambioPedido` y `AuditoriaPedido`. El texto de este registro usa esos nombres. Los prompts y los outputs de la IA que aparecen en bloques de código se conservan literales, porque documentan lo que se pidió y lo que devolvió la IA en ese momento: las variantes con guion bajo que figuran ahí (por ejemplo `Item_pedido` o `Estado_pedido`) son **contexto histórico no vigente**.

**Rol:** Coordinador de Repositorio + Analista Funcional del Caso de Uso 1

**Autor:** Santiago Medel Marquez

**Herramienta utilizada:** Claude (modelo Claude Opus 5.5) en modo agente

> **Aclaración:** la consigna indica GitHub Copilot Agent Mode. Se utilizó Claude en modo agente, que lee los archivos de la PR y del repositorio como contexto. Se documenta la herramienta real utilizada.

---

## Tareas de coordinación

- **Correcciones de A3:** todos los Request Changes del docente sobre `release/actividad-obligatoria-3` se resolvieron con ramas `fix/` (PRs #211, #212, #214, #217, #219, #221 y #223) y la release se mergeó a `master` (PR #209).
- **Backport:** se creó `backport/release-actividad-obligatoria-3` desde `master` actualizada para llevar las correcciones aprobadas a `develop` y establecer la base de la Actividad Obligatoria N°4 (PR #225, aprobada por otro integrante).
- **Integración de las correcciones del Arquitecto de Dominio:** se revisaron y mergearon a `develop` las ramas `fix/` de las issues #247, #248 y #249 (PRs #257 y #258). Como `develop` no es la rama por defecto, las issues vinculadas se cerraron a mano.
- **Merge erróneo contra `master`:** la PR #259 se mergeó por error a `master` y, como su rama salía de `develop`, arrastró todo el trabajo de A4. La PR #261 lo revirtió, pero eso dejaba esos commits como "ya mergeados y revertidos" en `master`, por lo que la release de A4 los hubiera perdido sin mostrar conflicto. Se integró la aclaración en `develop` (PR #260) y se revirtió el revert hacia `develop` (PR #262, sin cambios de archivos y aprobada por otro integrante). Se verificó con `git merge-tree` que el merge `develop` → `master` queda limpio.
- **Correcciones de la release de A4 (Request Changes de la PR #265):** se usó Claude en modo agente para verificar cada RC contra `release/actividad-obligatoria-4` y para aplicar las correcciones en ramas `fix/`. Las PRs fueron #266, #267 y #268, más #269 (nombres y tarjetas CRC), #270 (CU5, contratos y diagrama final) y #271 (desvío de la #259 y changelog).
  - **Prompt:** "En este repositorio, fijate cuáles de estas correcciones (RC2 a RC23) ya están hechas y cuáles faltan", seguido de "hacé todo".
  - **Decisiones revisadas sobre el output de la IA:**
    1. En CU5 se adoptó la regla del diagrama de casos de uso y de actividades: Mostrador ve solo `LISTO`. La secuencia, el pseudocódigo, la introducción y los diagramas de clases se ajustaron a esa regla.
    2. Para cada contrato se tomó la firma que ya compartían más artefactos. `calcularDiferencia(totalAnterior, totalActualizado)` viene del parcial y la secuencia del CU2, y `registrarAccion(..., accion, fechaHora)` del CU3. `obtenerEstado()`, `cambiarEstado` y `validarTransicion(nuevoEstado, actor)` vienen del diagrama final.
    3. No se renombraron los registros históricos (`ia/`, entradas previas del changelog, boceto `.excalidraw` de A2), porque documentan lo que se hizo en su momento.
    4. Los `.png` se regeneraron con el servidor de PlantUML. El diagrama del CU2 superaba 4096 px y se le agregó `scale 0.9`. Las capturas de `pilares-poo.md` se regeneraron a partir de fragmentos del diagrama final corregido.
  - **Verificación:** se buscaron con `grep` los nombres con guion bajo y las firmas viejas en `.md` y `.puml` (sin resultados fuera de los registros históricos) y se revisaron visualmente los diagramas regenerados.
- **Seguimiento de la release de A4 (RC25 a RC30, PR #273):** se usó Claude en modo agente para hacer la verificación transversal de contratos entre las secuencias, los diagramas de clases parciales, los anexos y el diagrama final.
  - **Prompt:** "tengo que corregir esto", seguido del texto del seguimiento con RC25 a RC30.
  - **Decisiones revisadas sobre el output de la IA:**
    1. La pantalla siempre delega en `ServicioPedidos`. El estado se consulta con `ServicioPedidos.consultarEstado` y `Pedido.obtenerEstado()`, en lugar de usar operaciones propias de cada CU (`verificarEstado`, `consultarPedidoYEstado`).
    2. `ProcesadorPago` es el único validador del medio de pago y lo invoca `ServicioPedidos` en CU1 y en CU2. `Pago` ya no valida.
    3. Política de pagos (RC30): `Pedido "1" -- "1" Pago` (pago original). En CU1 el `Pago` se crea recién cuando se aprueba el cobro. En CU2 los cobros adicionales y las devoluciones son movimientos `AJUSTE` / `DEVOLUCION` en `HistorialPago`. Se reemplazó `consultarPagoOriginal` por `Pedido.obtenerPagoOriginal()`.
    4. CU3 usa `cancelarPedido(...)` del diagrama final y CU5 mueve `obtenerPedidosPorEstados(estados)` a `ServicioPedidos`.
  - **Verificación:** se usó un script que compara cada mensaje de las secuencias, y cada operación de los diagramas parciales, con las operaciones del diagrama final por clase y cantidad de parámetros, sin diferencias. Los `.png` se regeneraron y se revisaron visualmente. Se ajustó la escala de los que superaban 4096 px.

---

## Prompt base para las code reviews de A4

Este prompt se utiliza en cada PR de los demás integrantes, cambiando la rama y los archivos revisados:

```text
Actúa como un Senior Software Engineer realizando code review profesional.
Utiliza como referencia anexos/introduccion.md para el dominio del proyecto.
Estás analizando una Pull Request activa de la Actividad Obligatoria N°4: anexo por caso de uso
(anexos/analisis-funcional/) y diagrama de clases del caso de uso en PlantUML (diagramas/01-diagrama-clases/).
Para verificar la trazabilidad, usa también como referencia las tarjetas CRC de las clases involucradas
en herramientas-agile/tarjetas-crc/ y el diagrama de secuencia del caso de uso en diagramas/05-diagramas-secuencia/.

Revisa especialmente:
- Que el anexo siga la estructura de 00-plantilla-anexo-caso-de-uso.md, en el mismo orden de secciones.
- Que no queden textos entre corchetes sin completar ni comentarios de la plantilla.
- Que cada RF esté citado con su identificador y texto literal de introduccion.md.
- Que las imágenes de casos de uso, actividades, secuencia y clases existan en el repositorio.
- Que las clases tengan atributos tipados y métodos con parámetros y tipo de retorno.
- Que las relaciones UML (asociación, agregación, composición, dependencia y herencia) sean correctas y tengan multiplicidades.
- Que los nombres de clases, responsabilidades y colaboraciones coincidan con las tarjetas CRC.
- Que los participantes y mensajes del diagrama de secuencia se correspondan con clases y métodos del diagrama.
- Que el pseudocódigo no use sintaxis de un lenguaje concreto y siga el flujo principal.
- Que existan el .puml y el .png con la nomenclatura 0N-clases-nombre-caso-uso-0N, y que los índices y el changelog.md estén actualizados.

INSTRUCCIONES IMPORTANTES:
- Identifica problemas reales en los archivos de la PR
- Enumera los hallazgos (1, 2, 3…)
- Sé claro, técnico y concreto
- No inventes problemas sin evidencia en los archivos
- No modifiques ningún archivo

Para cada hallazgo usa esta estructura:
HALLAZGO #<número>
Archivo:
Línea:
Problema:
Sugerencia:
```

---

## Code Reviews realizadas

Las reviews se hicieron con Claude en modo agente sobre la rama de cada PR. Los hallazgos validados se trasladaron como comentario general de la review en GitHub.

| # | PR | Rama | Autor | Resultado |
|---|----|------|-------|-----------|
| 1 | [#234](https://github.com/eloeyhe/SistemaPedidos/pull/234) | `feature/analista-cu-4-5-add-anexo-cu4-cu5` | Analista Funcional CU4 y CU5 (@eloeyhe) | Request changes (2 rondas) → Aprobada |
| 2 | [#246](https://github.com/eloeyhe/SistemaPedidos/pull/246) | `feature/analista-cu-2-3-add-anexo-cu2-cu3` | Analista Funcional CU2 y CU3 (@nachovelesquen7-rgb) | Request changes (2 rondas) → Aprobada |
| 3 | [#256](https://github.com/eloeyhe/SistemaPedidos/pull/256) | `feature/arquitecto-dominio-add-diagrama-final` | Arquitecto de Dominio (@Leandro2107) | Request changes → Aprobada |
| 4 | [#257](https://github.com/eloeyhe/SistemaPedidos/pull/257) | `fix/unificar-nombre-servicio-pedidos` | Arquitecto de Dominio (@Leandro2107) | Aprobada, con una corrección agregada por el coordinador |

### Review 1 - PR #234

**Rama:** `feature/analista-cu-4-5-add-anexo-cu4-cu5`
**Archivos de contexto referenciados:** anexos de CU4 y CU5, .puml/.png de clases de CU4 y CU5, tarjetas CRC involucradas, diagramas de secuencia de CU4 y CU5, `changelog.md` e índice de diagramas de clases.

**Prompt utilizado:** prompt base de este documento, cambiando la rama y los archivos.

**Hallazgos de la IA:**
1. `changelog.md` se pisó con una versión vieja: se borraron las entradas de #225 y #226, el título de AO3 quedó como AO2 y se perdieron los `[Fixed]` de A3.
2. `ia/a4/analista-cu-4-5.md` cita rutas de `.puml` que no existen (`cambiar-estado-de-pedido` en lugar de `cambiar-estado-pedido`).
3. `diagramas_de_clases.md`: typo "Estdo" y entradas sin el mismo formato que el resto.
4. Descripción de la PR con datos de la plantilla sin completar (rol, año, textos opcionales y "Closes" en cada issue).

**Ajustes al output de la IA:** en la segunda ronda se volvió a revisar el changelog y se detectó que seguía modificando entradas de AO1/AO2 y que el link de la PR quedó vacío; se pidió copiar el changelog de `develop` y agregar solo la entrada nueva.

**Resultado:** Request changes (2 rondas) → Aprobada.

### Review 2 - PR #246

**Rama:** `feature/analista-cu-2-3-add-anexo-cu2-cu3`
**Archivos de contexto referenciados:** anexos de CU2 y CU3, .puml/.png de clases de CU2 y CU3, tarjetas CRC involucradas, diagramas de secuencia de CU2 y CU3, diagrama de clases de CU1 como referencia de nomenclatura.

**Prompt utilizado:** prompt base de este documento, cambiando la rama y los archivos.

**Hallazgos de la IA:**
1. Los enums `EstadoPedido`, `MetodoPago` y `TipoMovimiento` estaban en minúscula, a diferencia de CU1, CU4 y CU5.
2. En CU2, `HistorialPago` e `HistorialCambioPedido` repetían como atributo lo que ya estaba dibujado como asociación.
3. Typos: "Responsability" en `diagramas_de_clases.md` y "Funcioanal" en el changelog.
4. Descripción de la PR con año, nombre de archivo y link de la consigna incorrectos, y "Closes" solo en el primer issue.
5. Sugerencia: en CU3 la clase `Pago` no participa del diagrama de secuencia de cancelación.

**Ajustes al output de la IA:** el hallazgo 5 se trasladó como sugerencia no bloqueante; el autor lo resolvió justificando la clase en el anexo. En la segunda ronda se verificó cada punto contra la rama y solo quedaron pendientes dos detalles de la descripción.

**Resultado:** Request changes (2 rondas) → Aprobada.

### Review 3 - PR #256

**Rama:** `feature/arquitecto-dominio-add-diagrama-final`
**Archivos de contexto referenciados:** diagrama de clases final (.puml/.png), diagramas parciales de CU1 a CU5, `pilares-poo.md`, `happy-path-global.md`, tarjetas CRC, diagramas de secuencia y `changelog.md`.

**Prompt utilizado:** prompt base de este documento, adaptado al diagrama final unificado.

**Hallazgos de la IA:**
1. `changelog.md` tenía un conflicto sin resolver en la sección de A2 (marcadores `<<<<<<<` / `=======` / `>>>>>>>` y bloque duplicado).
2. Las ramas `fix/` de las issues #247, #248 y #249 no existían y los diagramas parciales seguían sin unificar (`SistemaPedidos` en CU1, `InterfazPedido` en CU2).
3. `happy-path-global.md` mencionaba tres tipos de usuario cuando participan dos.
4. Sugerencia: multiplicidad Pedido–Pago distinta entre el diagrama final (1..1) y CU3 (0..*).

**Ajustes al output de la IA:** el hallazgo 4 se marcó como sugerencia; el autor lo justificó en el anexo. Después de mergear la PR #257 se detectó una nota del diagrama final que seguía nombrando `SistemaPedidos` y el autor la eliminó antes de la aprobación.

**Resultado:** Request changes → Aprobada.

### Review 4 - PR #257

**Rama:** `fix/unificar-nombre-servicio-pedidos`
**Archivos de contexto referenciados:** .puml/.png de clases y de secuencia de CU1 y CU2, anexos de CU1 y CU2, diagrama de clases final.

**Prompt utilizado:** prompt base de este documento, cambiando la rama y los archivos.

**Hallazgos de la IA:**
1. El PNG de clases de CU2 se regeneró en 4096 px de ancho, el límite por defecto de PlantUML, y quedó recortado: la clase `HistorialPago` aparecía cortada a la derecha.
2. El título de la PR solo menciona CU1, pero también renombra `InterfazPedido` a `PantallaPedidos` en CU2.

**Ajustes al output de la IA:** en lugar de devolver la PR, el coordinador regeneró el PNG con `PLANTUML_LIMIT_SIZE` mayor y lo agregó a la rama. Se verificó que la imagen nueva no estuviera recortada. El hallazgo 2 se consideró no bloqueante porque el changelog ya describe los dos cambios.

**Resultado:** Aprobada.
