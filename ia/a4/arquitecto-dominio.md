# Documentación de Uso de IA - Arquitecto de Dominio

Este documento registra el uso de IA en la Actividad Obligatoria N°4 para el rol de **Arquitecto de Dominio**: la generación del diagrama de clases final unificado con **GitHub Copilot Agent Mode**.

---

## 1. Diagrama de Clases Final Unificado

### 1.1 Archivos de contexto referenciados

Para generar `diagramas/01-diagrama-clases/06-clases-diagrama-final.puml`, Copilot Agent Mode trabajó con los artefactos de A1 a A3 que el rol requiere como contexto completo:

- **Boceto inicial de clases (A1):** `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- **Tarjetas CRC (A2):** todas las de `herramientas-agile/tarjetas-crc/`
- **Diagramas de secuencia (A3):** todos los de `diagramas/05-diagramas-secuencia/`
- **Diagramas de clases por caso de uso:** todos los disponibles en `diagramas/01-diagrama-clases/` (CU1 a CU5)

### 1.2 Prompt utilizado

```text
Necesito crear un .puml de un diagrama de clases final tomando como referencia:
diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
Todas las tarjetas CRC de herramientas-agile/tarjetas-crc/
Todos los diagramas de secuencia de diagramas/05-diagramas-secuencia/
Todos los diagramas de clases por caso de uso disponibles en diagramas/01-diagrama-clases/
```

### 1.3 Resultado de la IA

Copilot Agent Mode respondió que creó `06-clases-diagrama-final.puml`, consolidando clases, relaciones y operaciones de los cinco casos de uso y sus secuencias. Informó además que:

- Incluyó las clases de las tarjetas CRC.
- Aclaró en el diagrama que `Cliente` es un actor externo y no una clase persistente, de acuerdo con el RF6.
- La revisión de errores no reportó problemas, pero no pudo comprobar el renderizado porque PlantUML no estaba disponible en su entorno.

### 1.4 Revisión y ajustes realizados

- **Contenido generado por la IA:** el .puml que devolvió Copilot se usó como base sin reescribirlo. La única edición manual sobre el diagrama final fue en `Historial_cambio_pedido`: cambié el retorno de `registrarCambioEstado` de `void` a `boolean`, para que coincida con el diagrama de clases y la secuencia del CU4 (que devuelve `acuseRegistro`).
- **Comparación contra los diagramas parciales:** como Copilot no pudo renderizar el diagrama, lo abrí en la vista previa de PlantUML de VS Code y comparé cada clase, atributo y método contra los diagramas de clases por caso de uso, las tarjetas CRC y las secuencias de A3. Esa comparación detectó tres inconsistencias entre los parciales, que gestioné con issues y ramas `fix/`:
  - [#247](https://github.com/eloeyhe/SistemaPedidos/issues/247): el CU1 usaba `SistemaPedidos` y el CU2 usaba `InterfazPedido`, mientras que el resto de los casos de uso y el diagrama final usan `ServicioPedidos` y `PantallaPedidos`. Se corrigió en la PR [#257](https://github.com/eloeyhe/SistemaPedidos/pull/257).
  - [#248](https://github.com/eloeyhe/SistemaPedidos/issues/248): `registrarCambioPedido` (CU2) y `registrarCambioEstado` (CU4) recibían el pedido de forma distinta, y el diagrama final usa `pedido: Pedido`. Se corrigió en la PR [#258](https://github.com/eloeyhe/SistemaPedidos/pull/258).
  - [#249](https://github.com/eloeyhe/SistemaPedidos/issues/249): existe la tarjeta CRC de `Cliente`, pero el CU1 y el diagrama final no la modelan como clase (RF6). Se dejó documentado en el CU1 en la PR [#259](https://github.com/eloeyhe/SistemaPedidos/pull/259), que se integró en `develop` mediante la PR [#260](https://github.com/eloeyhe/SistemaPedidos/pull/260).
- **Ajuste de exportación (solo visual):** el PNG salía recortado a la derecha por el límite de 4096 píxeles del exportador, aunque la vista previa se veía completa. Lo resolví con parámetros de renderizado (resolución y espaciado) sin modificar clases, atributos, métodos ni relaciones.