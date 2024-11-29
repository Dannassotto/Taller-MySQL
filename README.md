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

