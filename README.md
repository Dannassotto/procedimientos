# procedimientos

## Insert
```sql
DELIMITER $$

CREATE PROCEDURE insertar_producto(
    IN producto_id INT,
    IN nuevo_producto VARCHAR(40),
    IN nueva_cantidad INT,
    IN nuevo_precio DECIMAL(19,2),
    IN nueva_marca VARCHAR(20),
    IN nuevo_estado TINYINT)
BEGIN
DECLARE mensaje VARCHAR(100);
    INSERT INTO productos (id , producto, cantidad, precio, marca, estado )
    VALUES (producto_id, nuevo_producto, nueva_cantidad, nuevo_precio, nueva_marca, nuevo_estado );
IF ROW_COUNT() > 0 THEN
        SET mensaje = 'Registro insertado correctamente';
    ELSE
        SET mensaje = 'Error al insertar el registro';
    END IF;
    SELECT mensaje AS Mensaje;
END $$
DELIMITER ;
```

## Buscar

```sql
DELIMITER $$
CREATE PROCEDURE buscar_producto(
    IN producto_id INT,
    IN nombre_producto VARCHAR(40)
)
BEGIN
    IF producto_id IS NOT NULL THEN
        SELECT * FROM productos WHERE id = producto_id;
    ELSEIF nombre_producto IS NOT NULL THEN
        SELECT * FROM productos WHERE producto = nombre_producto;
    ELSE
        SELECT 'Por favor ingrese un id o un nombre de producto' AS Mensaje;
    END IF;
END $$
DELIMITER ;

```
## insert_verify

```sql
DELIMITER $$
CREATE PROCEDURE insertar_producto_verify(
    IN producto_id INT,
    IN nuevo_producto VARCHAR(40),
    IN nueva_cantidad INT,
    IN nuevo_precio DECIMAL(19,2),
    IN nueva_marca VARCHAR(20),
    IN nuevo_estado TINYINT
)
BEGIN
    DECLARE mensaje VARCHAR(100);

    IF EXISTS (SELECT 1 FROM productos WHERE id = producto_id) THEN
        SET mensaje = 'Error: El producto con este ID ya existe';
    ELSE
        INSERT INTO productos (id, producto, cantidad, precio, marca, estado)
        VALUES (producto_id, nuevo_producto, nueva_cantidad, nuevo_precio, nueva_marca, nuevo_estado);
        
        IF ROW_COUNT() > 0 THEN
            SET mensaje = 'Registro insertado correctamente';
        ELSE
            SET mensaje = 'Error al insertar el registro';
        END IF;
    END IF;

    SELECT mensaje AS Mensaje;
END $$
DELIMITER ;

```




