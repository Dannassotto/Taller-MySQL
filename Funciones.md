```sql

DELIMITER $$

CREATE FUNCTION DíasTranscurridos(fecha_inicio DATE) 
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN DATEDIFF(CURDATE(), fecha_inicio);
END $$

DELIMITER ;

SELECT DíasTranscurridos('2025-01-01');

```
```sql

DELIMITER $$

CREATE FUNCTION TotalConImpuesto(monto DECIMAL(10,2), impuesto DECIMAL(5,2)) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN monto + (monto * impuesto / 100);
END $$

DELIMITER ;


SELECT TotalConImpuesto(100, 15);

```

```sql
DELIMITER $$

CREATE FUNCTION TotalPedidosCliente(cliente_id INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE total INT;
    
    SELECT COUNT(*) INTO total
    FROM Pedidos
    WHERE Pedidos.cliente_id = cliente_id;
    
    
    IF total IS NULL THEN
        SET total = 0;
    END IF;

    
    RETURN total;
END $$

DELIMITER ;


SELECT TotalPedidosCliente(2);
SELECT TotalPedidosCliente(1);
SELECT TotalPedidosCliente(6);
```

```sql
DELIMITER $$

CREATE FUNCTION AplicarDescuento(p_producto_id INT, p_descuento DECIMAL(5, 2)) 
RETURNS DECIMAL(10, 2)  
DETERMINISTIC  
BEGIN
    DECLARE v_precio_actual DECIMAL(10, 2);  
    DECLARE v_nuevo_precio DECIMAL(10, 2);   

    SELECT precio INTO v_precio_actual
    FROM Productos
    WHERE id = p_producto_id;

    SET v_nuevo_precio = v_precio_actual - (v_precio_actual * p_descuento / 100);

    UPDATE Productos
    SET precio = v_nuevo_precio
    WHERE id = p_producto_id;


    RETURN v_nuevo_precio;
END $$

DELIMITER ;

SELECT AplicarDescuento(1, 10);

```
```sql

DELIMITER $$

CREATE FUNCTION TieneDireccion(cliente_id INT) 
RETURNS BOOLEAN
DETERMINISTIC
BEGIN
    DECLARE direccion_existente INT;
    SELECT COUNT(*) INTO direccion_existente FROM UbicacionCliente WHERE cliente_id = cliente_id;
    RETURN direccion_existente > 0;
END $$

DELIMITER ;

SELECT TieneDireccion(1);

```
```sql


DELIMITER $$

CREATE FUNCTION ObtenerSalarioAnual(p_empleado_id INT)
RETURNS DECIMAL(10, 2)  
DETERMINISTIC  
BEGIN
    DECLARE v_salario_mensual DECIMAL(10, 2);  
    DECLARE v_salario_anual DECIMAL(10, 2);    

    SELECT salario INTO v_salario_mensual
    FROM Empleados
    WHERE id = p_empleado_id;

    SET v_salario_anual = v_salario_mensual * 12;

    RETURN v_salario_anual;
END $$

DELIMITER ;


SELECT ObtenerSalarioAnual(5);

```

```sql
DELIMITER $$

CREATE FUNCTION TotalVentasProducto(categoria_id INT) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE total DECIMAL(10,2);
    SELECT SUM(p.total) INTO total
    FROM Pedidos p
    JOIN DetallesPedido dp ON p.id = dp.pedido_id
    JOIN Productos prod ON dp.producto_id = prod.id
    WHERE prod.tipo_id = categoria_id;
    RETURN total;
END $$

DELIMITER ;


SELECT TotalVentasProducto(1);

```

```sql


DELIMITER $$

CREATE FUNCTION NombreCliente(cliente_id INT) 
RETURNS VARCHAR(100)
DETERMINISTIC
BEGIN
    DECLARE nombre_cliente VARCHAR(100);
    SELECT nombre INTO nombre_cliente FROM Clientes WHERE id = cliente_id;
    RETURN nombre_cliente;
END $$

DELIMITER ;


SELECT NombreCliente(1);

```

```sql


DELIMITER $$

CREATE FUNCTION ObtenerTotalPedido(p_pedido_id INT)
RETURNS DECIMAL(10, 2)  
DETERMINISTIC  
BEGIN
    DECLARE v_total DECIMAL(10, 2);  

    SELECT total INTO v_total
    FROM Pedidos
    WHERE id = p_pedido_id;

    RETURN v_total;
END $$

DELIMITER ;


SELECT ObtenerTotalPedido(3);

```

```sql

DELIMITER $$

CREATE FUNCTION ProductoEnInventario(producto_id INT) 
RETURNS VARCHAR(50)
DETERMINISTIC
BEGIN
    DECLARE resultado VARCHAR(50);

    IF EXISTS (SELECT 1 FROM inventarioProductos WHERE idproductos = producto_id) THEN
        SET resultado = 'El producto está en inventario';
    ELSE
        SET resultado = 'El producto no está en inventario';
    END IF;

    RETURN resultado;
END $$

DELIMITER ;


SELECT ProductoEnInventario(1);  

```
