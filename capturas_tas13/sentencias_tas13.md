# Crear triggers
### 1.- Crear un función y un trigger para validar que el numero de cedula del client tenga 10 números (no letras) en la tabla "client".
  - Sentencia:
  ```
  CREATE OR REPLACE FUNCTION validar_cedula() RETURNS TRIGGER AS $$
BEGIN
    IF NOT (NEW.nui ~ '^\d{10}$') THEN
        RAISE EXCEPTION 'El número de cédula debe tener 10 dígitos.';
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER validar_cedula_trigger
BEFORE INSERT OR UPDATE ON client
FOR EACH ROW
EXECUTE PROCEDURE validar_cedula();

  ```
  - Captura:

<img src="./capturas_tas13/trigger_01.jpg"alt="drawing" width="500"/>

### 2. Crear un función y un trigger para que cada vez que se inserte un nuevo registro en la tabla item se disminuya el stock de la tabla product.
  - Sentencia:
  ```
  CREATE OR REPLACE FUNCTION decrease_stock() RETURNS TRIGGER AS $$
BEGIN
    UPDATE product
    SET stock = stock - NEW.quantity
    WHERE id = NEW.product_id;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER decrease_stock_trigger
AFTER INSERT ON detail
FOR EACH ROW
EXECUTE PROCEDURE decrease_stock();

  ```
  - Captura:

<img src="./capturas_tas13/trigger_02.jpg"alt="drawing" width="500"/>

### 3. Crear un función y un trigger para la tabla invoice donde valide que el campo create_at sea del año actual (fecha sistema).
  - Sentencia:
  ```
  CREATE OR REPLACE FUNCTION validar_fecha_actual()
  RETURNS TRIGGER AS $$
  BEGIN
  IF EXTRACT(YEAR FROM NEW.create_at) != EXTRACT(YEAR FROM CURRENT_DATE) THEN
    RAISE EXCEPTION 'La fecha de creación (create_at) debe ser del año actual.';
  END IF;
  RETURN NEW;
  END;
  $$ LANGUAGE plpgsql;

  CREATE TRIGGER trigger_validar_fecha_actual
  BEFORE INSERT OR UPDATE ON invoice
  FOR EACH ROW EXECUTE PROCEDURE validar_fecha_actual();
  ```
  - Captura:

<img src="./capturas_tas13/trigger_03.jpg" alt="drawing" width="500"/>

### 4. Crear un función y un trigger para la tabla client y validar que el correo tenga un @.
  - Sentencia:
  ```
  CREATE OR REPLACE FUNCTION valida_email() RETURNS TRIGGER AS $$
  BEGIN
  IF POSITION('@' IN NEW.email) = 0 THEN
    RAISE EXCEPTION 'Su correo electrónico debe tener un @.';
  END IF;
  RETURN NEW;
  END;
  $$ LANGUAGE plpgsql;

  CREATE TRIGGER trigger_valida_email
  BEFORE INSERT OR UPDATE ON client
  FOR EACH ROW 
  EXECUTE PROCEDURE valida_email();
  ```
  - Captura:

<img src="./capturas_tas13/trigger_04.pjg" alt="drawing" width="500"/>