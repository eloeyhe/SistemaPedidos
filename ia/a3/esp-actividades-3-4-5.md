# Informe de Interacción con IA - Especialista en Diagramas de Actividades

**Rol:** Especialista en Diagramas de Actividades - Casos de Uso 3, 4 y 5

**Herramientas utilizadas:** GitHub Copilot Agent Mode (VS Code) 

---

## 1. Archivos de Contexto Referenciados

### CU03 - Cancelar Pedido

- `diagramas/02-casos-de-uso/02-caso-uso-cancelar-pedido-03.puml`
- `diagramas/03-escenarios-casos-de-uso/03-cancelar-pedido-cancelacion-antes-entrega-01.md`

### CU04 - Cambiar Estado de Pedido

- `diagramas/02-casos-de-uso/02-caso-uso-cambiar-estado-pedido-04.puml`
- `diagramas/03-escenarios-casos-de-uso/03-cambiar-estado-pedido-avance-preparacion-01.md`

### CU05 - Consultar Pedidos Activos

- `diagramas/02-casos-de-uso/02-caso-uso-consultar-pedidos-activos-05.puml`
- `diagramas/03-escenarios-casos-de-uso/03-consultar-pedidos-activos-prioridad-urgentes-01.md`


---

## 2. Prompts Utilizados

### Prompt 1 - Generación inicial de diagramas

```text
Actúa como un experto en Diseño Orientado a Objetos. Necesito que generes un Diagrama de Actividades para el Caso de Uso N° [3 / 4 / 5]: "[Nombre del Caso de Uso, ej: Cancelar Pedido]".

Basándote estrictamente en los archivos de contexto adjuntos:
- Diagrama de CU: @diagramas/02-casos-de-uso/0X-caso-uso-[nombre].puml
- Escenario: @diagramas/03-escenarios-casos-de-uso/03-[nombre]-flujo-principal-01.md

Requisitos técnicos y de diseño obligatorios:

1. Sintaxis: Utilizar la sintaxis de PlantUML (iniciando con @startuml y terminando con @enduml, utilizando 'start' y 'stop').

2. Carriles (Swimlanes): Incluir un mínimo de 3 carriles para dividir las responsabilidades (por ejemplo: |Cliente/Usuario|, |Sistema|, |Cocina/Encargado|).

3. Actividades: Representar al menos 10 actividades descriptivas ordenadas cronológicamente según el flujo principal y alternativo del escenario.

4. Control de flujo:
- Utilizar rombos de decisión con bloques 'if (...) then (...) else (...) endif' para caminos alternativos o validaciones.
- Utilizar barras de sincronización 'fork' y 'fork again' para procesos que ocurran en paralelo si el caso de uso lo requiere.

5. Nombres consistentes: Asegurar que los nombres de actores, acciones y componentes coincidan exactamente con lo documentado en los archivos de contexto.

Por favor, devolve únicamente el código PlantUML dentro de un bloque de código. 
```



---

## 3. Output Obtenido

### CU03 - Cancelar Pedido
```text
@startuml
title CU03 - Cancelar Pedido - Cancelacion antes de la entrega
skinparam dpi 150
skinparam shadowing false

|Usuario de Mostrador / Encargado|
start
:Seleccionar pedido a cancelar;
:Ingresar numero o referencia del pedido;

|Sistema de Pedidos|
:Buscar pedido;
:Mostrar estado actual del pedido;
:Verificar estado y permisos del actor;

if (¿RECIBIDO y actor autorizado?) then (Si)
  :Habilitar cancelacion directa;
else (No)
  if (¿ EN_PREPARACION?) then (Si)
    :Bloquear cancelacion directa;
    :Enviar solicitud de autorizacion;
    |Encargado|
    :Revisar solicitud de cancelacion;
    if (¿Autoriza?) then (Si)
      :Autorizar cancelacion;
      |Sistema de Pedidos|
      :Registrar Encargado autorizante;
    else (No)
      :Denegar autorizacion;
      |Sistema de Pedidos|
      :Mantener estado original del pedido;
      :Informar rechazo de cancelacion;
      stop
    endif
  else (No)
    :Bloquear cancelacion automatica;
    :Informar que el estado no permite cancelar;
    stop
  endif
endif

|Sistema de Pedidos|
:Solicitar confirmacion de cancelacion;
|Usuario de Mostrador / Encargado|
if (¿Confirma la cancelacion?) then (Si)
  |Sistema de Pedidos|
  :Cambiar estado del pedido a CANCELADO;
  :Conservar identificador, items y pagos;
  :Registrar usuario responsable;
  :Registrar fecha y hora en auditoria;
  :Mostrar cancelacion confirmada;
else (No)
  |Sistema de Pedidos|
  :Mantener estado original del pedido;
  :Informar cancelacion desistida;
endif
stop
@enduml
```

### CU04 - Cambiar Estado de Pedido
```text
@startuml 04-actividad-cambiar-estado-pedido-04
title CU04 - Cambiar Estado de Pedido
!option handwritten false
skinparam dpi 150

|Cocina|
start
:1. Seleccionar pedido existente;

|Sistema de Pedidos|
:2. Consultar pedido seleccionado;
:3. Mostrar estado actual;
:4. Identificar transicion solicitada;
:5. Verificar permisos del actor;

if (¿Actor autorizado?) then (Si)
  :6. Validar transicion de estado;
  if (¿Transicion permitida?) then (Si)
    if (¿Estado actual RECIBIDO?) then (Si)
      |Cocina|
      :7. Iniciar preparacion del pedido;
      |Sistema de Pedidos|
      :8. Actualizar a EN_PREPARACION;
    else (No)
      if (¿Estado actual EN_PREPARACION?) then (Si)
        |Cocina|
        :9. Finalizar preparacion del pedido;
        |Sistema de Pedidos|
        :10. Actualizar a LISTO;
      else (LISTO)
        |Usuario de Mostrador|
        :11. Registrar entrega del pedido;
        |Sistema de Pedidos|
        :12. Actualizar a ENTREGADO;
      endif
    endif
    note right
      Cada pedido conserva un unico
      estado actual valido.
    end note
  else (No)
    :Rechazar cambio sin modificar estado;
  endif
else (No)
  :Rechazar solicitud sin modificar estado;
endif
stop
@enduml
```
### CU05 - Consultar Pedidos Activos
```text
@startuml
title CU05 - Consultar Pedidos Activos (10 actividades)

|Cocina|
start
:1. Acceder al monitor de pedidos activos;

|Sistema de Pedidos|
:2. Comprobar disponibilidad de datos;
if (¿Hay conexión?) then (Sí)
  :3. Obtener pedidos activos;
  :4. Filtrar estados RECIBIDO,
EN_PREPARACION y LISTO;
  :5. Agrupar pedidos por estado
sin duplicarlos;
  if (¿Hay pedidos activos?) then (Sí)
    :6. Identificar pedidos urgentes;
    :7. Ordenar urgentes primero
dentro de cada estado;
    :8. Adaptar la vista según el actor;
    if (¿Consulta Cocina?) then (Sí)
      |Cocina|
      :9. Consultar comandas
RECIBIDO y EN_PREPARACION;
    else (No: Mostrador)
      |Usuario de Mostrador|
      :10. Consultar pedidos LISTO
para entrega;
    endif
  else (No)
    :Mostrar vista vacía;
  endif
else (No)
  :Advertir posible información desactualizada;
endif
stop
@enduml

```

---

## 4. Ajustes Realizados

### CU03 - Cancelar Pedido

- En la primera versión, el Usuario de Mostrador y el Encargado compartían un mismo carril:`|Usuario de Mostrador / Encargado|`
, por lo que separaron sus responsabilidades:
`|Usuario de Mostrador|` y `|Encargado|`.
- Se reemplazó `¿RECIBIDO y actor autorizado?` por `¿Pedido en RECIBIDO y actor autorizado?`.
- Se reemplazó `¿EN_PREPARACION?` por `¿Pedido en EN_PREPARACION?`.
- Se reemplazó `¿Autoriza?` por `¿Autoriza la cancelacion?`.




### CU04 - Cambiar Estado de Pedido

- Se detectó un inconveniente al intentar visualizar la vista previa debido a `!option handwritten false` por lo que se elimino del archivo.

### CU05 - Consultar Pedidos Activos

- No se realizaron ajustes al archivo

