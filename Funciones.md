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

```sql

DELIMITER $$

CREATE FUNCTION CalcularDescuento(tipo_id INT, precio DECIMAL(10,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE descuento DECIMAL(10,2);
    
    IF tipo_id = 1 THEN  
        SET descuento = 0.10; 
    ELSE
        SET descuento = 0.00; 
    END IF;
    RETURN precio * (1 - descuento); 
END$$

DELIMITER ;

SELECT p.nombre, p.precio, CalcularDescuento(p.tipo_id, p.precio) AS precio_con_descuento
FROM productos p;

```


```sql
DELIMITER $$


CREATE FUNCTION CalcularImpuesto(precio DECIMAL(10, 2))
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    RETURN precio * 1.15;  
END $$

DELIMITER ;

SELECT nombre, precio, CalcularImpuesto(precio) AS precio_final
FROM productos;



DELIMITER $$


```

```sql
CREATE FUNCTION TotalPedidosCliente(cliente_id INT)
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    DECLARE total DECIMAL(10, 2);
    SELECT SUM(p.total) INTO total
    FROM pedidos p
    WHERE p.cliente_id = cliente_id;
    RETURN total;
END $$

DELIMITER ;


SELECT c.nombre, TotalPedidosCliente(c.id) AS total_pedidos
FROM clientes c
HAVING total_pedidos > 1000;


```



```sql

DELIMITER $$

CREATE FUNCTION SalarioAnual(salario_mensual DECIMAL(10, 2))
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    RETURN salario_mensual * 12;
END$$

DELIMITER ;


SELECT nombre, SalarioAnual(salario) AS salario_anual
FROM empleados
HAVING salario_anual > 50000;


```



```sql
DELIMITER $$

CREATE FUNCTION Bonificacion(salario DECIMAL(10, 2))
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    RETURN salario * 0.10;  

END $$

DELIMITER ;

SELECT nombre, salario, Bonificacion(salario) AS bonificacion,
       (salario + Bonificacion(salario)) AS salario_ajustado
FROM empleados;

```


```sql

DELIMITER $$


CREATE FUNCTION DiasDesdeUltimoPedido(cliente_id INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE dias INT;
    SELECT DATEDIFF(CURDATE(), MAX(fecha)) INTO dias
    FROM pedidos
    WHERE cliente_id = cliente_id;
    RETURN dias;
END $$

DELIMITER ;


SELECT c.nombre, DiasDesdeUltimoPedido(c.id) AS dias_ultimo_pedido
FROM clientes c
WHERE DiasDesdeUltimoPedido(c.id) <= 30;


```

```sql

DELIMITER $$

CREATE FUNCTION TotalInventarioProducto(producto_id INT)
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    DECLARE total DECIMAL(10, 2);

    -- Calcular el total del inventario (cantidad * precio)
    SELECT SUM(ip.cantidad * p.precio) INTO total
    FROM inventarioproductos ip
    JOIN productos p ON ip.id = p.id
    WHERE p.id = producto_id;

    -- Devolver el total calculado
    RETURN total;
END$$

DELIMITER ;


SELECT p.nombre, TotalInventarioProducto(p.id) AS total_inventario
FROM productos p
HAVING total_inventario > 500;


```

```sql


CREATE TABLE HistorialPrecios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    producto_id INT,
    precio_anterior DECIMAL(10, 2),
    precio_nuevo DECIMAL(10, 2),
    fecha_cambio DATETIME
);

DELIMITER $$
CREATE TRIGGER RegistroCambioPrecio
AFTER UPDATE ON productos
FOR EACH ROW
BEGIN
    IF OLD.precio != NEW.precio THEN
        INSERT INTO HistorialPrecios (producto_id, precio_anterior, precio_nuevo, fecha_cambio)
        VALUES (OLD.id, OLD.precio, NEW.precio, NOW());
    END IF;
END$$

DELIMITER ;


```

