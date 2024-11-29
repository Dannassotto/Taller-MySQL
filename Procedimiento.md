## PROCEDIMIENTOS

```sql

DELIMITER $$

CREATE PROCEDURE ActualizarPrecioProductosProveedor(
    IN p_proveedor_id INT,     
    IN nuevo_precio DECIMAL(10,2)  
)
BEGIN
    UPDATE Productos
    SET precio = nuevo_precio
    WHERE proveedor_id = p_proveedor_id; 
END $$

DELIMITER ;

CALL ActualizarPrecioProductosProveedor(1, 150.00);


SELECT * FROM Productos WHERE proveedor_id = 1;

```

```sql
DELIMITER $$

CREATE PROCEDURE ObtenerDireccionCliente(
    IN p_cliente_id INT   -- Parámetro de entrada: ID del cliente
)
BEGIN
    -- Seleccionar la dirección del cliente dado su ID
    SELECT direccion
    FROM UbicacionCliente
    WHERE cliente_id = p_cliente_id;
END $$

DELIMITER ;

CALL ObtenerDireccionCliente(1);

```

```sql
DELIMITER $$

CREATE PROCEDURE RegistrarPedido(
    IN cliente_id INT,
    IN empleado_id INT,
    IN fecha DATE,
    IN total DECIMAL(10,2),
    IN productos JSON
)
BEGIN
    DECLARE pedido_id INT;
    DECLARE i INT DEFAULT 0;
    DECLARE producto_id INT;
    DECLARE cantidad INT;
    DECLARE num_productos INT;

    INSERT INTO Pedidos (cliente_id, idempleado, fecha, total)
    VALUES (cliente_id, empleado_id, fecha, total);

    SET pedido_id = LAST_INSERT_ID();

    SET num_productos = JSON_LENGTH(productos);

    WHILE i < num_productos DO
        SET producto_id = JSON_UNQUOTE(JSON_EXTRACT(productos, CONCAT('$[', i, '].producto_id')));
        SET cantidad = JSON_UNQUOTE(JSON_EXTRACT(productos, CONCAT('$[', i, '].cantidad')));

        INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad)
        VALUES (pedido_id, producto_id, cantidad);

        SET i = i + 1;
    END WHILE;

END $$

DELIMITER ;

CALL RegistrarPedido(
    1,  
    2,  
    '2024-11-19',  
    100.00,  
    '[{"producto_id": 10, "cantidad": 5}, {"producto_id": 15, "cantidad": 3}]'  
);

SELECT * FROM pedidos;

```
```sql
DELIMITER $$

CREATE PROCEDURE CalcularTotalVentasCliente(
    IN cliente_id INT
)
BEGIN
    SELECT SUM(total) AS TotalVentas
    FROM Pedidos
    WHERE cliente_id = cliente_id;
END $$

DELIMITER ;


CALL CalcularTotalVentasCliente(1);

```

```sql
DELIMITER $$

CREATE PROCEDURE ObtenerEmpleadosPorPuesto(
    IN puesto_id INT
)
BEGIN
    SELECT nombre, puesto
    FROM Empleados
    WHERE idpuesto = puesto_id;
END $$

DELIMITER ;

CALL ObtenerEmpleadosPorPuesto(1);

```

```sql
DELIMITER $$

CREATE PROCEDURE ActualizarSalarioPorPuesto(
    IN puesto_id INT,
    IN nuevo_salario DECIMAL(10,2)
)
BEGIN
    UPDATE Empleados
    SET salario = nuevo_salario
    WHERE idpuesto = puesto_id;
END $$

DELIMITER ;


CALL ActualizarSalarioPorPuesto(1, 500);

SELECT * FROM empleados;

```

```sql
DELIMITER $$

CREATE PROCEDURE ListarPedidosEntreFechas(
    IN fecha_inicio DATE,
    IN fecha_fin DATE
)
BEGIN
    SELECT id, cliente_id, idempleado, fecha, total
    FROM Pedidos
    WHERE fecha BETWEEN fecha_inicio AND fecha_fin;
END $$

DELIMITER ;


CALL ListarPedidosEntreFechas('2024-01-01', '2024-12-31');

```

```sql
DELIMITER $$

CREATE PROCEDURE AplicarDescuentoCategoria(
    IN categoria_id INT,
    IN descuento DECIMAL(5,2)
)
BEGIN
    UPDATE Productos
    SET precio = precio - (precio * descuento / 100)
    WHERE tipo_id = categoria_id;
END $$

DELIMITER ;

CALL AplicarDescuentoCategoria(3, 10);

SELECT * FROM Productos WHERE tipo_id = 3;

```

```sql
DELIMITER $$

CREATE PROCEDURE ListarProveedoresPorTipo(
    IN tipo_producto_id INT
)
BEGIN
    SELECT pr.nombre AS Proveedor, pr.contacto, pr.telefono
    FROM Proveedores pr
    JOIN Productos p ON pr.id = p.proveedor_id
    WHERE p.tipo_id = tipo_producto_id
    GROUP BY pr.id;
END $$

DELIMITER ;

CALL ListarProveedoresPorTipo (1);

```

```sql

DELIMITER $$

CREATE PROCEDURE PedidoMayorValor()
BEGIN
    SELECT id, cliente_id, idempleado, fecha, total
    FROM Pedidos
    ORDER BY total DESC
    LIMIT 1;
END $$

DELIMITER ;


CALL PedidoMayorValor();

```
