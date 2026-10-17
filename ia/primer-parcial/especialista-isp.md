# Informe de Interacción con IA - Especialista en ISP

**Rol:** Especialista en el Principio de Segregación de Interfaces (ISP)

**Herramienta utilizada:** GitHub Copilot Agent Mode (VS Code)

## 1. Archivos de contexto referenciados

- `anexos/introduccion.md`
- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- Tarjetas CRC de `herramientas-agile/tarjetas-crc/`

## 2. Prompt utilizado

```text
Leer anexos/introduccion.md, diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw y las tarjetas CRC de herramientas-agile/tarjetas-crc/ e identificar responsabilidades en las clases actuales que podrían abstraerse en interfaces cohesivas, detectando posibles "interfaces gordas" que obligarían a implementar métodos no utilizados.
```

## 3. Output obtenido

### Análisis de responsabilidades e interfaces

El boceto no define interfaces explícitas, por lo que no hay una interfaz gorda existente que pueda señalarse como incumplimiento. Sin embargo, varias responsabilidades podrían reunirse en contratos demasiado amplios que hagan depender a las clases de operaciones que no utilizan.

| Área | Contrato cohesivo posible | Responsabilidades relacionadas | Límite recomendado |
| --- | --- | --- | --- |
| Catálogo | `Vendible` o `ItemDeCatalogo` | Exponer nombre y precio vigente para la venta. Podría ser común a `Producto` y `Combo`, cuyo precio propio no se obtiene necesariamente sumando sus componentes. | No incluir preparación, caducidad ni administración del catálogo. |
| Preparación | `Preparables` | Iniciar o informar la preparación de productos preparados. | No exigir esta capacidad a productos envasados ni a combos. |
| Operaciones de pedido | Contratos separados para modificar ítems, cambiar estado, priorizar y cancelar | Representan capacidades distintas del `Pedido` y reglas de negocio diferentes. | Evitar un contrato único de gestión de pedidos si cada consumidor solo necesita algunas operaciones. |
| Operaciones por actor | Contratos de caso de uso separados para mostrador, cocina y encargado | Mostrador registra, modifica y entrega; cocina inicia y finaliza preparaciones; encargado interviene en cancelaciones excepcionales y devoluciones. | No obligar a cada rol a implementar o depender de todas las operaciones. |
| Pago | Contrato de procesamiento del medio de pago, si los medios requieren comportamientos diferentes | Validar o procesar un cobro según el medio seleccionado. | Separar el procesamiento del medio de pago del registro del pago y del cálculo de diferencias. Limitar los medios a efectivo y transferencia para el alcance actual. |
| Trazabilidad | Contratos independientes para auditoría, cambios de pedido y movimientos de dinero | `Auditoria_pedido`, `Historial_cambio_pedido` e `Historial_pago` registran hechos diferentes. | No reunir los tres registros en un contrato genérico que obligue a implementar operaciones no aplicables. |

### Posibles interfaces gordas

- **`Producto`:** sería demasiado amplio si un mismo contrato exigiera proporcionar nombre y precio, iniciar preparación, consultar vencimiento y cambiar precios del catálogo. `ProductoPreparado` y `ProductoEnvasado` no comparten todas esas capacidades.
- **Operaciones de usuario:** una interfaz única con registrar, modificar, entregar, preparar, marcar listo, cancelar y devolver mezclaría las responsabilidades de mostrador, cocina y encargado. Las tarjetas CRC distribuyen esas operaciones entre actores distintos.
- **`Pago`:** puede volverse demasiado amplio si combina registrar el cobro, calcular diferencias y mantener el historial. El boceto distingue `Pago` de `Historial_pago`; conviene conservar esa separación.
- **Trazabilidad:** una interfaz única que exija auditar acciones generales, registrar cambios de campos y registrar movimientos financieros incluiría métodos que no corresponden a todos los consumidores.

Un nombre de método compartido —por ejemplo, `registrar()`— no es suficiente para justificar una interfaz común. Debe existir una necesidad real de consumir esas implementaciones mediante el mismo contrato.

### Observaciones del modelo

1. **`Cliente` como clase de dominio:** los requisitos lo describen como actor externo e indirecto y aclaran que no se necesita una ficha formal. Además, crear, modificar y cancelar pedidos son operaciones iniciadas por el personal. Conviene revisar si `Cliente` debe permanecer como clase o figurar únicamente como actor de los casos de uso.
2. **Medios de pago:** el ejemplo de polimorfismo de la introducción menciona pago digital/QR, mientras que RF8 limita esta fase a efectivo o transferencia. Si se modelan medios mediante interfaces, sus contratos deben corresponder al alcance actual, sin exigir capacidades de medios excluidos del MVP.
3. **No abstraer por anticipado:** las CRC no muestran por sí solas la necesidad de múltiples implementaciones para `Personalizacion_item` o `Item_pedido`. No hace falta crear una interfaz para cada clase; las abstracciones deben responder a consumidores y variantes reales.

### Justificación

Segregar contratos por capacidad evita que actores, servicios o clases dependan de operaciones ajenas a sus necesidades. Las separaciones propuestas mantienen alineadas las interfaces con las responsabilidades descritas en los requisitos y las tarjetas CRC, sin anticipar abstracciones que el diseño actual no justifica.

**Prioridad sugerida:** separar las capacidades de los actores y las operaciones de pedido; evitar contratos amplios para `Producto`, `Pago` y trazabilidad. Estas propuestas son candidatos de diseño, no interfaces ya presentes en el boceto.

## 4. Ajustes realizados

- Se guardó este análisis en formato Markdown en `ia/primer-parcial/especialista-isp.md`.
- No se modificaron el boceto de clases, las tarjetas CRC ni el código del sistema.
