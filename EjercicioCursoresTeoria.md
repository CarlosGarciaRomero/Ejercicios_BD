# CURSORES
## Incorpora FOR UPDATE/SHARE, realiza una actualización en el cursor y comprueba que ahora el cursor es sensitive.
```sql
CREATE OR REPLACE PROCEDURE actualizarCursor (INOUT cursor refcursor)
LANGUAGE plpgsql
AS $procedure$
DECLARE
    registro record;
BEGIN
    OPEN cursor FOR SELECT * FROM empleados FOR UPDATE;
    LOOP
        FETCH cursor INTO registro;
        EXIT WHEN NOT FOUND;
        UPDATE empleados SET salario = salario + 100 WHERE CURRENT OF cursor;
    END LOOP;
END;
$procedure$;
```

## Piensa en un caso en el que sea necesario utilizar un cursor. Intenta programarlo.
```sql
CREATE OR REPLACE PROCEDURE salaDisponible (INOUT cursor refcursor)
LANGUAGE plpgsql
AS $procedure$
DECLARE
    registro record;
BEGIN
    OPEN cursor FOR SELECT * FROM salas WHERE ocupada = false;
    LOOP
        FETCH cursor INTO registro;
        EXIT WHEN NOT FOUND;
        UPDATE salas SET ocupada = true WHERE CURRENT OF cursor;
    END LOOP;
END;
```

## Investiga si existe algún tipo de estructura de bucle alternativa que permita el recorrido de un cursor.

Existen varias opciones, entre ellas WHILE y FOR. Ahora pondre unos ejemplos para que queden explicarlo de mejor manera
- WHILE:
```sql
CREATE OR REPLACE PROCEDURE salaDisponible (INOUT cursor refcursor)
LANGUAGE plpgsql
AS $procedure$
DECLARE
    registro record;
BEGIN
    OPEN cursor FOR SELECT * FROM salas WHERE ocupada = false;
    FETCH cursor INTO registro;
    WHILE FOUND LOOP
        UPDATE salas SET ocupada = true WHERE CURRENT OF cursor;
        FETCH cursor INTO registro;
    END LOOP;
END;
$procedure$;
```
- FOR:
```sql
CREATE OR REPLACE PROCEDURE salaDisponible()
LANGUAGE plpgsql
AS $procedure$
DECLARE
    registro record;
BEGIN
    FOR registro IN SELECT * FROM salas WHERE ocupada = false
    LOOP
        UPDATE salas SET ocupada = true WHERE salas.id = registro.id;
    END LOOP;
END;
$procedure$;
```

En ambos casos estaran funcionando hasta que no haya mas registros en la tabla en la que la sala este ocupada.