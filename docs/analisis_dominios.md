# Análisis de Dominios Funcionales

# Dominio 1: geografía y Datos de Referencia

**Tablas:**
- `time_zone`
- `continent`
- `country`
- `state_province`
- `city`
- `district`
- `address`
- `currency`

**Propósito:**
Agrupa toda la información geográfica y de referencia del sistema. Provee 
la estructura base de ubicaciones (desde continentes hasta direcciones 
específicas) y datos monetarios (monedas) que son reutilizados por 
prácticamente todos los demás dominios. Sin este dominio, ninguna entidad 
del sistema podría situarse geográficamente ni operar con divisas.

**Relaciones clave:**
- `country` → es referenciado por `airline` (país de origen de la aerolínea)
  y por `person` (nacionalidad del pasajero).
- `address` → es referenciado por `airport` (ubicación física del aeropuerto)
  y por `maintenance_provider` (ubicación del proveedor de mantenimiento).
- `currency` → es referenciado por `loyalty_program`, `fare`, `payment`,
  `invoice` y `exchange_rate`.
- `city` → depende de `state_province`, que depende de `country`, que depende
  de `continent`. Es la cadena jerárquica principal del dominio.

**Orden de carga (para datos de prueba):**
`continent` → `country` → `state_province` → `time_zone` → `city` 
→ `district` → `address` → `currency`

# Dominio 2: Aerolínea

**Tablas**
- `airline`

**Proposito**
Este dominio gestiona la información base de las aerolíneas que operan dentro del sistema. Define la identidad de cada aerolínea y su país de origen, permitiendo asociarla con vuelos, operaciones y configuraciones del sistema. Es fundamental para contextualizar la operación aérea dentro de una organización específica.

**Relaciones clave**

- `airline`-> es referenciado por `flight` (Para indicar la aerolínea operadora)
- `airline`-> se relaciona con `country`(dominio geográfico) para definir su país de origen.

**Orden de carga (para datos de prueba):**
`airline` -> `country`

# Dominio 3: Identidad

**Tablas**

- `person_type`
- `document_type`
- `contact_type`
- `person`
- `person_document`
- `person_contact`

**Proposito**
Este dominio gestiona la información personal de los individuos en el sistema, incluyendo pasajeros, empleados y otros actores. Permite almacenar datos básicos, documentos de identificación y medios de contacto, garantizando una representación completa y flexible de cada persona.

**Relaciones clave**

- `person` -> se relaciona con `person_type` para clasificar el tipo de individuo.
- `person_document` establece una relación uno a muchos entre `person` y `document_type`
- `person_contact` establece una relación uno a muchos entre `person` y `contact_type`
- `person` es referenciado por `customer`y procesos de reserva.

**Orden de carga (para datos de prueba):**

`person_type` -> `document_type` -> `contact_type` -> `person` -> `person_document` -> `person_contact`

# Dominio 4: Seguridad

**Tablas**

- `user_status`
- `security_role`
- `security_permission`
- `user_account`
- `user_role`
- `role_permission`

**Proposito**

Este dominio administra la autenticación y autorización de usuarios en el sistema. Permite gestionar cuentas, estados, roles y permisos, garantizando el control de acceso a las funcionalidades según el perfil del usuario.

**Relaciones clave**
- `user_account` se asocia a `person` para identificar al usuario.
- `user_role` define una relación muchos a muchos entre `user_account` y `security_role`.
- `role_permission` define una relación muchos a muchos entre `security_role` y `security_permission`.

**Orden de carga (para datos de prueba):**

`user_status` → `security_role` → `security_permission` → `user_account` → `user_role` → `role_permission`

# Dominio 5: Clientes y Fidelización

**Tablas**

- `customer_category`
- `benefit_type`
- `loyalty_program`
- `loyalty_tier`
- `customer`
- `loyalty_account`
- `loyalty_account_tier`
- `miles_transaction`
- `customer_benefit`

**Proposito**

Este dominio gestiona la información de clientes y programas de fidelización. Permite clasificar clientes, asignar beneficios, administrar cuentas de millas y niveles de fidelidad, incentivando la recurrencia y el valor del cliente dentro del sistema.

**Relaciones clave**

- `customer` se relaciona con `person`.
- `loyalty_account` se asocia a `customer`.
- `loyalty_account_tier` vincula cuentas con niveles `(loyalty_tier)`.
- `miles_transaction` registra movimientos de millas.
- `customer_benefit` relaciona clientes con beneficios.

**Orden de carga (para datos de prueba):**

`customer_category` → `benefit_type` → `lovalty_program` → `lovalty_tier` → `customer` → `lovalty_account` -> `loyalty_account_tier` → `miles_transaction` → `customer_benefit`

**Dominio 6: Aeropuerto**

**Tablas:**

- `airport`
- `terminal`
- `boarding_gate`
- `runway`
- `airport_regulation`

**Propósito:** 
Este dominio gestiona la infraestructura aeroportuaria, incluyendo aeropuertos, terminales, puertas de embarque y pistas. Permite modelar el entorno físico donde se desarrollan las operaciones de vuelo.

**Relaciones clave:**

- `airport` se relaciona con `address` para su ubicación.
- `terminal` depende de `airport` (relación uno a muchos).
- `boarding_gate` depende de `terminal`.
- `runway` depende de `airport`.
- `airport_regulation` se asocia a `airport`.

**Orden de carga:**
airport → terminal → boarding_gate → runway → airport_regulation

Dominio 7: Aeronaves

Tablas:

aircraft_manufacturer
aircraft_model
cabin_class
aircraft
aircraft_cabin
aircraft_seat
maintenance_provider
maintenance_type
maintenance_event

Propósito:
Este dominio gestiona la información de aeronaves, su configuración interna y mantenimiento. Permite definir modelos, clases de cabina, distribución de asientos y registrar eventos de mantenimiento.

Relaciones clave:

aircraft_model se relaciona con aircraft_manufacturer.
aircraft se asocia a aircraft_model.
aircraft_cabin depende de aircraft y cabin_class.
aircraft_seat depende de aircraft_cabin.
maintenance_event se relaciona con aircraft, maintenance_type y maintenance_provider.

Orden de carga:
aircraft_manufacturer → aircraft_model → cabin_class → aircraft → aircraft_cabin → aircraft_seat → maintenance_provider → maintenance_type → maintenance_event

🛬 Dominio 8: Operaciones de Vuelo

Tablas:

flight_status
delay_reason_type
flight
flight_segment
flight_delay

Propósito:
Este dominio gestiona la operación de vuelos, incluyendo su programación, estados, segmentos y retrasos. Permite representar la ejecución real de los vuelos dentro del sistema.

Relaciones clave:

flight se asocia a airline.
flight_segment depende de flight.
flight se relaciona con flight_status.
flight_delay se asocia a flight y delay_reason_type.

Orden de carga:
flight_status → delay_reason_type → flight → flight_segment → flight_delay

🎟️ Dominio 9: Ventas y Reservas

Tablas:

reservation_status
sale_channel
fare_class
fare
ticket_status
reservation
reservation_passenger
sale
ticket
ticket_segment
seat_assignment
baggage

Propósito:
Este dominio gestiona el proceso de venta de tiquetes y reservas. Incluye la creación de reservas, emisión de tiquetes, asignación de asientos y gestión de equipaje.

Relaciones clave:

reservation se relaciona con customer.
reservation_passenger vincula reservas con personas.
ticket se asocia a reservation.
ticket_segment conecta tiquetes con segmentos de vuelo.
seat_assignment asigna asientos a pasajeros.

Orden de carga:
reservation_status → sale_channel → fare_class → fare → ticket_status → reservation → reservation_passenger → sale → ticket → ticket_segment → seat_assignment → baggage

🛂 Dominio 10: Abordaje

Tablas:

boarding_group
check_in_status
check_in
boarding_pass
boarding_validation

Propósito:
Este dominio gestiona el proceso de check-in y abordaje de pasajeros. Permite validar el acceso al vuelo mediante pases de abordar y controlar el flujo de embarque.

Relaciones clave:

check_in se relaciona con ticket.
boarding_pass se genera a partir de check_in.
boarding_validation valida el pase de abordar.
boarding_group organiza el orden de embarque.

Orden de carga:
boarding_group → check_in_status → check_in → boarding_pass → boarding_validation

💳 Dominio 11: Pagos

Tablas:

payment_status
payment_method
payment
payment_transaction
refund

Propósito:
Este dominio gestiona las transacciones financieras relacionadas con la compra de servicios. Permite registrar pagos, estados, métodos y devoluciones.

Relaciones clave:

payment se relaciona con sale.
payment_transaction registra movimientos asociados al pago.
refund se asocia a payment.

Orden de carga:
payment_status → payment_method → payment → payment_transaction → refund

🧾 Dominio 12: Facturación

Tablas:

tax
exchange_rate
invoice_status
invoice
invoice_line

Propósito:
Este dominio gestiona la generación de facturas y cálculos fiscales. Se encarga de la documentación legal de las transacciones, separándose del dominio de pagos para mantener una correcta gestión contable.

Relaciones clave:

invoice se relaciona con payment.
invoice_line depende de invoice.
tax se aplica a las líneas de factura.
exchange_rate permite conversión de monedas.

Orden de carga:
tax → exchange_rate → invoice_status → invoice → invoice_line