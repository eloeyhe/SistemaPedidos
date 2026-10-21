# Informe de Interacción con IA - Especialista en Diagramas de Actividades (CU1 y CU2)

**Rol:** Especialista en Diagramas de Actividades - Casos de Uso 1 y 2

**Autor:** Santiago Medel Marquez

**Herramienta utilizada:** Claude Code (modelo Claude Opus 5.5), extensión de VS Code

> **Aclaración:** la consigna indica GitHub Copilot como herramienta. Para este rol se utilizó Claude Code en modo agente dentro de VS Code, que cumple la misma función: lee los archivos del repositorio como contexto y genera los diagramas PlantUML. Se documenta la herramienta real utilizada.

---

## 1. Archivos de contexto referenciados

- `diagramas/02-casos-de-uso/02-caso-uso-tomar-pedido-01.puml`: actores, límite del sistema y relaciones `<<include>>`/`<<extend>>` del CU1.
- `diagramas/02-casos-de-uso/02-caso-uso-modificar-pedido-02.puml`: actores, límite del sistema y relaciones `<<include>>`/`<<extend>>` del CU2.
- `diagramas/03-escenarios-casos-de-uso/03-tomar-pedido-registro-pedido-01.md`: pasos del flujo principal, precondiciones y aspectos sobresalientes del CU1.
- `diagramas/03-escenarios-casos-de-uso/03-modificar-pedido-modificacion-recibido-01.md`: pasos del flujo principal, precondiciones y aspectos sobresalientes del CU2.
- `ACTIVIDAD_OBLIGATORIA_N_3.pdf`: consigna de la actividad, adjuntada al chat (requisitos mínimos, nomenclatura de archivos y formato del índice).

---

## 2. Prompt utilizado

Se adjuntó la consigna en PDF y se envió el siguiente pedido:

```text
podemos hacer mi parte que es el especialista de diagramas caso 1 y 2
```

Durante la generación se agregaron estas indicaciones:

```text
no lo subas todavia
```

```text
solo armemos la base para tener todo armado
```

Con ese pedido, la IA leyó por su cuenta los archivos de contexto de la sección 1 y tomó de la consigna los requisitos: al menos 10 actividades y 3 carriles por diagrama, decisiones, bifurcaciones y caminos alternativos, y la nomenclatura `04-actividad-nombre-caso-uso-0X.puml`.

---

## 3. Output obtenido

La IA generó los dos diagramas, los exportó a `.png` con PlantUML, revisó las imágenes renderizadas y creó el índice `diagramas_de_actividades.md`.

### Caso de Uso 1 - Tomar Pedido

```plantuml
@startuml 04-actividad-tomar-pedido-01
skinparam dpi 150
skinparam ArrowFontSize 11

title Diagrama de Actividad - Tomar Pedido (CU01)

|Cliente|
start
:Solicitar pedido al mostrador;

|Usuario de Mostrador|
:Iniciar nuevo pedido;

|Sistema de Pedidos|
:Generar número único del pedido
y registrar fecha y hora;

|Usuario de Mostrador|
:Ingresar nombre o referencia de retiro;

repeat
  :Seleccionar producto o combo y cantidad;
  if (¿Requiere personalización?) then (sí)
    :Agregar personalizaciones
    (quitar ingredientes o adicionales);
  else (no)
  endif
  |Sistema de Pedidos|
  :Calcular subtotal del ítem;
  |Usuario de Mostrador|
repeat while (¿Agregar otro ítem?) is (sí) not (no)

|Sistema de Pedidos|
:Calcular total del pedido;

|Usuario de Mostrador|
if (¿El pedido es prioritario?) then (sí)
  :Marcar pedido como prioritario;
else (no)
endif

repeat
  |Cliente|
  :Elegir medio de pago
  (efectivo o transferencia);
  |Usuario de Mostrador|
  :Registrar medio de pago seleccionado;
  |Sistema de Pedidos|
  :Validar medio de pago;
backward :Informar rechazo de la transferencia;
repeat while (¿Transferencia rechazada y el cliente reintenta?) is (sí) not (no)

if (¿Pago aprobado?) then (no)
  :Descartar pedido sin guardarlo;
  stop
else (sí)
endif

:Registrar pago asociado al pedido;
:Registrar pedido en estado RECIBIDO
conservando los precios históricos;

fork
  :Enviar pedido automáticamente a Cocina;
  |Cocina|
  :Visualizar pedido recibido;
fork again
  |Usuario de Mostrador|
  :Informar número de pedido al cliente;
  |Cliente|
  :Recibir número de pedido para el retiro;
end fork

stop
@enduml
```

### Caso de Uso 2 - Modificar Pedido

```plantuml
@startuml 04-actividad-modificar-pedido-02
skinparam dpi 150
skinparam ArrowFontSize 11

title Diagrama de Actividad - Modificar Pedido (CU02)

|Cliente|
start
:Solicitar modificación del pedido;

|Usuario de Mostrador|
:Buscar pedido por número o referencia;

|Sistema de Pedidos|
:Mostrar información del pedido;
:Verificar estado del pedido;

if (¿Estado RECIBIDO?) then (no)
  :Rechazar modificación por estado;
  |Usuario de Mostrador|
  :Informar al cliente que el pedido
  ya no puede modificarse;
  stop
else (sí)
endif

|Usuario de Mostrador|
:Seleccionar opción modificar pedido;

repeat
  :Agregar, eliminar o modificar
  productos y cantidades;
  :Modificar personalizaciones;
  |Sistema de Pedidos|
  :Recalcular total del pedido;
backward :Impedir pedido sin ítems;
repeat while (¿El pedido quedó sin ítems?) is (sí) not (no)

if (¿El pedido ya estaba pagado?) then (sí)
  :Calcular diferencia respecto
  del pago original;
  if (¿Resultado de la diferencia?) then (a cobrar)
    |Cliente|
    :Abonar diferencia
    (efectivo o transferencia);
    |Sistema de Pedidos|
    :Validar medio de pago;
    :Registrar cobro adicional como ajuste;
  elseif () then (a devolver)
    :Registrar devolución como ajuste;
    |Usuario de Mostrador|
    :Devolver diferencia al cliente;
  else (sin diferencia)
  endif
else (no)
endif

|Usuario de Mostrador|
:Confirmar modificación;

|Sistema de Pedidos|
:Revalidar estado RECIBIDO al confirmar;
if (¿Sigue en estado RECIBIDO?) then (no)
  :Descartar cambios y ajustes no confirmados;
  stop
else (sí)
endif

:Actualizar pedido conservando
el mismo identificador;
:Registrar cambio en el historial del pedido;

|Usuario de Mostrador|
:Informar total actualizado al cliente;

|Cliente|
:Recibir confirmación de la modificación;
stop
@enduml
```

### Decisiones de diseño tomadas por la IA

- **Carriles del CU1:** Cliente, Usuario de Mostrador, Sistema de Pedidos y Cocina, tomados de los actores del diagrama de casos de uso. El Cliente participa porque solicita el pedido, elige el medio de pago y recibe el número para el retiro.
- **Carriles del CU2:** Cliente, Usuario de Mostrador y Sistema de Pedidos. Cocina no se incluyó porque no es actor del CU2 ni interviene en el escenario.
- **Pasos del escenario:** cada paso del flujo principal de A2 se representa como actividad, en el mismo orden.
- **Caminos alternativos:** salen de los `<<extend>>` de los diagramas de casos de uso: "Marcar pedido como prioritario" y "Resolver rechazo de transferencia" en el CU1; "Rechazar modificación por estado", "Impedir pedido sin ítems" y "Registrar ajuste de cobro" en el CU2.
- **Reglas de negocio de los escenarios:** en el CU1, el pedido no se guarda si la transferencia es rechazada. En el CU2, el estado `RECIBIDO` se revalida al confirmar, el identificador no cambia y cada cobro adicional o devolución se registra como ajuste separado.
- **Bifurcación (fork) en el CU1:** el envío automático a Cocina y el aviso del número de pedido al cliente ocurren en paralelo una vez registrado el pedido.

---

## 4. Ajustes realizados al output de la IA

- No se realizaron ajustes manuales: los diagramas se versionaron tal como los generó la IA, después de revisar los `.png` renderizados.

---

## 5. Iteraciones relevantes

- **Iteración 1:** se pidió no subir los cambios al repositorio todavía. La IA trabajó solo de forma local, sin commit ni push.
- **Iteración 2:** se pidió armar la base del rol. La IA creó la rama `feature/esp-actividades-1-2-add-diagrama-actividad-1`, la carpeta `diagramas/04-diagramas-actividades/`, los dos diagramas con sus `.png`, el índice, este informe y la entrada del `changelog.md`.
- **Iteración 3:** se pidió subir el trabajo. La IA hizo el commit y el push de la rama `feature/esp-actividades-1-2-add-diagrama-actividad-1` al repositorio.

---

## 6. Resultado final

- [Diagrama de Actividad - Tomar Pedido](../../diagramas/04-diagramas-actividades/04-actividad-tomar-pedido-01.png)
- [Diagrama de Actividad - Modificar Pedido](../../diagramas/04-diagramas-actividades/04-actividad-modificar-pedido-02.png)
