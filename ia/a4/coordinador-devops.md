# Registro de Code Reviews asistidas con IA - Coordinador de Repositorio

**Rol:** Coordinador de Repositorio + Analista Funcional del Caso de Uso 1

**Autor:** Santiago Medel Marquez

**Herramienta utilizada:** Claude (modelo Claude Opus 5.5) en modo agente

> **Aclaración:** la consigna indica GitHub Copilot Agent Mode. Se utilizó Claude en modo agente, que lee los archivos de la PR y del repositorio como contexto. Se documenta la herramienta real utilizada.

---

## Tareas de coordinación

- **Correcciones de A3:** todos los Request Changes del docente sobre `release/actividad-obligatoria-3` se resolvieron con ramas `fix/` (PRs #211, #212, #214, #217, #219, #221 y #223) y la release se mergeó a `master` (PR #209).
- **Backport:** se creó `backport/release-actividad-obligatoria-3` desde `master` actualizada para llevar las correcciones aprobadas a `develop` y establecer la base de la Actividad Obligatoria N°4 (PR #225, aprobada por otro integrante).

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

Las reviews se registran a medida que los integrantes abren sus PRs hacia `develop` (mínimo 4).

| # | PR | Rama | Autor | Resultado |
|---|----|------|-------|-----------|
| 1 | | `feature/analista-cu-2-3-add-anexo-cu2-cu3` | Analista Funcional CU2 y CU3 | |
| 2 | | `feature/analista-cu-4-5-add-anexo-cu4-cu5` | Analista Funcional CU4 y CU5 | |
| 3 | | `feature/arquitecto-dominio-add-diagrama-final` | Arquitecto de Dominio | |
| 4 | | | | |

<!-- Por cada review, copiar el bloque siguiente con los datos reales y borrar este comentario antes de la release.

### Review N - PR #XXX

**Rama:** `feature/...`
**Archivos de contexto referenciados:** anexo, .puml/.png de clases, tarjetas CRC involucradas, diagrama de secuencia del CU.

**Prompt utilizado:** prompt base de este documento, cambiando la rama y los archivos.

**Hallazgos de la IA:**
1. ...

**Ajustes al output de la IA:** qué hallazgos se descartaron por falta de evidencia y cuáles se trasladaron como comentario a la PR.

**Resultado:** Request changes / Aprobada.
-->
