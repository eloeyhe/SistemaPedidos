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
  - Los cinco escenarios terminaban en `-01`; se renumeraron del `-01` al `-05` en secuencia por caso de uso, igual que los demás diagramas.
  - El índice de diagramas de clases se renombró a `diagramas_clases.md`, que es el nombre que usan la estructura final del proyecto y las tareas del rol de Coordinador y DevOps.
  - Se tuvieron en cuenta las observaciones del docente de la Actividad Obligatoria N°4: títulos literales de las PRs en el changelog, nombres de clase en PascalCase, tarjetas CRC para todas las clases usadas, ningún archivo fuera de la estructura pedida, issues cerradas y solo las ramas pedidas al momento de la entrega.
- **Índices de navegación:** se creó `anexos/patrones-diseno/patrones_diseno.md` y se actualizaron `diagramas/01-diagrama-clases/diagramas_clases.md`, `diagramas/diagramasUML.md`, `anexos/anexos.md` y `README.md` con los tres patrones de diseño.

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
HALLAZGO #<número>
Archivo:
Línea:
Problema:
Sugerencia:
```

---

## 3. Code Reviews realizadas

| # | PR | Rama | Autor | Resultado |
|---|----|------|-------|-----------|
| 1 | «PR_1» | «RAMA_1» | «AUTOR_1» | «RESULTADO_1» |
| 2 | «PR_2» | «RAMA_2» | «AUTOR_2» | «RESULTADO_2» |
| 3 | «PR_3» | «RAMA_3» | «AUTOR_3» | «RESULTADO_3» |
| 4 | «PR_4» | «RAMA_4» | «AUTOR_4» | «RESULTADO_4» |
