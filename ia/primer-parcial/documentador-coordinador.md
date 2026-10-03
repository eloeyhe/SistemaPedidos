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
