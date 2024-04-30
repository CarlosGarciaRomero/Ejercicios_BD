# EJERCICIO 6

## Parte a.
Tanto los procedimientos y las funcionesse tratan de codigos conjuntos para evitarnos el repetir codigo y con esto haciendo que sea más facil de leer y conseguimos una mejor eficiencia en el codigo y control de errores.

```sql
CREATE OR REPLACE
FUNCTION creartabla_prueba()
RETURNS VOID AS 
$function$
BEGIN
    CREATE TABLE prueba_ud6 (
        id SERIAL PRIMARY KEY,
        nombre VARCHAR(27),
        edad integer
    );
END;
$function$
LANGUAGE plpgsql
;
``` 
## Parte b.

Opcion 1:
```sql
CREATE OR REPLACE
FUNCTION creartabla_prueba(nombre VARCHAR)
RETURNS VOID AS
$function$
BEGIN
    CREATE TABLE $1 (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(27),
    edad integer
    );
END;
$function$
LANGUAGE plpgsql
;
```
Opcion 2:
```sql
CREATE OR REPLACE
FUNCTION creartabla_prueba(nombre VARCHAR)
RETURNS VOID AS
$function$
BEGIN
    EXECUTE format('
    CREATE TABLE %I (
        id SERIAL PRIMARY KEY,
        nombre VARCHAR(27),
        edad INTEGER
    );', nombre);
END;
$function$
LANGUAGE plpgsql
;
```

## Parte c.

- Funciones:
  
    Permiten el uso de parámetros de entrada, tener la posibilidad de realizar operaciones y ademas devolver los resultados dados por la operaciones hechas

    - Procedimientos:

    Si bien es cierto que son similares a las funciones, estos se almacenan en la propia base de datos ya compilados permitiendo su uso reiteradamente. Ademas de eso no devuelve un valor si no que pueden llegar a realizar cambios en la propia base de datos

## Parte d.

1. Un procedimiento que devuelva el stock de un determinado artículo

```sql
CREATE OR REPLACE PROCEDURE stock(IN p_id INT, INOUT p_stock INT) 
AS $procedure$
BEGIN
    SELECT stock INTO p_stock 
    FROM inventario 
    WHERE id = p_id;

END;
$procedure$ 
LANGUAGE plpgsql;
```

2. Un procedimiento que devuelva el total facturado entre 2 fechas.
```sql
CREATE OR REPLACE PROCEDURE total_facturado(IN inicio DATE, IN fin, INOUT total NUMERIC)
AS $procedure$
BEGIN
    SELECT SUM(f.total) INTO total
    FROM factura f
    WHERE f.fecha_factura BETWEEN inicio AND fin;
END;
$procedure$
LANGUAGE plpgsql;
```

3. Un procedimiento de tu invención que creas que te podría venir bien para futuros usos
```sql
CREATE OR REPLACE PROCEDURE act_precios(IN p_aumento NUMERIC)
AS $procedure$
BEGIN
    UPDATE productos 
    SET precio = precio * p_aumento;
END;
$procedure$
LANGUAGE plpgsql;
```