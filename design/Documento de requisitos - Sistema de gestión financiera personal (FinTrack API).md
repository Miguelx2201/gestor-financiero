# Documento de requisitos de software: FinTrack API

**Cliente:** Sofía Ramírez (usuaria final y patrocinadora del proyecto) **Desarrollador a cargo:** Miguel **Versión del documento:** 1.0 **Fecha:** 28 de octubre de 2026

---

## 1. Contexto y necesidad

Hola Miguel. Soy una persona que quiere tener claridad sobre su plata. Hoy llevo mis ingresos y gastos en una hoja de cálculo y en notas del celular, y me pasa siempre lo mismo: se me olvida registrar cosas, no sé cuánto he gastado en cada categoría y a fin de mes no entiendo adónde se fue el dinero.

Necesito un **sistema de gestión financiera personal** que me permita registrar mis movimientos, organizarlos por cuentas y categorías, definir presupuestos y consultar reportes claros. Por ahora solo me interesa el **backend**: una API REST. Un amigo desarrollará después una aplicación móvil que la consuma, así que la API debe estar bien diseñada y documentada.

## 2. Objetivo del sistema

Permitir que cada usuario controle sus finanzas personales registrando ingresos, gastos y transferencias entre sus cuentas, controlando presupuestos mensuales por categoría y obteniendo reportes de su situación financiera.

## 3. Alcance

### 3.1 Dentro del alcance

- Registro e inicio de sesión de usuarios.
- Gestión de cuentas (efectivo, banco, tarjeta, etc.).
- Gestión de categorías de ingresos y gastos.
- Registro de transacciones (ingresos, gastos y transferencias).
- Presupuestos mensuales por categoría con alertas.
- Reportes y resúmenes financieros.
- Documentación de la API.

### 3.2 Fuera del alcance

- Interfaz gráfica (web o móvil).
- Conexión real con bancos o pasarelas de pago.
- Soporte de varias monedas con conversión automática.
- Envío de correos o notificaciones push (las alertas se consultan por la API).
- Finanzas compartidas entre varios usuarios (por ahora cada usuario ve solo lo suyo).

## 4. Tipos de usuario

| Rol | Descripción |
| --- | --- |
| **USUARIO** | Persona registrada que gestiona únicamente sus propios datos financieros. |
| **ADMIN** | Administra la plataforma: consulta usuarios, los desactiva y gestiona las categorías predefinidas del sistema. No puede ver las transacciones de los usuarios. |

## 5. Glosario

- **Cuenta:** lugar donde está mi dinero (billetera, cuenta de ahorros, tarjeta de crédito).
- **Transacción:** movimiento de dinero. Puede ser ingreso, gasto o transferencia.
- **Categoría:** clasificación de una transacción (Alimentación, Transporte, Salario, etc.).
- **Presupuesto:** límite de gasto que me fijo para una categoría en un mes.
- **Saldo:** dinero disponible en una cuenta en un momento dado.

## 6. Requisitos funcionales

La prioridad indica en qué versión los necesito: **Alta** = versión 1 (MVP), **Media** = versión 2, **Baja** = versión 3 o más adelante.

### 6.1 Autenticación y usuarios

| ID | Requisito | Prioridad |
| --- | --- | --- |
| RF-01 | Quiero registrarme con nombre, correo electrónico y contraseña. | Alta |
| RF-02 | Quiero iniciar sesión con correo y contraseña y recibir un token JWT para usar el resto de la API. | Alta |
| RF-03 | Quiero consultar y actualizar mi perfil (nombre y contraseña). | Alta |
| RF-04 | Quiero que un ADMIN pueda listar usuarios y activar o desactivar cuentas de usuario. | Media |
| RF-05 | Quiero poder eliminar mi cuenta y mis datos. | Baja |

### 6.2 Cuentas

| ID | Requisito | Prioridad |
| --- | --- | --- |
| RF-06 | Quiero crear cuentas indicando nombre, tipo (EFECTIVO, AHORROS, CORRIENTE, TARJETA\_CREDITO) y saldo inicial. | Alta |
| RF-07 | Quiero listar mis cuentas y ver el saldo actual de cada una. | Alta |
| RF-08 | Quiero editar el nombre de una cuenta. | Alta |
| RF-09 | Quiero archivar una cuenta que ya no uso. Una cuenta archivada no admite nuevas transacciones pero conserva su historial. | Media |
| RF-10 | Quiero ver el saldo total consolidado de todas mis cuentas. | Alta |

### 6.3 Categorías

| ID | Requisito | Prioridad |
| --- | --- | --- |
| RF-11 | El sistema debe traer categorías predefinidas (por ejemplo: Alimentación, Transporte, Vivienda, Salud, Entretenimiento, Salario, Otros ingresos), administradas por el ADMIN. | Alta |
| RF-12 | Quiero crear mis propias categorías, indicando si son de INGRESO o de GASTO. | Alta |
| RF-13 | Quiero editar y eliminar mis categorías personalizadas, siempre que no tengan transacciones asociadas. | Media |
| RF-14 | Quiero listar todas las categorías disponibles para mí (predefinidas y propias), filtrando por tipo. | Alta |

### 6.4 Transacciones

| ID | Requisito | Prioridad |
| --- | --- | --- |
| RF-15 | Quiero registrar un **ingreso** indicando cuenta, categoría, monto, fecha y una descripción opcional. | Alta |
| RF-16 | Quiero registrar un **gasto** con los mismos datos. | Alta |
| RF-17 | Quiero registrar una **transferencia** entre dos de mis cuentas, indicando origen, destino, monto y fecha. | Alta |
| RF-18 | Al registrar, editar o eliminar una transacción, el saldo de las cuentas involucradas debe quedar actualizado y consistente. | Alta |
| RF-19 | Quiero consultar el detalle de una transacción. | Alta |
| RF-20 | Quiero editar una transacción registrada (monto, categoría, fecha, descripción). | Media |
| RF-21 | Quiero eliminar una transacción y que el saldo se corrija. | Media |
| RF-22 | Quiero listar mis transacciones con **paginación** y filtros por rango de fechas, cuenta, categoría, tipo y rango de montos, ordenadas de la más reciente a la más antigua. | Alta |
| RF-23 | Quiero buscar transacciones por texto en la descripción. | Baja |

### 6.5 Presupuestos

| ID | Requisito | Prioridad |
| --- | --- | --- |
| RF-24 | Quiero definir un presupuesto mensual para una categoría de gasto, indicando mes, año y monto límite. | Media |
| RF-25 | Quiero editar y eliminar mis presupuestos. | Media |
| RF-26 | Quiero consultar, para un mes dado, cuánto he gastado en cada categoría con presupuesto, cuánto me queda y el porcentaje consumido. | Media |
| RF-27 | Quiero que el sistema me indique el estado de cada presupuesto: **NORMAL** (menos del 80 %), **ALERTA** (entre 80 % y 100 %) o **EXCEDIDO** (más del 100 %). | Media |

### 6.6 Reportes

| ID | Requisito | Prioridad |
| --- | --- | --- |
| RF-28 | Quiero un resumen mensual con total de ingresos, total de gastos y balance (ingresos menos gastos). | Alta |
| RF-29 | Quiero ver el gasto total por categoría en un rango de fechas. | Media |
| RF-30 | Quiero ver la evolución mensual de ingresos y gastos de los últimos 6 o 12 meses. | Media |
| RF-31 | Quiero exportar mis transacciones a un archivo CSV. | Baja |

### 6.7 Transacciones recurrentes y metas (versión 3)

| ID | Requisito | Prioridad |
| --- | --- | --- |
| RF-32 | Quiero programar transacciones recurrentes (arriendo, salario, suscripciones) con frecuencia semanal, quincenal o mensual, y que el sistema las registre automáticamente. | Baja |
| RF-33 | Quiero crear metas de ahorro con nombre, monto objetivo y fecha límite, y registrar aportes a cada una. | Baja |

## 7. Reglas de negocio

| ID | Regla |
| --- | --- |
| RN-01 | El correo electrónico debe ser único en el sistema. |
| RN-02 | La contraseña debe tener mínimo 8 caracteres e incluir letras y números. Nunca se almacena en texto plano. |
| RN-03 | Un usuario solo puede ver y modificar sus propias cuentas, categorías, transacciones y presupuestos. |
| RN-04 | Los montos de las transacciones siempre son **mayores que cero**. El signo lo define el tipo de transacción, no el número. |
| RN-05 | Un **ingreso** suma al saldo de la cuenta. Un **gasto** resta del saldo. |
| RN-06 | Una **transferencia** resta el monto de la cuenta origen y lo suma a la cuenta destino, en una sola operación atómica: o se aplican ambos movimientos o ninguno. |
| RN-07 | La cuenta origen y la cuenta destino de una transferencia deben ser distintas y pertenecer al mismo usuario. |
| RN-08 | Las cuentas de tipo EFECTIVO, AHORROS y CORRIENTE **no pueden quedar con saldo negativo**. Si un gasto o transferencia lo causaría, se rechaza con un mensaje claro. Las cuentas de TARJETA\_CREDITO sí pueden tener saldo negativo (representa deuda). |
| RN-09 | Un ingreso solo puede asociarse a categorías de tipo INGRESO, y un gasto solo a categorías de tipo GASTO. Las transferencias no llevan categoría. |
| RN-10 | La fecha de una transacción no puede ser posterior a hoy. |
| RN-11 | Una cuenta archivada no admite nuevas transacciones. |
| RN-12 | Una categoría con transacciones asociadas no puede eliminarse. |
| RN-13 | Solo puede existir **un presupuesto por categoría, mes y año** para cada usuario. |
| RN-14 | Un presupuesto solo aplica a categorías de tipo GASTO. |
| RN-15 | El gasto consumido de un presupuesto se calcula sumando los gastos de esa categoría dentro del mes y año del presupuesto, en todas las cuentas del usuario. |
| RN-16 | Todos los montos se manejan en **pesos colombianos (COP)** y deben representarse con precisión decimal (sin errores de redondeo por punto flotante). |
| RN-17 | Si edito o elimino una transacción, el saldo de las cuentas se recalcula de forma consistente, respetando la regla RN-08. |
| RN-18 | Las categorías predefinidas solo las gestiona el ADMIN y los usuarios no pueden modificarlas ni eliminarlas. |

## 8. Requisitos no funcionales

### 8.1 Tecnología (restricciones que impongo)

- Lenguaje: **Java** (versión LTS vigente).
- Framework: **Spring Boot**.
- Base de datos: **PostgreSQL**.
- Persistencia con **Spring Data JPA / Hibernate**.
- Gestor de dependencias: Maven o Gradle, a tu elección.
- Control de versiones con **Git** y repositorio en GitHub.

### 8.2 Seguridad

- RNF-01: Autenticación con **JWT** y autorización por roles.
- RNF-02: Contraseñas almacenadas con un algoritmo de hash seguro (por ejemplo, BCrypt).
- RNF-03: Ningún endpoint, salvo registro e inicio de sesión, debe ser accesible sin autenticación.
- RNF-04: Las credenciales y secretos (base de datos, clave del JWT) no pueden estar escritos en el código; deben venir de variables de entorno.
- RNF-05: La API nunca debe exponer directamente las entidades de la base de datos; debe usar **DTOs**.

### 8.3 Calidad y mantenibilidad

- RNF-06: Arquitectura en capas (controlador, servicio, repositorio) con separación clara de responsabilidades.
- RNF-07: Validación de todos los datos de entrada, con mensajes de error comprensibles.
- RNF-08: Manejo global y consistente de errores, con una estructura de respuesta uniforme (código HTTP, mensaje, fecha y detalle).
- RNF-09: Cobertura de pruebas razonable: pruebas unitarias de la lógica de negocio y pruebas de integración de los flujos principales. Me importa especialmente que estén probadas las reglas RN-06, RN-08 y RN-15.
- RNF-10: Código legible y consistente, con nombres claros.

### 8.4 Datos

- RNF-11: El esquema de base de datos debe gestionarse con **migraciones versionadas** (Flyway o Liquibase), no con creación automática de tablas.
- RNF-12: Las operaciones que modifican saldos deben ser **transaccionales**.
- RNF-13: Debe haber integridad referencial y restricciones en la base de datos (llaves foráneas, unicidad, no nulos), no solo validaciones en el código.

### 8.5 Rendimiento y usabilidad de la API

- RNF-14: Los listados deben estar paginados. Un tamaño de página por defecto de 20 y máximo de 100.
- RNF-15: Los endpoints de consulta comunes deben responder en menos de 1 segundo con hasta 10.000 transacciones por usuario.
- RNF-16: La API debe seguir convenciones REST: URLs en plural y sustantivos, verbos HTTP correctos y códigos de estado apropiados (200, 201, 204, 400, 401, 403, 404, 409).
- RNF-17: Debe estar documentada con **OpenAPI / Swagger**, accesible desde el navegador.

### 8.6 Despliegue

- RNF-18: Debe poder levantarse completa (aplicación y base de datos) con **Docker** y `docker-compose` mediante un solo comando.
- RNF-19: El repositorio debe incluir un **README** con descripción, tecnologías, instrucciones para ejecutarla y ejemplos de uso.

## 9. Casos de uso principales

Para que tengas claro cómo uso el sistema, estos son los flujos que más me importan:

1. **Empezar a usar el sistema:** me registro, inicio sesión, creo mis cuentas con su saldo inicial y reviso las categorías disponibles.
2. **Registrar mi día a día:** anoto un gasto de almuerzo en mi cuenta de efectivo y veo cómo baja el saldo.
3. **Recibir mi salario:** registro un ingreso en mi cuenta de ahorros con la categoría Salario.
4. **Mover plata:** transfiero dinero de mi cuenta de ahorros a mi billetera y ambos saldos cambian a la vez.
5. **Controlar un presupuesto:** fijo 400.000 COP para Alimentación este mes y consulto cuánto llevo gastado y si estoy en alerta.
6. **Revisar mi mes:** consulto el resumen del mes y el gasto por categoría para entender adónde se fue mi plata.

## 10. Criterios de aceptación (ejemplos)

Consideraré que el sistema cumple cuando, como mínimo, se verifiquen estos escenarios:

- **CA-01:** Si tengo 100.000 COP en una cuenta de ahorros e intento registrar un gasto de 150.000 COP, el sistema lo rechaza y el saldo se mantiene en 100.000.
- **CA-02:** Si transfiero 50.000 COP de la cuenta A (saldo 200.000) a la cuenta B (saldo 30.000), quedan en 150.000 y 80.000. Si ocurre cualquier error a mitad de la operación, ambos saldos permanecen como estaban.
- **CA-03:** Si tengo un presupuesto de 400.000 para Alimentación y llevo gastados 340.000, el sistema indica 85 % consumido y estado ALERTA.
- **CA-04:** Si otro usuario intenta consultar una de mis transacciones, la API responde que no existe o que no tiene acceso, sin revelar su contenido.
- **CA-05:** Si intento registrar una transacción con fecha futura o con monto negativo, la API responde con error de validación y un mensaje claro.
- **CA-06:** Al listar transacciones con filtro de fechas y paginación, recibo solo las del rango pedido, ordenadas de la más reciente a la más antigua, junto con el total de resultados.
- **CA-07:** El proyecto se levanta desde cero con `docker-compose up` y la documentación Swagger queda disponible.

## 11. Planificación por versiones

Prefiero recibir el sistema por etapas funcionales que esperar a tenerlo todo:

| Versión | Contenido | Requisitos |
| --- | --- | --- |
| **v1 (MVP)** | Autenticación, cuentas, categorías, transacciones básicas, resumen mensual, Docker y documentación | Prioridad Alta |
| **v2** | Edición y eliminación de transacciones, presupuestos y alertas, reportes por categoría y evolución mensual, rol ADMIN | Prioridad Media |
| **v3** | Transacciones recurrentes, metas de ahorro, exportación CSV, búsqueda por texto | Prioridad Baja |

## 12. Entregables esperados

Por cada versión, espero recibir:

1. Código fuente en un repositorio de GitHub con historial de commits ordenado.
2. Diagrama de clases, modelo entidad-relación y modelo relacional (a tu criterio de diseño).
3. Scripts de migración de la base de datos.
4. Pruebas automatizadas.
5. Documentación de la API (Swagger) y README.
6. Configuración de Docker.

## 13. Supuestos y decisiones que te dejo a ti

Como cliente, no me interesan los detalles internos, así que estas decisiones las tomas tú. Solo te pido que las justifiques brevemente en el README:

- Cómo modelar el saldo de una cuenta: campo almacenado y actualizado, o calculado a partir de las transacciones.
- Cómo representar las transferencias: una sola fila con origen y destino, o dos movimientos enlazados.
- Cómo gestionar las categorías predefinidas frente a las personalizadas.
- Estrategia de eliminación: física o lógica (marcar como eliminado).
- Estructura de paquetes y organización del proyecto.
- Cómo evitar problemas cuando dos operaciones modifican el mismo saldo al mismo tiempo (concurrencia).

## 14. Preguntas abiertas

Hay cosas que todavía no tengo del todo claras. Si durante el desarrollo ves que alguna te bloquea, propón una solución razonable y déjala documentada:

- ¿Debe la tarjeta de crédito tener cupo máximo y fecha de corte?
- ¿Qué pasa con las transacciones de una categoría cuando un ADMIN desactiva esa categoría predefinida?
- ¿Conviene permitir adjuntar una imagen del recibo a una transacción?

---

*Este documento es un punto de partida. Si tienes dudas o detectas ambigüedades, pregúntame como lo haría un cliente real: los requisitos se afinan conversando.*
