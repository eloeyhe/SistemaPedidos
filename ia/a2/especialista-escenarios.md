# Especialista en Escenarios de Casos de Uso

## Rol

Especialista en Escenarios de Casos de Uso.

## Objetivo

Elaborar los escenarios correspondientes a los casos de uso definidos para el MVP de Sabor Kiosco, respetando los requisitos funcionales y no funcionales establecidos en la Actividad Obligatoria N°1.

## Prompt utilizado

Necesito que leas en introduccion.md y la plantilla de escenarios para completar los campos de cada escenario (ID, área, actores, descripción, evento activador, tipo de señal, flujo principal, pre/postcondiciones, suposiciones, requerimientos, aspectos sobresalientes, prioridad y riesgo) para cada caso de uso.

## Archivos utilizados como contexto

- `anexos/introduccion.md`
- Plantilla de escenarios de casos de uso proporcionada para la Actividad Obligatoria N°2.

## Casos de uso trabajados

Se desarrolló un escenario para cada uno de los cinco casos de uso definidos en la Actividad Obligatoria N°1:

1. CU01 - Tomar Pedido
2. CU02 - Modificar Pedido
3. CU03 - Cancelar Pedido
4. CU04 - Cambiar Estado de Pedido
5. CU05 - Consultar Pedidos Activos

## Ajustes realizados

Durante la elaboración de los escenarios se realizaron los siguientes ajustes:

- Se adaptó la estructura de la plantilla proporcionada a los casos de uso del proyecto Sabor Kiosco.
- Se mantuvieron los actores definidos en `anexos/introduccion.md`.
- Se respetaron los estados de los pedidos definidos en el proyecto: `RECIBIDO`, `EN_PREPARACION`, `LISTO`, `ENTREGADO` y `CANCELADO`.
- Se relacionaron los escenarios con los requisitos funcionales correspondientes.
- Se definió una ruta principal de al menos cinco pasos para cada escenario.
- Se incorporó la información necesaria para cada paso de la ruta principal.
- Se mantuvieron las restricciones existentes, por ejemplo, que un pedido solamente puede modificarse mientras se encuentre en estado `RECIBIDO`.
- En la cancelación se mantuvo el registro histórico del pedido y su identificador.
- En la consulta de pedidos activos se contempló la prioridad de los pedidos marcados como urgentes dentro de un mismo estado.
- Se evitaron agregar comportamientos que no estuvieran definidos en los requisitos o casos de uso de la Actividad Obligatoria N°1.

## Resultado

Se generaron cinco archivos individuales dentro de:

`diagramas/03-escenarios-casos-de-uso/`

Los archivos corresponden a:

- `03-tomar-pedido-registro-pedido-01.md`
- `03-modificar-pedido-modificacion-recibido-01.md`
- `03-cancelar-pedido-cancelacion-antes-entrega-01.md`
- `03-cambiar-estado-pedido-avance-preparacion-01.md`
- `03-consultar-pedidos-activos-prioridad-urgentes-01.md`

También se creó el índice:

- `escenarios_de_casos_de_uso.md`