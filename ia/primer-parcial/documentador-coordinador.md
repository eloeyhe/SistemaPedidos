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
Cambio concreto recomendado.
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

--- 

### 4. Ajustes críticos realizados

- **Sugerencias aceptadas:** 
- Ninguna.
- **Sugerencias descartadas o modificadas:** 
- Conservar en `Pedido` el identificador numérico único.
 * No se acepto la sugerencia debido a que no se trata de un Diagrama entidad-relación, por lo tanto las clases no deben incorporar identificadores únicos.

 
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
```

### 2. Archivos de contexto referenciados

- `anexos/introduccion.md`

### 3. Output obtenido de Copilot

HALLAZGO #1

Archivo: `documentador-coordinador.md`
Línea: 523-525

Tipo de problema:
diseño

Severidad:
media

Explicación técnica:
El registro descarta conservar un identificador único en `Pedido` con el argumento de que el modelo no es un diagrama entidad-relación. Sin embargo, RF6 de `anexos/introduccion.md` requiere que el sistema genere un identificador numérico único para cada pedido. La decisión documentada contradice ese requisito y deja sin resolver cómo se identifica cada pedido.

Sugerencia de mejora:
Revisar la sugerencia descartada y conservar en el diseño de `Pedido` la identidad numérica única exigida por RF6; no es necesario que sea una clave de base de datos.


### 4. Ajustes críticos realizados

**Sugerencias aceptadas:**
- Ninguna.

**Sugerencias descartadas o modificadas:**
- Se descarta la sugerencia de incorporar `idPedido` en el diagrama de clases, debido a que las clases no deben incorporar identificadores únicos como en Diagrama entidad-relación.





