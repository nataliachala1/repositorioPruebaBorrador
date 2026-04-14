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
