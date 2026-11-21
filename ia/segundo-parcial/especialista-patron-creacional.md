# IA – Especialista en Patrón de Diseño Creacional (Segundo Parcial)

**Rol:** Especialista en Patrón de Diseño Creacional
**Patrón seleccionado:** Builder
**Herramienta:** GitHub Copilot – Agent Mode
**Entregables generados:**

- `diagramas/01-diagrama-clases/01-patron-creacional-builder.puml`
- `diagramas/01-diagrama-clases/01-patron-creacional-builder.png`
- `anexos/patrones-diseno/patron-de-diseno-creacional.md`

---

## Archivos de contexto referenciados

- `diagramas/01-diagrama-clases/06-clases-diagrama-final.puml` (diagrama de clases final; la consigna lo nombra `01-diagrama-clases-final.puml`)
- `diagramas/01-diagrama-clases/01-solid-01-srp.puml`
- `diagramas/01-diagrama-clases/01-solid-02-ocp.puml`
- `diagramas/01-diagrama-clases/01-solid-03-lsp.puml`
- `diagramas/01-diagrama-clases/01-solid-04-isp.puml`
- `diagramas/01-diagrama-clases/01-solid-05-dip.puml`

---

## Iteración 1 – Identificación del problema y selección del patrón

### Prompt utilizado

```
Actuá como especialista en patrones de diseño GoF. Tomando como base el diagrama de clases final
#file:06-clases-diagrama-final.puml y los diagramas SOLID #file:01-solid-01-srp.puml
#file:01-solid-02-ocp.puml #file:01-solid-03-lsp.puml #file:01-solid-04-isp.puml
#file:01-solid-05-dip.puml del Sistema de Pedidos:

1. Identificá un problema concreto de creación de objetos en el sistema.
2. Evaluá cuál de los patrones creacionales (Singleton, Factory Method, Abstract Factory,
   Builder, Prototype) lo resuelve mejor y justificá por qué descartás los demás.
3. Indicá qué clases existentes cambian de rol y qué clases nuevas se incorporan.

No generes código todavía, solo el análisis.
```

### Output obtenido

> _(Pegar acá el resumen de la respuesta de Copilot.)_

### Ajustes realizados al output

> _(Describir qué se aceptó, qué se corrigió y por qué.)_

---

## Iteración 2 – Generación del diagrama de clases

### Prompt utilizado

```
Con el análisis anterior, generá el archivo
diagramas/01-diagrama-clases/01-patron-creacional-builder.puml aplicando el patrón Builder
a la construcción de Pedido en el CU1 (Tomar pedido). Requisitos:

- Incluí solo las clases que participan del patrón: ServicioPedidos (Director),
  IPedidoBuilder (Builder), PedidoBuilder (ConcreteBuilder), Pedido (Product), ItemPedido,
  PersonalizacionItem, Producto, Combo, Pago y EstadoPedido.
- Respetá los nombres, atributos, tipos y multiplicidades del diagrama de clases final.
- ServicioPedidos ya no debe tener pedidoEnCurso ni itemEnCurso; recibe IPedidoBuilder
  por constructor (DIP, igual que en 01-solid-05-dip.puml).
- El constructor de Pedido debe tener visibilidad de paquete y reemplazar a
  registrarPedido(...) de 6 parámetros.
- construir() debe validar referencia de retiro, al menos un ítem y pago con monto
  igual al total.
- Usá estereotipos <<Director>>, <<Builder>>, <<ConcreteBuilder>> y <<Product>>, las
  mismas skinparam del diagrama final y notas que expliquen la validación.
```

### Output obtenido

> _(Pegar acá el resumen o fragmento relevante del .puml que devolvió Copilot.)_

### Ajustes realizados al output

> _(Ejemplos de ajustes a documentar si ocurrieron: nombres o tipos que no coincidían con el
> diagrama final, relaciones o multiplicidades incorrectas, clases que sobraban, notas
> faltantes, errores de renderizado de PlantUML.)_

---

## Iteración 3 – Redacción del anexo

### Prompt utilizado

```
Redactá anexos/patrones-diseno/patron-de-diseno-creacional.md siguiendo exactamente esta
estructura de la consigna:

# Anexo – Aplicación de Patrón de Diseño Creacional – Builder
## Patrones de Diseño Creacionales y su relación con SOLID
## Propósito y Tipo del Patrón (Propósito / Tipo)
## Motivación (funcionamiento original y limitaciones, clases involucradas, problemas de
   mantenibilidad, nuevas clases y su función, cómo el patrón reorganiza la arquitectura)
## Estructura de Clases (imagen incrustada de
   ../../diagramas/01-diagrama-clases/01-patron-creacional-builder.png y enlace
   "Ver diagrama en tamaño completo")
## Justificación Técnica de la Estructura de Clases (descripción de cada clase:
   responsabilidad, relaciones, por qué es necesaria; y flujo de creación indicando qué
   clase inicia el flujo, cuál delega y cuál instancia el objeto final)

Basate en #file:01-patron-creacional-builder.puml y #file:06-clases-diagrama-final.puml.
Usá estilo académico en español, sin inventar clases que no estén en el diagrama.
```

### Output obtenido

> _(Pegar acá el resumen de la respuesta de Copilot.)_

### Ajustes realizados al output

> _(Describir correcciones de redacción, ortografía, consistencia con el diagrama, etc.)_

---

## Conclusión

> _(Breve reflexión: qué aportó la IA, qué hubo que revisar manualmente y cómo se verificó la
> consistencia con el diagrama de clases final.)_
