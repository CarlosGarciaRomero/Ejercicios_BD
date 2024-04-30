# EJERCICIOS FUNCIONES
## SUMA Y DIVISION
1. Suma
```postgresql
CREATE OR REPLACE FUNCTION Suma (num1 INT, num2 INT) RETURNS INT
AS $$
BEGIN
    RETURN num1 + num2;
END;
$$ LANGUAGE plpgsql;
```

2. Division
```postgresql
CREATE OR REPLACE FUNCTION Division (num1 float, num2 float) RETURNS float
AS $function$
BEGIN
    RETURN num1 / num2;
END;
$function$ 
LANGUAGE plpgsql;
```

## DEVOLVER LISTA DE FACTURAS ENTRE DOS FECHAS

```postgresql

    CREATE OR REPLACE FUNCTION FacturasEntreFechas (fecha1 DATE, fecha2 DATE)
    RETURNS TABLE (id_factura INT, fecha DATE, total DECIMAL)
    AS $function$
    BEGIN
        RETURN QUERY
        SELECT id_factura, fecha, total
        FROM factura
        WHERE fecha BETWEEN fecha1 AND fecha2;
    END;
    $function$
    LANGUAGE plpgsql;
```

## Función llamada Login que indique si un usuario es válido tras introducir su id y su contraseña.

```postgresql
CREATE OR REPLACE FUNCTION Login (id_usu INT, contrasena VARCHAR(10))
RETURNS BOOLEAN
AS $function$
DECLARE
    contrasena_usuario VARCHAR(10);
BEGIN
    SELECT contrasena INTO contrasena_usuario
    FROM usuario u
    WHERE u_id = usu;

    IF contrasena_usuario = contrasena THEN
        RETURN TRUE;
    ELSE
        RETURN FALSE;
    END IF;
END;
$function$
LANGUAGE plpgsql;
```

## Función que calcule la letra del DNI

```postgresql
CREATE OR REPLACE FUNCTION DNI (dni INT)
RETURNS CHAR
AS $function$
DECLARE
    letras CHAR[] := {'T', 'R', 'W', 'A', 'G', 'M', 'Y', 'F', 'P', 'D', 'X', 'B', 'N', 'J', 'Z', 'S', 'Q', 'V', 'H', 'L', 'C', 'K', 'E'};
    letra CHAR;
BEGIN
BEGIN
    letra := letras[dni % 23+1];
    RETURN letra;
END;
$function$
LANGUAGE plpgsql;
```

## Una función que devuelva la concatenación de 2 cadenas que se le pasarán como parámetro.

```postgresql
CREATE OR REPLACE FUNCTION Concatenar (cadena1 VARCHAR(10), cadena2 VARCHAR(10))
RETURNS VARCHAR(20)
AS $function$
BEGIN
    RETURN cadena1 || cadena2;
END;
$function$
LANGUAGE plpgsql;
```
