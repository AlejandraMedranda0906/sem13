# TAS12 - Subconsultas-invoice_db
## 1. El numero total de facturas realizadas por cada cliente. nombre_cliente | direccion | nro_facturas
  - Sentencia:
  ```
  SELECT
  c.fullname,
  c.address,
  (SELECT COUNT(*) 
  FROM
  invoice i
  WHERE 
  i.client_id = c.id) AS nro_facturas
  FROM
  client c;

  ```
  - Captura:

<img src="./capturas_tas12/01.jpg" alt="drawing" width="500"/>

## 2. Listar nombre y correo de los clientes junto a su compra mas cara realizada. nombres |  correo   | total_mas_alto
  - Sentencia:
  ```
  SELECT
  c.fullname AS nombres,
  c.email AS correo,
  (SELECT MAX(i.total) 
  FROM invoice i
  WHERE i.client_id = c.id) AS total_mas_alto
  FROM
  client c;

  ```
  - Captura:

<img src="./capturas_tas12/02.jpg" alt="drawing" width="500"/>

## 3. Listar las facturas donde sus totales sean mayores al promedio de las facturas. fecha_factura | total
  - Sentencia:
  ```
  SELECT i.create_at AS fecha_factura, i.total
  FROM
  invoice i
  WHERE i.total > (SELECT AVG(total) FROM invoice);

  ```
  - Captura:

<img src="./capturas_tas12/03.jpg" alt="drawing" width="500"/>


# TAS12 - Subconsultas-event_db
## 1. Crear dos ejemplo con la base de datos event. Uno con subconsulta en SELECT. 
### Listar todos los miembros junto con el número de conferencias a las que se han registrado.
  - Sentencia:
  ```
  SELECT
  m.fullname AS nombre_miembro,
  m.email AS correo,
  (SELECT COUNT(*)
  FROM register r
  WHERE r.member_id = m.id) AS numero_conferencias
  FROM
  member m;

  ```
  - Captura:

<img src="./capturas_tas12/04.jpg" alt="drawing" width="500"/>

## 2. subconsulta  en WHERE
### Listar los eventos donde el total de asistentes es mayor que el número promedio de asistentes en todos los eventos.
  - Sentencia:
  ```
  SELECT
  e.description AS descripcion_evento,
  e.start_date AS fecha_inicio,
  e.end_date AS fecha_fin,
  e.total_attendees AS total_asistentes,
  e.city AS ciudad
  FROM
  event e
  WHERE
  e.total_attendees > (SELECT AVG(total_attendees)
  FROM event);

  ```
  - Captura:

<img src="./capturas_tas12/05.jpg" alt="drawing" width="500"/>

