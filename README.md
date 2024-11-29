# Taller-MySQL

## CREACION BASE DE DATOS Y NORMALIZACION
```sql
CREATE DATABASE vtaszfs;
USE vtaszfs;
```
# TABLA Clientes
```sql
CREATE TABLE Clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    email VARCHAR(100) UNIQUE
);
```
 # TABLA UbicacionCliente
 ```sql
CREATE TABLE UbicacionCliente (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT,
    direccion VARCHAR(255),
    ciudad VARCHAR(100),
    estado VARCHAR(50),
    codigo_postal VARCHAR(10),
    pais VARCHAR(50),
    FOREIGN KEY (cliente_id) REFERENCES Clientes(id)
);
```
# TABLA Proveedores
```sql
CREATE TABLE Proveedores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    contacto VARCHAR(100),
    telefono VARCHAR(20),
    direccion VARCHAR(255)
);
```
# TABLA TiposProductos
```sql

CREATE TABLE TiposProductos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    tipo_nombre VARCHAR(100),
    descripcion TEXT
);
```
# TABLA Productos
```sql
CREATE TABLE Productos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    precio DECIMAL(10, 2),
    proveedor_id INT,
    tipo_id INT,
    FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id),
    FOREIGN KEY (tipo_id) REFERENCES TiposProductos(id)
);
```

# TABLA puestos
```sql
CREATE TABLE puestos(
id INT PRIMARY KEY AUTO_INCREMENT,
nombre VARCHAR (80)
);
```
```sql

CREATE TABLE Empleados (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    puesto VARCHAR(50),
    salario DECIMAL(10, 2),
    idpuesto INT,
    fecha_contratacion DATE,
    FOREIGN KEY (idpuesto) REFERENCES puestos(id)

);
```

```sql

CREATE TABLE Pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT,
    idempleado INT,
    fecha DATE,
    total DECIMAL(10, 2),
    FOREIGN KEY (cliente_id) REFERENCES Clientes(id),
    FOREIGN KEY (idempleado) REFERENCES Empleados(id)

);
```

```sql

CREATE TABLE DetallesPedido (
    id INT PRIMARY KEY AUTO_INCREMENT,
    pedido_id INT,
    producto_id INT,
    cantidad INT,
    precio DECIMAL(10, 2),
    FOREIGN KEY (pedido_id) REFERENCES Pedidos(id),
    FOREIGN KEY (producto_id) REFERENCES Productos(id)
);
```

```sql

CREATE TABLE historialpedidos(
id INT PRIMARY KEY AUTO_INCREMENT,
id_pedidos INT(11),
fechamodificacion DATE,
FOREIGN KEY (id_pedidos) REFERENCES pedidos(id)
);
```

```sql

CREATE TABLE ProveedorContactos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    proveedor_id INT,
    contacto VARCHAR(100),
    telefono VARCHAR(20),
    FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id)
);
```




```sql

CREATE TABLE datosempleado(
id INT PRIMARY KEY AUTO_INCREMENT,
salario DECIMAL(10, 2),
fecha_contratacion DATE,
idempleados INT,
salario_anterior DECIMAL(10, 2),
fecha_modificacion DATE,
FOREIGN KEY (idempleados) REFERENCES empleados(id)
);
```

```sql


CREATE TABLE inventarioProductos(
id INT PRIMARY KEY AUTO_INCREMENT,
idproductos INT,
cantidad INT,
FOREIGN KEY (idproductos) REFERENCES productos(id)
);
```

```sql

CREATE TABLE pais(
id INT PRIMARY KEY AUTO_INCREMENT,
nombre VARCHAR(80)
);
```

```sql

CREATE TABLE region(
id INT PRIMARY KEY AUTO_INCREMENT,
nombre VARCHAR(80)
);
```


```sql

CREATE TABLE ciudad (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(80)
);
```
```sql

CREATE TABLE ubicacion(
id INT PRIMARY KEY AUTO_INCREMENT,
idpais INT,
dirección VARCHAR(80),
idregion INT,
idciudad INT,
FOREIGN KEY (idpais) REFERENCES pais(id),
FOREIGN KEY (idciudad) REFERENCES ciudad(id),
FOREIGN KEY (idregion) REFERENCES region(id)
);
```

```sql

CREATE TABLE sucursal (
id INT PRIMARY KEY AUTO_INCREMENT,
nombre VARCHAR(80),
idubicacion INT,
idciudad INT,
idproveedores INT,
FOREIGN KEY (idproveedores) REFERENCES proveedores(id),
FOREIGN KEY (idubicacion) REFERENCES ubicacion(id),
FOREIGN KEY (idciudad) REFERENCES ciudad(id)
);
```
```sql

CREATE TABLE contactocliente(
id INT PRIMARY KEY AUTO_INCREMENT,
idcliente INT,
teléfono VARCHAR(20),
FOREIGN KEY (idcliente) REFERENCES clientes(id)
);
```
```sql

CREATE TABLE producto_ingreso (
id INT PRIMARY KEY AUTO_INCREMENT,
idproductos INT,
descripción VARCHAR(50),
faltantes VARCHAR(80),
FOREIGN KEY (idproductos) REFERENCES productos(id)
);
```
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
 CREATE TRIGGER SalarioModificado
     AFTER UPDATE ON Empleados
     FOR EACH ROW
     BEGIN
         IF OLD.salario <> NEW.salario THEN
             INSERT INTO datosempleado (idempleados, salario_anterior, salario, fecha_modificacion)
             VALUES (NEW.id, OLD.salario, NEW.salario, CURDATE());
      END IF;  
     END $$

DELIMITER ;


UPDATE Empleados
     SET salario = 3600.00
     WHERE id = 1;

SELECT * FROM datosempleado;

```

```sql

DELIMITER $$

CREATE TRIGGER evitar_borrado_productos_con_pedidos
BEFORE DELETE ON productos
FOR EACH ROW
BEGIN
    IF EXISTS (
        SELECT 1 FROM DetallesPedido WHERE producto_id = OLD.id
    ) THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'No se puede borrar un producto con pedidos activos.';
   END IF;
  END $$

DELIMITER ;


DELETE FROM productos WHERE id = 15;

```

```sql
DELIMITER $$

CREATE TRIGGER registro_historial_pedido
AFTER UPDATE ON Pedidos
FOR EACH ROW
BEGIN
    INSERT INTO historialpedidos (id_pedidos, fechamodificacion)
    VALUES (NEW.id, NOW());
END $$

DELIMITER ;



UPDATE Pedidos
SET total = 200.00
WHERE id = 1;


SELECT * FROM historialpedidos;

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
