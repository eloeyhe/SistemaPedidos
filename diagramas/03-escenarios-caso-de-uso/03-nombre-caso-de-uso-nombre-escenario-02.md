| **Nombre del escenario:** |Flujo principal - Registro de Socio exitoso | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Registro de Socio | | **ID Única:** | 1 |
| **Área** | Sistema del Gimnasio FitnessPro, Alta de socios | | | |
| **Actor(es):** | Socio, Recepcion | | | |
| **Descripción:** | Permite al socio registrar su información tal como nombre, dirección, el teléfono, edad, historial médico | | | |

| **Activar Evento:** | El socio se acerca a mostrador en recepción, donde le toman sus datos correspondientes, al hacer click en registrar el socio recibe un número de credencial por el sistema | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. El socio se presenta en recepción | El socio tiene su dni con sus datos requeridos |
| 2. El recepcionista ingresa al sistema, y toma los datos | Usuario y clave del operador del sistema |
| 3. Se despliega un formulario de registro para cargar los datos | Formulario web de Registro de socio |
| 4. Se introducen los datos y se da click en el botón "Registrar Socio" | Formulario web de Registro de socio |
| 5. Se validan que los datos requeridos del socio sean correctos | Formulario web de Registro de socio |
| 6. Se valida que el socio no exista | Formulario web de Registro de socio |
| 7. Se escribe un registro de socio y se genera un número de credencial | Formulario web de Registro de socio, BD Registro de Socio |
| 8. La página web de confirmación muestra al usuario el nro de credencial | Página de confirmación |
| 9. Se le brinda al nuevo socio ese nro asociado | QR, tarjeta, etc. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El operador tiene un usuario y clave válidos. El Socio tiene los datos necesarios |
| **Poscondiciones:** | El socio queda reitrado exitosamente |
| **Suposiciones:** | EL socio tiene el DNI. Que hay un navegador para ingresar al sitio. |
| **Reunir requerimentos:** | Registro de Socios |
| **Aspectos sobresalientes:** | ¿se debe controlar el numero de intentos de registro?¿El usuario puede cambiar la clave?¿La tarjeta, QR se genera en el momento?¿El socio ahora tiene usuario y contraseña en el sistema? |
| **Prioridad:** | Tiempo (Alta - Media -Baja) |
| **Riesgo:** | Tiempo - Costo (Alto - Medio -Bajo) |