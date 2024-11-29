## INSERTAR INFORMACION DE LAS TABLAS

```sql


INSERT INTO Clientes (nombre, email) VALUES ('Juan Perez', 'juan.perez@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Maria Gomez', 'maria.gomez@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Carlos Ruiz', 'carlos.ruiz@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Ana Lopez', 'ana.lopez@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Luis Fernandez', 'luis.fernandez@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Sofia Ramirez', 'sofia.ramirez@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Diego Torres', 'diego.torres@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Laura Mendoza', 'laura.mendoza@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Pedro Castillo', 'pedro.castillo@email.com');
INSERT INTO Clientes (nombre, email) VALUES ('Lucia Morales', 'lucia.morales@email.com');


INSERT INTO Proveedores (nombre, contacto, telefono, direccion) VALUES ('Proveedor Uno', 'Juan Garcia', '123-456-7890', 'Calle 1, Ciudad A');
INSERT INTO Proveedores (nombre, contacto, telefono, direccion) VALUES ('Proveedor Dos', 'Maria Fernandez', '234-567-8901', 'Avenida 2, Ciudad B');
INSERT INTO Proveedores (nombre, contacto, telefono, direccion) VALUES ('Proveedor Tres', 'Carlos Martinez', '345-678-9012', 'Boulevard 3, Ciudad C');
INSERT INTO Proveedores (nombre, contacto, telefono, direccion) VALUES ('Proveedor Cuatro', 'Ana Rios', '456-789-0123', 'Calle 4, Ciudad D');


INSERT INTO TiposProductos (tipo_nombre, descripcion) VALUES ('Electrónica', 'Productos electrónicos');
INSERT INTO TiposProductos (tipo_nombre, descripcion) VALUES ('Ropa', 'Ropa de moda');
INSERT INTO TiposProductos (tipo_nombre, descripcion) VALUES ('Alimentos', 'Productos alimenticios');
INSERT INTO TiposProductos (tipo_nombre, descripcion) VALUES ('Juguetes', 'Juguetes para niños de todas las edades');
INSERT INTO TiposProductos (tipo_nombre, descripcion)
VALUES
('Herramientas', 'la mejor calidad en herramientas');


INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Televisor', 300.00, 1, 1);
INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Laptop', 800.00, 1, 1);
INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Camiseta', 20.00, 2, 2);
INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Pantalón', 25.00, 2, 2);
INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Pan', 1.50, 3, 3);
INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Queso', 3.50, 3, 3);
INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Carro de juguete', 15.00, 4, 4);
INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES ('Muñeca', 12.00, 4, 4),
('Martillo', 25.50, 4, 5),    
('Destornillador', 15.30, 4, 5),  
('Alicate', 20.75, 4, 5),      
('Llave inglesa', 30.00, 4, 5),
('Sierra', 45.80, 4, 5),
('Tablet', 200.00, 1, 1),      
('Zapatillas', 50.00, 2, 2),   
('Mermelada', 5.00, 3, 3),     
('Puzzle 3D', 20.00, 4, 4);
       


INSERT INTO puestos (nombre) VALUES 
('Vendedor'),
('Administrador'),
('Gerente');



INSERT INTO Empleados (nombre, puesto, salario, idpuesto, fecha_contratacion) VALUES 
('Luis García', 'Vendedor', 2200.00, 1, '2023-07-15'),
('Pedro Martínez', 'Vendedor', 2300.00, 1, '2023-09-05'),
('Lucía Rodríguez', 'Vendedor', 2100.00, 1, '2023-08-20'),
('María López', 'Vendedor', 2500.00, 1, '2023-10-10'),
('Javier Sánchez', 'Vendedor', 2400.00, 1, '2023-11-01');

INSERT INTO Pedidos (cliente_id, idempleado, fecha, total) VALUES 
(1, 1, '2024-11-15', 500.00),  
(2, 2, '2024-11-16', 250.00),  
(3, 3, '2024-11-17', 300.00),  
(4, 1, '2024-11-18', 400.00),  
(5, 2, '2024-11-19', 150.00),  
(6, 1, '2024-11-20', 600.00),  
(7, 3, '2024-11-21', 350.00),  
(8, 1, '2024-11-22', 450.00);  




INSERT INTO UbicacionCliente (cliente_id, direccion, ciudad, estado, codigo_postal, pais) VALUES (1, 'Calle 1, Ciudad A', 'Ciudad A', 'Estado A', '12345', 'Pais A');
INSERT INTO UbicacionCliente (cliente_id, direccion, ciudad, estado, codigo_postal, pais) VALUES (2, 'Avenida 2, Ciudad B', 'Ciudad B', 'Estado B', '23456', 'Pais B');
INSERT INTO UbicacionCliente (cliente_id, direccion, ciudad, estado, codigo_postal, pais) VALUES (3, 'Boulevard 3, Ciudad C', 'Ciudad C', 'Estado C', '34567', 'Pais C');
INSERT INTO UbicacionCliente (cliente_id, direccion, ciudad, estado, codigo_postal, pais) VALUES (4, 'Calle 4, Ciudad D', 'Ciudad D', 'Estado D', '45678', 'Pais D');
INSERT INTO UbicacionCliente (cliente_id, direccion, ciudad, estado, codigo_postal, pais) VALUES (5, 'Calle 1, Ciudad A', 'Ciudad A', 'Estado A', '12345', 'Pais A');

INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad)
VALUES (1, 15, 10);  
INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad)
VALUES (2, 16, 8);   
INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad)
VALUES (3, 17, 12);  
INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad)
VALUES (4, 8, 15); 
INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad)
VALUES (5,9, 20); 



INSERT INTO inventarioProductos (idproductos) 
VALUES 
(1), 
(2),  
(3),  
(4),  
(5),  
(6),  
(7),  
(8),  
(9),  
(10), 
(11), 
(12), 
(13), 
(14), 
(15), 
(16), 
(17); 


```
