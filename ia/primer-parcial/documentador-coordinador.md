# Registro de Code Reviews asistidas con IA - Documentador y Coordinador

## Objetivo
Coordinar la estructura del proyecto, la documentación y la trazabilidad de cambios.

## Code Review 1: PR [#133] - [ESPECIALISTA EN PRINCIPIOS DE EXTENSIÓN (OCP + LSP)]
* **Rama revisada:** `feature/esp-extension-ocp-add-anexo-ocp`
* **Autor:** Eloy Eyheramendy

 ### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
utiliza como referencia introduccion.md
Estás analizando los cambios de una Pull Request activa.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.



```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`


### 3. Output obtenido de Copilot
HALLAZGO #1

Archivo:
anexos/principios-solid/02-ocp.md
Línea:
22

Tipo de problema:
legibilidad

Severidad:
media

Explicación técnica:
La imagen del diagrama usa una ruta absoluta de Windows local: C:\Users\Asus\Desktop\SistemaPedido\diagramas\01-diagrama-clases\01-solid-02-ocp.png. Esto no es portable ni es una ruta válida dentro del repositorio en GitHub, GitLab o cualquier entorno de revisión ajeno al equipo del autor. Como resultado, la imagen no se renderizará correctamente en la documentación y la explicación queda visualmente rota.

Sugerencia de mejora:
Usar una ruta relativa al documento para que el recurso se resuelva correctamente en cualquier entorno de trabajo. Por ejemplo:
../../diagramas/01-diagrama-clases/01-solid-02-ocp.png




### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:**  
 - Usar una ruta relativa al documento para que el recurso se resuelva correctamente en cualquier entorno de trabajo. Por ejemplo:
../../diagramas/01-diagrama-clases/01-solid-02-ocp.png

* **Sugerencias descartadas o modificadas:** 
- Ninguna.

### 5. Evidencia de revisión en GitHub

- **Pull Request:** [#133](https://github.com/eloeyhe/SistemaPedidos/pull/133)
- **Archivo revisado:** `anexos/principios-solid/02-ocp.md`
- **Línea del hallazgo:** 22
- **Tipo:** Legibilidad
- **Severidad:** Media
- **Decisión inicial:** REQUEST CHANGES
- **Corrección solicitada:** Reemplazar la ruta absoluta por una relativa.
- **Evidencia:** [Comentario técnico realizado en el diff de la PR #133](https://github.com/eloeyhe/SistemaPedidos/pull/133/changes/BASE..167308e871d68a88cb00cbf57929f9d120242456#r4175374092)
- **Resultado:** Se registró una aprobación posterior.
---

## Code Review 2: PR [#141] - [ESPECIALISTA EN PRINCIPIOS DE EXTENSIÓN (OCP + LSP)]
* **Rama revisada:** `feature/esp-extension-lsp-add-anexo-lsp`
* **Autor:** Eloy Eyheramendy

 ### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
utiliza como referencia introduccion.md
Estás analizando los cambios de una Pull Request activa.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.



```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`


### 3. Output obtenido de Copilot
HALLAZGO #1

Archivo:
anexos/principios-solid/03-lsp.md
Línea:
22

Tipo de problema:
legibilidad

Severidad:
media

Explicación técnica:
La imagen del diagrama usa una ruta absoluta de Windows local: `C:\Users\Asus\Desktop\SistemaPedido\diagramas\01-diagrama-clases\01-solid-03-lsp.png`. Esta ruta no es portable ni válida en GitHub ni en otros entornos de revisión, por lo que la imagen no se renderizará correctamente fuera del equipo del autor. El resultado es que la explicación técnica queda visualmente rota y la documentación deja de ser usable para otros colaboradores.

Sugerencia de mejora:
Usar una ruta relativa al documento para que el recurso se resuelva correctamente en cualquier entorno de trabajo. Por ejemplo:
`../../diagramas/01-diagrama-clases/01-solid-03-lsp.png`


### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:**  
 - Usar una ruta relativa al documento para que el recurso se resuelva correctamente en cualquier entorno de trabajo. Por ejemplo:
`../../diagramas/01-diagrama-clases/01-solid-03-lsp.png`

* **Sugerencias descartadas o modificadas:** 
-Ninguna.

### 5. Evidencia de revisión en GitHub

- **Pull Request:** [#141](https://github.com/eloeyhe/SistemaPedidos/pull/141)
- **Archivo revisado:** `anexos/principios-solid/03-lsp.md`
- **Línea del hallazgo registrada por Copilot:** 22
- **Tipo de problema:** Legibilidad
- **Severidad:** Media
- **Hallazgo identificado:** La documentación utilizaba una ruta absoluta de Windows que impedía visualizar correctamente el diagrama LSP fuera del equipo del autor.
- **Corrección solicitada:** Reemplazar la ruta absoluta por una ruta relativa: `../../diagramas/01-diagrama-clases/01-solid-03-lsp.png`.
- **Decisión inicial:** `REQUEST CHANGES`
- **Evidencia:** [Comentario técnico realizado en el diff de la PR #141](https://github.com/eloeyhe/SistemaPedidos/pull/141/files)
- **Resultado:** Posteriormente se registró una revisión con estado `APPROVED` en GitHub.

---

## Code Review 3: PR [#150] - [ESPECIALISTA EN INVERSIÓN DE DEPENDENCIAS (DIP)]
* **Rama revisada:** `feature/esp-dip-add-anexo-dip`
* **Autor:** Eloy Eyheramendy

 ### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
utiliza como referencia introduccion.md

Estás analizando los cambios de una Pull Request activa.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.



```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`


### 3. Output obtenido de Copilot
HALLAZGO #3

Archivo:
anexos/principios-solid/05.dip.md
Línea:
22

Tipo de problema:
legibilidad

Severidad:
media

Explicación técnica:
La imagen del diagrama usa una ruta absoluta de Windows local: C:\Users\Asus\Desktop\SistemaPedido\diagramas\01-diagrama-clases\01-solid-05-dip.png. Ese path no es portable ni válido fuera del entorno del autor, por lo que la documentación romperá la visualización en GitHub o en cualquier otra máquina de revisión. El problema es real porque el recurso no se resolverá en el repositorio y la explicación técnica queda sin el diagrama correspondiente.

Sugerencia de mejora:
Usar una ruta relativa al documento para que la imagen se resuelva en cualquier entorno de trabajo. Por ejemplo:
../../diagramas/01-diagrama-clases/01-solid-05-dip.png





### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:**  
 - Usar una ruta relativa al documento para que la imagen se resuelva en cualquier entorno de trabajo. Por ejemplo:
../../diagramas/01-diagrama-clases/01-solid-05-dip.png


* **Sugerencias descartadas o modificadas:** 
- Ninguna.

### 5. Evidencia de revisión en GitHub

- **Pull Request:** [#150](https://github.com/eloeyhe/SistemaPedidos/pull/150)
- **Archivo revisado:** `anexos/principios-solid/05.dip.md`
- **Línea del hallazgo registrada por Copilot:** 22
- **Tipo de problema:** Legibilidad
- **Severidad:** Media
- **Hallazgo identificado:** La documentación utilizaba una ruta absoluta de Windows que impedía visualizar correctamente el diagrama DIP fuera del equipo del autor.
- **Corrección solicitada:** Reemplazar la ruta absoluta por una ruta relativa: `../../diagramas/01-diagrama-clases/01-solid-05-dip.png`.
- **Decisión inicial:** `REQUEST CHANGES`
- **Evidencia:** [Comentario técnico realizado en el diff de la PR #150](https://github.com/eloeyhe/SistemaPedidos/pull/150/files)
- **Resultado:** Posteriormente se registró una revisión con estado `APPROVED` en GitHub.
---
## Code Review 4: PR [#151] - [ESPECIALISTA EN ISP]
* **Rama revisada:** `feature/esp-isp-add-anexo-isp`
* **Autor:** Leandro Dominguez

 ### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.
utiliza como referencia introduccion.md

Estás analizando los cambios de una Pull Request activa.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

```

### 2. Archivos de contexto referenciados
* `anexos/introduccion.md`


### 3. Output obtenido de Copilot


HALLAZGO #1

Archivo:
`changelog.md`

Línea:
28

Tipo de problema:
otro

Severidad:
media

Explicación técnica:
El changelog contiene marcadores de conflicto de Git (`>>>>>>> origin/develop` en la línea 46) que quedaron versionados. Son residuos de una resolución de conflicto sin finalizar: se muestran como texto en el Markdown y dejan ambiguo el contenido integrado del changelog, incluida la entrada de esta rama.

Sugerencia de mejora:
Resolver el conflicto conservando las entrada válida ISP y eliminar los marcadores de conflicto.

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:**  
- Resolver el conflicto conservando las entrada válida de ISP y eliminar los marcadores de conflicto.

* **Sugerencias descartadas o modificadas:** 
- Ninguna.

### 5. Evidencia de revisión en GitHub

- **Pull Request:** [#151](https://github.com/eloeyhe/SistemaPedidos/pull/151)
- **Archivo revisado:** `changelog.md`
- **Línea del hallazgo registrada por Copilot:** 28
- **Tipo de problema:** Otro (conflicto de integración Git)
- **Severidad:** Media
- **Hallazgo identificado:** Se detectaron marcadores de conflicto de Git, como `>>>>>>> origin/develop`, dentro del archivo `changelog.md`. Estos residuos indicaban que la resolución del conflicto no había finalizado correctamente y afectaban la integridad de la documentación.
- **Corrección solicitada:** Resolver el conflicto conservando las entradas válidas correspondientes a ISP y eliminar los marcadores de conflicto.
- **Decisión inicial:** `REQUEST CHANGES`
- **Evidencia:** [Comentario técnico realizado en el diff de la PR #151](https://github.com/eloeyhe/SistemaPedidos/pull/151/files)
- **Resultado:** Posteriormente se registró una revisión con estado `APPROVED` en GitHub.

---

## Code Review 5: PR [#117] - [Documetnador y Coordinador]

- **Pull Request:** [#117](https://github.com/eloeyhe/SistemaPedidos/pull/117)
- **Rama revisada:** `feature/doc-coord-repo-update-readme-md`
- **Autor:** `nachovelesquen7-rgb`

 ### 1. Prompt utilizado
```text
Actúa como un Senior Software Engineer realizando code review profesional.
utiliza como referencia introduccion.md

Estás analizando los cambios de una Pull Request #117

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:

Decisión sugerida: (APPROVE | REQUEST CHANGES | COMMENT)
```

--- 

### 2. Archivos de contexto

- `anexos/introduccion.md`


### 3. Output obtenido de Copilot

HALLAZGO #1

Archivo: `ia/primer-parcial/documentador-coordinador.md`
Línea: 1 (archivo vacío)

Tipo de problema:
otro

Severidad:
media


Explicación técnica:
El PR agrega el archivo destinado a la documentación del rol Documentador y Coordinador, pero está vacío. Por lo tanto, no aporta documentación del repositorio ni información vinculada con el contexto y los requisitos descritos en `introduccion.md`.

Sugerencia de mejora:
Agregar al archivo la documentación del repositorio correspondiente al rol, incluyendo cómo se organiza y dónde se encuentran sus documentos y diseños.


Decisión sugerida por IA: REQUEST CHANGES, hasta que se complete la documentación.

### 4. Ajustes críticos realizados
* **Sugerencias aceptadas:**  
- Agregar al archivo la documentación del repositorio correspondiente al rol, incluyendo cómo se organiza y dónde se encuentran sus documentos y diseños.
- **Sugerencias descartadas o modificadas:** 
- Ninguna.

--- 


## Code Review 6: PR [#126] - [ESPECIALISTA EN RESPONSABILIDAD ÚNICA (SRP)]

- **Pull Request:** [#126](https://github.com/eloeyhe/SistemaPedidos/pull/126)
- **Rama revisada:** `feature/esp-srp-add-anexo-srp`
- **Autor:** `nachovelesquen7-rgb`

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando una code review profesional.

Utiliza como referencia anexos/introduccion.md y analiza los cambios
originales de la Pull Request #126.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales de los archivos modificados.
- Enumera los hallazgos de forma independiente.
- Indica archivo, línea, tipo de problema y severidad.
- Explica técnicamente cada problema.
- Propone una corrección concreta.
- No inventes problemas hipotéticos.
- No incluyas sugerencias de tests.
- No modifiques ningún archivo.

Para cada hallazgo utiliza esta estructura:

HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:

Sugerencia de mejora:

Decisión sugerida: (APPROVE | REQUEST CHANGES | COMMENT)

```

### 2. Archivos de contexto referenciados

- `anexos/introduccion.md`


### 3. Output obtenido de Copilot

HALLAZGO #1

Archivo: `especialista-srp.md` y `01-solid-01-srp.puml`  
Línea: 59; clase `Pedido` (línea 7)

Tipo de problema:
diseño

Severidad:
media


Explicación técnica:
El informe justifica eliminar `idPedido` porque, según indica, las clases no necesitan un identificador único. Sin embargo, el requisito RF6 de `anexos/introduccion.md` exige generar un identificador numérico único para cada pedido. El diagrama resultante tampoco muestra ese atributo en `Pedido`, así que el diseño deja sin representar cómo se cumple el requisito.

Sugerencia de mejora:
Conservar en `Pedido` el identificador numérico único.

Decisión sugerida por IA: REQUEST CHANGES, hasta aclarar la inconsistencia.

--- 

### 4. Ajustes críticos realizados

- **Sugerencias aceptadas:** 
- Conservar en `Pedido` el identificador numérico único.
- **Sugerencias descartadas o modificadas:** 
- Ninguno.

 
---

## Code Review 7: PR [#152] - [DOCUMENTADOR Y COORDINADOR]

- **Pull Request:** [#152](https://github.com/eloeyhe/SistemaPedidos/pull/152)
- **Rama revisada:** `feature/doc-coord-repo-update-readme-md`
- **Autor:** `nachovelesquen7-rgb`

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando una code review profesional.

Utiliza como referencia anexos/introduccion.md y analiza los cambios
originales de la Pull Request #152.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales de los archivos modificados.
- Enumera los hallazgos de forma independiente.
- Indica archivo, línea, tipo de problema y severidad.
- Explica técnicamente cada problema.
- Propone una corrección concreta.
- No inventes problemas hipotéticos.
- No incluyas sugerencias de tests.
- No modifiques ningún archivo.

Para cada hallazgo utiliza esta estructura:

HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:

Sugerencia de mejora:

Decisión sugerida: (APPROVE | REQUEST CHANGES | COMMENT)

```

### 2. Archivos de contexto referenciados

- `anexos/introduccion.md`

### 3. Output obtenido de Copilot

HALLAZGO #1

Archivo: changelog.md
Líneas: 17 

Tipo de problema: Legibilidad

Severidad: Media

Explicación técnica:

En las entradas de la PR #152, el rol del responsable figura como ocumentador y Coordinador de Repositorio + srp, omitiendo la letra inicial de «Documentador». 

Estas inconsistencias afectan la uniformidad del changelog y dificultan la identificación correcta de los responsables y los cambios registrados.

Sugerencia de mejora:

Corregir el nombre del rol a Documentador y Coordinador de Repositorio + Especialista SRP.

Decisión sugerida: REQUEST CHANGES, hasta corregir las inconsistencias de documentación.

### 4. Ajustes críticos realizados

**Sugerencias aceptadas:**
- Corregir el nombre del rol a Documentador y Coordinador de Repositorio + Especialista SRP.

**Sugerencias descartadas o modificadas:**
- Ninguna.



## Code Review 8: PR [#171] - [ESPECIALISTA EN PRINCIPIOS DE EXTENSIÓN (OCP + LSP)]

- **Pull Request:** [#171](https://github.com/eloeyhe/SistemaPedidos/pull/171)
- **Rama revisada:** `fix/mejoras-documentacion-ocp-lsp`
- **Autor:** `Eloy Eyheramendy`

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

Estás analizando los cambios de una Pull Request activa de la rama fix/mejoras-documentacion-ocp-lsp.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Ejemplo de código corregido (si aplica):
```codigo
ejemplo
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)
Al final agrega:

**RESUMEN GENERAL DE LA PR**
Evaluación global de calidad y riesgos técnicos.

DECISIÓN FINAL SUGERIDA POR IA:

APPROVE / REQUEST CHANGES / COMMENT ONLY
No completes la sección "DECISIÓN DEL REVISOR HUMANO".
Debe quedar vacía para edición manual.
Publica comentarios directamente en la Pull Request en las líneas correspondientes.
No respondas en el chat salvo para el resumen final.

```

### 2. Archivos de contexto referenciados

- `anexos/introduccion.md`

### 3. Output obtenido de Copilot

HALLAZGO #1

Archivo:
anexos/principios-solid/02-ocp.md

Línea:
8-27

Tipo de problema:
diseño

Severidad:
media

Explicación técnica:
La PR presenta la refactorización de `Pago` como una estrategia extensible con `PagoQR`/`PagoTarjeta`, pero el resto del dominio del proyecto establece en RF8 que el MVP solo admite `EFECTIVO` y `TRANSFERENCIA`. Esa contradicción no es solo de estilo: la justificación del OCP se vuelve engañosa porque se presenta una extensión como “solución del diseño” cuando el alcance del sistema no la incluye. La documentación y el diagrama terminan mezclando una evolución futura con la solución actual del dominio.

Sugerencia de mejora:
Mantener el alcance del documento alineado con RF8. Si se quiere mostrar una extensión futura, debe quedar explícitamente etiquetada como “fuera del alcance del MVP” y no como parte de la solución actual del diseño OCP.

Ejemplo de código corregido (si aplica):
```md
> Alcance actual del MVP (RF8): únicamente EFECTIVO y TRANSFERENCIA.
> `PagoQR` y `PagoTarjeta` son extensiones futuras para OCP, no parte del alcance actual.
```
DECISIÓN DEL REVISOR HUMANO:
[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:

==================================================
HALLAZGO #2

Archivo:
anexos/principios-solid/03-lsp.md

Línea:
17-23

Tipo de problema:
diseño

Severidad:
baja

Explicación técnica:
El documento explica que `Cocina` trabaja solo con `IElaborable`, pero el diagrama de clases muestra a `Combo` como implementador de `IElementoMenu` y no como `IElaborable`. La explicación del principio LSP es correcta para productos elaborados, pero la forma en que se presenta la jerarquía deja la frontera conceptual ambigua: `IElementoMenu` representa “precio”, mientras `IElaborable` representa “puede prepararse”. Si el lector no lee con cuidado, puede interpretar que cualquier elemento del menú es válido para la cocina, lo cual no es cierto.

Sugerencia de mejora:
Separar claramente el alcance de cada contrato en la documentación: `IElementoMenu` solo define el precio comercial, mientras que `IElaborable` define el contrato operativo de la cocina. Esto deja explícito que `Combo` puede ser un elemento de compra pero no un elemento de preparación.

Ejemplo de código corregido (si aplica):
```plantuml
interface IElementoMenu {
  + getPrecio(): Double
}

interface IElaborable {
  + iniciarPreparacion(): void
}

class Cocina {
  + prepararItem(item: IElaborable): void
}
```
DECISIÓN DEL REVISOR HUMANO:
[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:

RESUMEN GENERAL DE LA PR
La PR mejora la redacción y estructura de los anexos OCP y LSP, y en general la documentación es clara y bien organizada. Sin embargo, hay dos inconsistencias reales entre la justificación del diseño y el alcance funcional del proyecto: el OCP asume extensiones futuras de pago que RF8 no contempla, y el LSP no deja explícita la separación entre “elemento de menú” y “elemento elaborable”. Esas discrepancias pueden confundir a la persona que lea la documentación y no reflejan con precisión el dominio del sistema.

DECISIÓN FINAL SUGERIDA POR IA:
REQUEST CHANGES

### 4. Ajustes críticos realizados

**Sugerencias aceptadas:**
- Hallazgo 1: Acepté la sugerencia porque el requisito RF8 establece que el sistema admite únicamente pagos en efectivo y transferencia.
- Hallazgo 2: Acepté la sugerencia ya que esto permite comprender que un combo puede venderse como elemento del menú, pero que la preparación corresponde a los productos elaborables que lo componen. aunque es de severidad baja.

**Sugerencias descartadas o modificadas:**
- Ninguna.

## Code Review 9: PR [#173] - [Documentador y Coordinador + Especialista SRP]

- **Pull Request:** [#173](https://github.com/eloeyhe/SistemaPedidos/pull/173)
- **Rama revisada:** `fix/correccion-detalles-srp`
- **Autor:** `Ignacio Velesquen`

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

Estás analizando los cambios de una Pull Request activa de la rama fix/mejoras-documentacion-ocp-lsp.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests
Hay un PR en github que esta activa y quiero analizarla, te dejo el link(https://github.com/eloeyhe/SistemaPedidos/pull/173). Quiero que en los archivos 01-srp.md, 01-solid-01-srp.png, 01-solid-01-srp.puml y especialista-srp.dip busques como dije antes problemas de codigo, nomeclatura, contendio,etc.

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Ejemplo de código corregido (si aplica):
```codigo
ejemplo
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)
Al final agrega:

RESUMEN GENERAL DE LA PR
Evaluación global de calidad y riesgos técnicos.

DECISIÓN FINAL SUGERIDA POR IA:

APPROVE / REQUEST CHANGES / COMMENT ONLY
No completes la sección "DECISIÓN DEL REVISOR HUMANO".
Debe quedar vacía para edición manual.
Publica comentarios directamente en la Pull Request en las líneas correspondientes.
No respondas en el chat salvo para el resumen final.

```

### 2. Archivos de contexto referenciados

- `01-srp.md`
- `01-solid-01-srp.png`
- `01-solid-01-srp.puml`
- `especialista-srp.dip`

### 3. Output obtenido de Copilot

==================================================
HALLAZGO #1

Archivo:
anexos/principios-solid/01-srp.md
Línea: 54

Tipo de problema:
diseño

Severidad:
media

Explicación técnica:
La regla nueva indica que el total del pedido se obtiene sumando los subtotales de sus ítems, pero RF1 exige admitir combos cuyo precio tiene lógica propia y no equivale a la suma de sus partes. El modelo descrito no incluye Combo ni otra representación de ese precio, por lo que el cálculo documentado no puede producir el total requerido para un pedido con combos.

Sugerencia de mejora:
Incluir en el modelo una representación del combo y su cálculo de precio independiente, o documentar cómo se integra ese precio al total del pedido sin tratarlo como una suma ordinaria de subtotales.

Ejemplo de código corregido (si aplica):

class Combo {
  + calcularPrecio(): Decimal
}
DECISIÓN DEL REVISOR HUMANO:

[x] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
El hallazgo es correcto y expone una inconsistencia crítica entre nuestro diseño y las reglas de negocio del MVP. Asumir que el total del pedido es meramente la suma de los subtotales viola el Requisito Funcional 1 (RF1), el cual establece que los combos poseen una lógica de precios independiente. Se actualizará la documentación del SRP y el modelo subyacente para incluir la abstracción correspondiente (por ejemplo, asegurando que tanto Producto como Combo implementen una interfaz común como IElementoMenu con su propio método de cálculo de precio), logrando que la responsabilidad de conocer el precio recaiga en el elemento y no en una sumatoria rígida dentro del Pedido.

==================================================
HALLAZGO #2

Archivo:
anexos/principios-solid/01-srp.md
Línea: 65

Tipo de problema:
diseño

Severidad:
media

Explicación técnica:
El texto asigna al CalculadorAjustePago la tarea de calcular la diferencia y al ServicioRegistroPago registrar el ajuste. Sin embargo, el diagrama mantiene la dependencia en sentido inverso (CalculadorAjustePago ..> ServicioRegistroPago), que representa que el calculador depende del servicio y no que el servicio solicita el cálculo. La relación contradice las responsabilidades que esta sección acaba de describir.

Sugerencia de mejora:
Invertir la dependencia en el PlantUML y regenerar el PNG para mostrar al ServicioRegistroPago dependiendo del CalculadorAjustePago.

Ejemplo de código corregido (si aplica):

ServicioRegistroPago ..> CalculadorAjustePago : solicita cálculo
DECISIÓN DEL REVISOR HUMANO:

[x] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
Se trata de un error conceptual en la representación gráfica del diagrama UML que contradice directamente la justificación redactada. Si el ServicioRegistroPago requiere del cálculo de la diferencia para poder efectuar el registro final, entonces es el Servicio quien depende y solicita información al CalculadorAjustePago, y no a la inversa. Se corregirá la dirección de la flecha de dependencia (..>) en el archivo PlantUML y se regenerará la imagen .png para asegurar la coherencia técnica del documento.

==================================================
HALLAZGO #3

Archivo:
ia/primer-parcial/especialista-srp.md
Línea: 59-60

Tipo de problema:
legibilidad

Severidad:
baja

Explicación técnica:
El texto agregado contiene errores ortográficos y gramaticales que dificultan la lectura del informe: «encneuntra», «desicion», «intrduccion», «remplazo» y la repetición «quien quien». Al ser parte de la explicación de los cambios del diseño, esos errores reducen la claridad del registro.

Sugerencia de mejora:
Corregirlos por «encuentra», «decisión», «introducción» y «reemplazó», eliminar la repetición y ajustar la puntuación y las tildes de las oraciones.

Ejemplo de código corregido (si aplica):
No aplica.

DECISIÓN DEL REVISOR HUMANO:

[x] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
Los criterios formales de evaluación del parcial exigen "pertinencia y claridad en la redacción, estilo académico y correcta construcción gramatical". Los errores tipográficos señalados restan profesionalismo al entregable final. Se procederá a aplicar las correcciones ortográficas e idiomáticas ("encuentra", "decisión", "introducción", "reemplazó") y se eliminarán las redundancias en el archivo Markdown de registro de IA para garantizar su legibilidad.

### 4. Ajustes críticos realizados

**Sugerencias aceptadas:**

Actualizar la documentación del SRP y el modelo subyacente para incluir una representación del Combo y su cálculo de precio independiente, asegurando que tanto Producto como Combo implementen una interfaz común (IElementoMenu) para cumplir con el RF1.

Invertir la dirección de la flecha de dependencia en el archivo PlantUML (ServicioRegistroPago ..> CalculadorAjustePago) y regenerar la imagen PNG para reflejar correctamente las responsabilidades.

Corregir los errores ortográficos e idiomáticos («encuentra», «decisión», «introducción», «reemplazó»), eliminar la repetición («quien quien») y ajustar la puntuación en el archivo especialista-srp.md para garantizar la claridad y el estilo académico exigido.

**Sugerencias descartadas o modificadas:**
- Ninguna.


## Code Review 10: PR [#176] - [Especialista en ISP]

- **Pull Request:** [#176](https://github.com/eloeyhe/SistemaPedidos/pull/176)
- **Rama revisada:** `fix/nuevos-ajustes-en-diagramas-ISP`
- **Autor:** `Leandro Dominguez`

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

Estás analizando los cambios de una Pull Request activa de la rama fix/nuevos-ajustes-en-diagramas-SP.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Ejemplo de código corregido (si aplica):
```codigo
ejemplo
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)
Al final agrega:

RESUMEN GENERAL DE LA PR
Evaluación global de calidad y riesgos técnicos.

DECISIÓN FINAL SUGERIDA POR IA:

APPROVE / REQUEST CHANGES / COMMENT ONLY
No completes la sección "DECISIÓN DEL REVISOR HUMANO".
Debe quedar vacía para edición manual.
Publica comentarios directamente en la Pull Request en las líneas correspondientes.
No respondas en el chat salvo para el resumen final.

```

### 2. Archivos de contexto referenciados

- `anexos/introduccion.md`

### 3. Output obtenido de Copilot

==================================================
HALLAZGO #1

Archivo:
`01-solid-04-isp.puml`

Línea:
67-70

Tipo de problema:
diseño

Severidad:
media

Explicación técnica:
El caso de uso CU03 incluye al Usuario de Mostrador y al Encargado como actores de la cancelación, pero en el diagrama `UsuarioEncargado` solo ofrece `autorizarCancelacion()` y `registrarDevolucionManual()`. `cancelarPedido()` está únicamente en `IAtencionCliente`, implementada por `UsuarioMostrador`. Por lo tanto, el contrato del Encargado no representa la operación de cancelación que CU03 también le asigna.

Sugerencia de mejora:
Extraer `cancelarPedido(idPedido, motivo)` a una interfaz específica de cancelación e implementarla tanto en `UsuarioMostrador` como en `UsuarioEncargado`. Mantener en `ISupervision` solo la autorización y la devolución.

Ejemplo de código corregido (si aplica):
```plantuml
interface ICancelacionPedido {
    + cancelarPedido(idPedido: string, motivo: string): void
}

ICancelacionPedido <|.. UsuarioMostrador
ICancelacionPedido <|.. UsuarioEncargado
```
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)

==================================================
HALLAZGO #2

Archivo:
`01-solid-04-isp.puml`

Línea:
4

Tipo de problema:
legibilidad

Severidad:
baja

Explicación técnica:
El PNG actualizado muestra una advertencia de PlantUML indicando que `skinparam handwritten` está obsoleto. La directiva `skinparam handwritten false` sigue en el fuente, por lo que al regenerar el diagrama la advertencia aparece en la imagen publicada.

Sugerencia de mejora:
Reemplazar la directiva por la sintaxis vigente y regenerar el PNG para que no muestre la advertencia.

Ejemplo de código corregido (si aplica):
```plantuml
!option handwritten false
```
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)

RESUMEN GENERAL DE LA PR
El anexo ISP amplía la explicación de las interfaces y el diagrama incorpora consultas y operaciones que faltaban. Sin embargo, el contrato de cancelación no contempla al Encargado como actor de CU03, y la imagen actualizada muestra una advertencia de sintaxis obsoleta de PlantUML.

DECISIÓN FINAL SUGERIDA POR IA:

REQUEST CHANGES

### 4. Ajustes críticos realizados

**Sugerencias aceptadas:**
- Hallazgo 1: Acepté la sugerencia porque el flujo de cancelación no tiene en cuenta al Encargado como actor de CU03.
- Hallazgo 2: Acepté la sugerencia ya que la imagen actualizada muestra una advertencia de sintaxis obsoleta.

**Sugerencias descartadas o modificadas:**
- Ninguna.

## Code Review 11: PR [#177] - [Especialista en Inversión de Dependencias]

- **Pull Request:** [#177](https://github.com/eloeyhe/SistemaPedidos/pull/177)
- **Rama revisada:** `fix/correcciones-documentacion.dip`
- **Autor:** `Santiago Medel Marquez`

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

Estás analizando los cambios de una Pull Request activa de la rama fix/correcciones-documentacion.dip.

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Ejemplo de código corregido (si aplica):
```codigo
ejemplo
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)
Al final agrega:

RESUMEN GENERAL DE LA PR
Evaluación global de calidad y riesgos técnicos.

DECISIÓN FINAL SUGERIDA POR IA:

APPROVE / REQUEST CHANGES / COMMENT ONLY
No completes la sección "DECISIÓN DEL REVISOR HUMANO".
Debe quedar vacía para edición manual.
Publica comentarios directamente en la Pull Request en las líneas correspondientes.
No respondas en el chat salvo para el resumen final.

```

### 2. Archivos de contexto referenciados

- `anexos/introduccion.md`

### 3. Output obtenido de Copilot

HALLAZGO #1

Archivo:
`05-dip.md`

Línea:
35

Tipo de problema:
diseño

Severidad:
media

Explicación técnica:
La sección afirma que la inyección de dependencias «garantiza» poder validar el 100 % de la lógica de negocio con pruebas unitarias. DIP y las interfaces facilitan aislar dependencias, pero no garantizan que se escriban pruebas ni que alcancen cobertura completa. Además, el diagrama no especifica que `ServicioTomarPedido` implemente toda la lógica mencionada —por ejemplo, rechazar pedidos vacíos—, por lo que la afirmación presenta como garantizado algo que depende de la implementación y de las pruebas que se desarrollen.

Sugerencia de mejora:
Indicar que DIP facilita escribir pruebas unitarias aisladas, quitar el porcentaje y presentar el cálculo del total y la validación de pedidos vacíos como ejemplos de comportamientos que podrían probarse si están implementados en el servicio.

Ejemplo de código corregido (si aplica):
```md
El uso de interfaces facilita la creación de mocks o stubs y permite probar de forma aislada la lógica de `ServicioTomarPedido`. La cobertura alcanzada dependerá de los casos de prueba implementados.
```
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)

RESUMEN GENERAL DE LA PR  
La PR mejora la explicación del DIP y añade una sección de testabilidad. Persiste una afirmación demasiado categórica sobre cobertura de pruebas; conviene matizarla para distinguir la capacidad que aporta la inyección de dependencias de la cobertura que efectivamente se implemente.

DECISIÓN FINAL SUGERIDA POR IA:

REQUEST CHANGES
### 4. Ajustes críticos realizados

**Sugerencias aceptadas:**
- Hallazgo 1: Acepté la sugerencia debido a que si bien la aplicación del principio DIP facilita la realización de pruebas unitarias, no garantiza una cobertura del 100 %. 

**Sugerencias descartadas o modificadas:**
- Ninguna.


## Code Review 12: PR [#180] - [ESPECIALISTA EN PRINCIPIOS DE EXTENSIÓN (OCP + LSP)]

- **Pull Request:** [#180](https://github.com/eloeyhe/SistemaPedidos/pull/180)
- **Rama revisada:** `fix/correccion-de-relaciones-de-diagramas-OCP-LSP`
- **Autor:** `Eloy Eyheramendy`

### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

Estás analizando los cambios de una Pull Request activa de la rama fix/correccion-de-relaciones-de-diagramas-OCP-LSP

INSTRUCCIONES IMPORTANTES:

- Identifica problemas reales del código
- Enumera los hallazgos (1, 2, 3…)
- Cada hallazgo debe ser independiente
- Sé claro, técnico y concreto
- No inventes problemas hipotéticos sin evidencia en el código
- No incluyas sugerencias de tests

Para cada hallazgo usa EXACTAMENTE esta estructura:

==================================================
HALLAZGO #<número>

Archivo:
Línea:

Tipo de problema:
(bug | performance | seguridad | legibilidad | diseño | otro)

Severidad:
(baja | media | alta | crítica)

Explicación técnica:
Por qué esto es un problema real.

Sugerencia de mejora:
Cambio concreto recomendado.

Ejemplo de código corregido (si aplica):
```codigo
ejemplo
DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación del revisor humano:
(Completar manualmente si se rechaza)
Al final agrega:

RESUMEN GENERAL DE LA PR
Evaluación global de calidad y riesgos técnicos.

DECISIÓN FINAL SUGERIDA POR IA:

APPROVE / REQUEST CHANGES / COMMENT ONLY
No completes la sección "DECISIÓN DEL REVISOR HUMANO".
Debe quedar vacía para edición manual.
Publica comentarios directamente en la Pull Request en las líneas correspondientes.
No respondas en el chat salvo para el resumen final.

```

### 2. Archivos de contexto referenciados

- `anexos/introduccion.md`

### 3. Output obtenido de Copilot

HALLAZGO #1

Archivo: `changelog.md`  
Línea: 149

Tipo de problema:  
otro

Severidad:  
baja

Explicación técnica:  
La entrada muestra `PR: #170`, pero el enlace apunta al PR `#180`. Esto hace que el identificador visible contradiga el destino del enlace y puede llevar a consultar el PR equivocado.

Sugerencia de mejora:  
Cambiar el texto `#170` por `#180` para que coincida con el enlace.

Ejemplo de código corregido (si aplica):  
No aplica; corregir el identificador del PR en la entrada.

DECISIÓN DEL REVISOR HUMANO:

[ ] Aceptar sugerencia  
[ ] Rechazar sugerencia

Justificación del revisor humano:


RESUMEN GENERAL DE LA PR  
Los cambios de nomenclatura en los documentos OCP y LSP no muestran problemas funcionales evidentes. Encontré una discrepancia menor en el changelog.

DECISIÓN FINAL SUGERIDA POR IA:

COMMENT ONLY

### 4. Ajustes críticos realizados

**Sugerencias aceptadas:**
- Hallazgo 1: Acepté la sugerencia debido a que La entrada muestra `PR: #170`, pero el enlace apunta al PR `#180`. Esto precenta una contradiccion.

**Sugerencias descartadas o modificadas:**
- Ninguna.


























