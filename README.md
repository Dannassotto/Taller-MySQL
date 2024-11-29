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
