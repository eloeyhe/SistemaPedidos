| **Nombre del escenario:**   | Flujo principal - Consulta de pedidos activos priorizando los urgentes                                                                                  |   |               |         |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | - | ------------- | ------- |
| **Nombre del caso de uso:** | Consultar Pedidos Activos                                                                                                                               |   | **ID Única:** | CU05-01 |
| **Área**                    | Sistema Sabor Kiosco, consulta y seguimiento de pedidos activos                                                                                         |   |               |         |
| **Actor(es):**              | Cocina, Usuario de Mostrador                                                                                                                            |   |               |         |
| **Descripción:**            | Permite consultar los pedidos activos agrupados o filtrados por estado, mostrando primero los pedidos marcados como urgentes dentro de un mismo estado. |   |               |         |

| **Activar Evento:** | Cocina o el Usuario de Mostrador accede al monitor de pedidos activos para consultar su estado y prioridad. | **Identificadores e iniciadores de caso de uso** |
| ------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Tipo de señal:**  | ☑️ Externa                                                                                                  | ☐ Temporal                                       |

| **Pasos desempeñados (ruta principal)**                                      | **Información para los pasos**                 |
| ---------------------------------------------------------------------------- | ---------------------------------------------- |
| 1. El actor accede al monitor de pedidos activos                             | Monitor de pedidos                             |
| 2. El sistema obtiene los pedidos que se encuentran activos                  | Estados `RECIBIDO`, `EN_PREPARACION` y `LISTO` |
| 3. El sistema agrupa o filtra los pedidos según su estado                    | Estado actual                                  |
| 4. El sistema adapta la visualización según el actor que realiza la consulta | Cocina / Usuario de Mostrador                  |
| 5. El sistema identifica los pedidos marcados como urgentes                  | Indicador de prioridad                         |
| 6. El sistema muestra primero los pedidos urgentes dentro del mismo estado   | Prioridad del pedido                           |
| 7. El actor consulta la información de los pedidos activos                   | Información del pedido                         |
| 8. El sistema mantiene un único estado actual para cada pedido mostrado      | Estado del pedido                              |

| **Condiciones, suposiciones y preguntas** |                                                                                                                                                                            |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Precondiciones:**                       | El actor tiene acceso al sistema. Existen pedidos activos para consultar.                                                                                                  |
| **Poscondiciones:**                       | Los pedidos activos quedan disponibles para consulta, mostrando su estado actual y respetando la prioridad de los pedidos urgentes dentro del mismo estado.                |
| **Suposiciones:**                         | Los pedidos activos corresponden a los estados `RECIBIDO`, `EN_PREPARACION` y `LISTO`. La prioridad urgente se encuentra registrada en el pedido.                          |
| **Requerimientos:**                        | RF3, RF7, RNF2, RNF4, RNF5                                                                                                                                                          |
| **Aspectos sobresalientes:**              | Solo se muestran pedidos `RECIBIDO`, `EN_PREPARACION` y `LISTO`; cada pedido aparece una sola vez. Los prioritarios se anteponen dentro de su estado. Si no hay pedidos se muestra la vista vacía y, ante una falla de conexión, se advierte que la información puede estar desactualizada. |
| **Prioridad:**                            | Alta                                                                                                                                                                       |
| **Riesgo:**                               | Medio                                                                                                                                                                      |
