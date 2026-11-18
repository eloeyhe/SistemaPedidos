# Informe de Interacción con IA - Analista Funcional del Caso de Uso 1

> **Nota de vigencia de nombres (RC10):** a partir de las correcciones de la release de A4 (PRs #266 y #269), las clases del modelo se nombran en PascalCase: `ItemPedido`, `PersonalizacionItem`, `EstadoPedido`, `MetodoPago`, `TipoMovimiento`, `HistorialPago`, `HistorialCambioPedido` y `AuditoriaPedido`. El texto de este registro usa esos nombres. Los prompts y los outputs de la IA que aparecen en bloques de código se conservan literales, porque documentan lo que se pidió y lo que devolvió la IA en ese momento: las variantes con guion bajo que figuran ahí (por ejemplo `Item_pedido` o `Estado_pedido`) son **contexto histórico no vigente**.

**Rol:** Coordinador de Repositorio + Analista Funcional del Caso de Uso 1

**Autor:** Santiago Medel Marquez

**Herramienta utilizada:** Claude (modelo Claude Opus 5.5) en modo agente, con acceso al repositorio clonado

> **Aclaración:** la consigna indica GitHub Copilot Agent Mode. Para este rol se utilizó Claude en modo agente, que cumple la misma función: lee los archivos del repositorio como contexto, genera el diagrama PlantUML, lo exporta a PNG y redacta el anexo. Se documenta la herramienta real utilizada, igual que en `ia/a3/esp-actividades-1-2.md`.

---

## 1. Archivos de contexto referenciados

- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`: clases, atributos, métodos y multiplicidades del boceto de A1 (Pedido, ItemPedido, Producto abstracto, PersonalizacionItem, Pago, Combo, enumerados `EstadoPedido` y `MetodoPago`, jerarquía de Usuario).
- `diagramas/02-casos-de-uso/02-caso-uso-tomar-pedido-01.puml`: actores y relaciones `<<include>>`/`<<extend>>` del CU1.
- `diagramas/03-escenarios-casos-de-uso/03-tomar-pedido-registro-pedido-01.md`: pasos del flujo principal, requisitos asociados (RF1, RF2, RF3, RF6, RF8) y aspectos sobresalientes.
- `diagramas/04-diagramas-actividades/04-actividad-tomar-pedido-01.puml`: swimlanes, decisiones y flujos alternativos.
- `diagramas/05-diagramas-secuencia/05-secuencia-tomar-pedido-registro-de-pedido-01.puml`: participantes, nombres exactos de los mensajes y objetos destruidos.
- `herramientas-agile/tarjetas-crc/01` a `05`, `07`, `08`, `09`, `13` y `14`: responsabilidades, colaboradores y propiedades de las clases involucradas.
- `anexos/introduccion.md`: texto literal de los requisitos funcionales.
- `ACTIVIDAD_OBLIGATORIA_N_4.pdf` y la plantilla `00-plantilla-anexo-caso-de-uso.md` de la consigna.

---

## 2. Prompt utilizado

Se adjuntó la consigna en PDF y se envió el siguiente pedido:

```text
ahora yo soy documentador y coordinador podes hacer mi parte desde vs subiendo todo a git el link de git es este https://github.com/eloeyhe/SistemaPedidos
```

Durante la generación se agregó esta indicación:

```text
no subas nada dejame chequearlo primero
```

Con ese pedido, la IA leyó los archivos de contexto de la sección 1 y tomó de la consigna los requisitos mínimos del diagrama de clases (atributos tipados, métodos con parámetros y retorno, las cinco relaciones UML, coherencia con CRC y secuencia) y la estructura obligatoria de la plantilla del anexo.

---

## 3. Output obtenido

- `diagramas/01-diagrama-clases/01-clases-tomar-pedido-01.puml` y su exportación `.png`.
- `anexos/analisis-funcional/01-caso-de-uso-tomar-pedido.md` con las seis secciones de la plantilla.
- Entradas en los índices `diagramas_de_clases.md` y `analisis_casos_uso.md`.

---

## 4. Ajustes realizados al output de la IA

1. **Nombres de métodos alineados al diagrama de secuencia.** Los métodos genéricos del boceto (`agregar()`, `registrarPago()`) se reemplazaron por los mensajes reales de A3: `asignarIdentificador(numeroPedido, fechaHora)`, `agregarItem(producto, cantidad)`, `agregarItemCombo(combo, cantidad)`, `conservarPrecioHistorico(precioBase)`, `registrarCobro(medioPago, totalPedido)`, etc.
2. **Se conservaron los nombres de las tarjetas CRC (decisión no vigente).** En la primera versión se mantuvieron los nombres con guion bajo (`Item_pedido`, `Personalizacion_item`, `Estado_pedido` y `Metodo_pago`), tal como figuraban entonces en las tarjetas, para no romper la trazabilidad. Esta decisión fue reemplazada en la corrección de la release: hoy las clases y sus tarjetas usan PascalCase (`ItemPedido`, `PersonalizacionItem`, `EstadoPedido` y `MetodoPago`), ver la iteración de corrección en la sección 5.
3. **Clases sin tarjeta CRC.** `SistemaPedidos`, `ProcesadorPago` y `PantallaCocina` aparecen como participantes en la secuencia pero no tienen tarjeta CRC. Se incluyeron con estereotipos `<<control>>`, `<<service>>` y `<<boundary>>` y se dejó constancia en el anexo para que el Arquitecto de Dominio decida cómo unificarlas en el diagrama final.
4. **Cliente fuera del diagrama.** El boceto inicial tenía una clase `Cliente`, pero RF6 descarta la ficha de cliente. Se dejó al Cliente sólo como actor, igual que en el diagrama de casos de uso.
5. **Relaciones revisadas.** Se usó composición para Pedido-ItemPedido e ItemPedido-PersonalizacionItem, porque la secuencia los destruye juntos ante el rechazo del pago, y agregación para SistemaPedidos-Pedido, porque los pedidos persisten por auditoría (RF5).
6. **Trazabilidad RNF5.** Se agregó el atributo `creadoPor : UsuarioMostrador` en `Pedido` para registrar quién tomó el pedido.

---

## 5. Iteraciones relevantes

- Se renderizó el `.puml` con PlantUML y se revisó la imagen para verificar que las multiplicidades y estereotipos fueran legibles.
- Se verificó que todas las rutas de imágenes y tarjetas CRC referenciadas en el anexo existan en el repositorio.
- **Iteración de corrección (revisión docente de la release, RC6, RC10, RC11, RC12, RC13 y RC19):**
  - Se normalizaron los nombres de clase a PascalCase en el CU1 y en las tarjetas CRC de esas clases: `ItemPedido`, `PersonalizacionItem`, `EstadoPedido` y `MetodoPago`. Esto reemplaza la decisión del ajuste 2, que había conservado el guion bajo.
  - Se crearon las tarjetas CRC de `ServicioPedidos`, `ProcesadorPago` y `PantallaCocina` (17, 18 y 19), se agregaron al índice y se enlazaron desde el anexo. Esto resuelve el ajuste 3.
  - Se alinearon los contratos entre el diagrama de clases, la secuencia y el pseudocódigo:
    - `registrarPedido(...)` queda sólo en `Pedido`. Se quitó `registrarPedido()` de `UsuarioMostrador`, que opera a través de `ServicioPedidos`.
    - `obtenerTotal()` ahora aparece como mensaje en la secuencia.
    - `descartarPedidoEnCurso()` pasa a ser público en `ServicioPedidos` y se invoca en la secuencia ante el rechazo del pago, igual que en el pseudocódigo.
    - `asociarPago(pago)` y `conservarPrecioHistorico(precioBase)` ya coincidían y se deja explícito que pertenecen a `Pedido` y a `ItemPedido`.
