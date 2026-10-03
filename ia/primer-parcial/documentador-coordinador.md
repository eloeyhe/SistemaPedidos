# Registro de Code Reviews asistidas con IA - Documentador y Coordinador

## Code Review 1: PR [#133] - [ESPECIALISTA EN PRINCIPIOS DE EXTENSIÓN (OCP + LSP)]
* **Rama revisada:** `future/esp-extension-ocp-add-anexo-ocp`
* **Autor:** Eloy Eyheramendy

 ### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

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


---

## Code Review 2: PR [#141] - [ESPECIALISTA EN PRINCIPIOS DE EXTENSIÓN (OCP + LSP)]
* **Rama revisada:** `feature/esp-extension-lsp-add-anexo-lsp`
* **Autor:** Eloy Eyheramendy

 ### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

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


---

## Code Review 3: PR [#150] - [ESPECIALISTA EN PRINCIPIOS DE EXTENSIÓN (OCP + LSP)]
* **Rama revisada:** `feature/esp-dip-add-anexo-dip`
* **Autor:** Eloy Eyheramendy

 ### 1. Prompt utilizado

```text
Actúa como un Senior Software Engineer realizando code review profesional.

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


---


