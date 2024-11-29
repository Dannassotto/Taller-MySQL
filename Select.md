```sql

SELECT 
    Pedidos.id AS Pedido_ID, 
    Clientes.nombre AS Cliente, 
    Pedidos.fecha AS Fecha_Pedido, 
    Pedidos.total AS Total
FROM 
    Pedidos
INNER JOIN 
    Clientes ON Pedidos.cliente_id = Clientes.id;

```

```sql

SELECT 
    Productos.nombre AS Producto, 
    Proveedores.nombre AS Proveedor
FROM 
    Productos
INNER JOIN 
    Proveedores ON Productos.proveedor_id = Proveedores.id;
```

```sql

SELECT 
    Pedidos.id AS Pedido_ID, 
    Pedidos.fecha AS Fecha_Pedido, 
    Pedidos.total AS Total, 
    UbicacionCliente.direccion AS Direccion, 
    UbicacionCliente.ciudad AS Ciudad
FROM 
    Pedidos
LEFT JOIN 
    UbicacionCliente ON Pedidos.cliente_id = UbicacionCliente.cliente_id;
```

```sql


SELECT 
    Empleados.nombre AS Empleado, 
    Pedidos.id AS Pedido_ID, 
    Pedidos.fecha AS Fecha_Pedido
FROM 
    Empleados
LEFT JOIN 
    Pedidos ON Empleados.id = Pedidos.idempleado;

```

```sql

SELECT 
    TiposProductos.tipo_nombre AS Tipo, 
    Productos.nombre AS Producto
FROM 
    Productos
INNER JOIN 
    TiposProductos ON Productos.tipo_id = TiposProductos.id;
```

```sql

SELECT 
    Clientes.nombre AS Cliente, 
    COUNT(Pedidos.id) AS Numero_Pedidos
FROM 
    Clientes
LEFT JOIN 
    Pedidos ON Clientes.id = Pedidos.cliente_id
GROUP BY 
    Clientes.id;

```

```sql

SELECT 
    Pedidos.id AS Pedido_ID, 
    Empleados.nombre AS Empleado, 
    Pedidos.fecha AS Fecha_Pedido
FROM 
    Pedidos
INNER JOIN 
    Empleados ON Pedidos.idempleado = Empleados.id;

```
```sql


SELECT 
    Productos.id AS ProductoID,
    Productos.nombre AS NombreProducto,
    Productos.precio AS Precio,
    Productos.proveedor_id AS ProveedorID,
    Productos.tipo_id AS TipoProductoID
FROM 
    DetallesPedido
RIGHT JOIN 
    Productos ON DetallesPedido.producto_id = Productos.id
WHERE 
    DetallesPedido.producto_id IS NULL;
```

```sql

SELECT 
    Pedidos.id AS Pedido_ID, 
    Clientes.nombre AS Cliente, 
    Pedidos.total AS Total, 
    UbicacionCliente.direccion AS Direccion, 
    UbicacionCliente.ciudad AS Ciudad
FROM 
    Pedidos
INNER JOIN 
    Clientes ON Pedidos.cliente_id = Clientes.id
LEFT JOIN 
    UbicacionCliente ON Clientes.id = UbicacionCliente.cliente_id;

```
```sql

SELECT 
    Productos.nombre AS Producto, 
    Proveedores.nombre AS Proveedor, 
    TiposProductos.tipo_nombre AS Tipo
FROM 
    Productos
INNER JOIN 
    Proveedores ON Productos.proveedor_id = Proveedores.id
INNER JOIN 
    TiposProductos ON Productos.tipo_id = TiposProductos.id;

```

```sql

SELECT 
    nombre AS Producto, 
    precio 
FROM 
    Productos
WHERE 
    precio > 50;
```

```sql

SELECT 
    nombre AS Cliente, 
    email 
FROM 
    Clientes
WHERE 
    id IN (
        SELECT cliente_id 
        FROM UbicacionCliente 
        WHERE ciudad = 'Ciudad A'
    );
```

```sql

SELECT 
    nombre AS Empleado, 
    fecha_contratacion 
FROM 
    Empleados
WHERE 
    fecha_contratacion >= CURDATE() - INTERVAL 2 YEAR;

```

```sql

SELECT 
    Proveedores.nombre AS Proveedor
FROM 
    Proveedores
INNER JOIN 
    Productos ON Proveedores.id = Productos.proveedor_id
GROUP BY 
    Proveedores.id
HAVING 
    COUNT(Productos.id) > 5;

```

```sql

SELECT 
    Clientes.nombre AS Cliente
FROM 
    Clientes
LEFT JOIN 
    UbicacionCliente ON Clientes.id = UbicacionCliente.cliente_id
WHERE 
    UbicacionCliente.cliente_id IS NULL;
```

```sql

SELECT 
    Clientes.nombre AS Cliente, 
    SUM(Pedidos.total) AS Total_Ventas
FROM 
    Clientes
INNER JOIN 
    Pedidos ON Clientes.id = Pedidos.cliente_id
GROUP BY 
    Clientes.id;
```

```sql

SELECT 
    AVG(salario) AS Salario_Promedio 
FROM 
    Empleados;
```

```sql

SELECT 
    tipo_nombre AS Tipo_Producto
FROM 
    TiposProductos;

```
```sql


SELECT 
    nombre AS Producto, 
    precio 
FROM 
    Productos
ORDER BY 
    precio DESC
LIMIT 3;
```

```sql

SELECT 
    Clientes.nombre AS Cliente, 
    COUNT(Pedidos.id) AS Numero_Pedidos
FROM 
    Clientes
INNER JOIN 
    Pedidos ON Clientes.id = Pedidos.cliente_id
GROUP BY 
    Clientes.id
ORDER BY 
    Numero_Pedidos DESC
LIMIT 1;
```

```sql

SELECT 
    Pedidos.id AS Pedido_ID, 
    Pedidos.fecha AS Fecha_Pedido, 
    Pedidos.total AS Total_Pedido, 
    Clientes.nombre AS Cliente
FROM 
    Pedidos
INNER JOIN 
    Clientes ON Pedidos.cliente_id = Clientes.id;
```

```sql

SELECT 
    Pedidos.id AS Pedido_ID, 
    Clientes.nombre AS Cliente, 
    UbicacionCliente.direccion AS Direccion, 
    UbicacionCliente.ciudad AS Ciudad, 
    UbicacionCliente.estado AS Estado, 
    UbicacionCliente.codigo_postal AS Codigo_Postal, 
    UbicacionCliente.pais AS Pais
FROM 
    Pedidos
INNER JOIN 
    Clientes ON Pedidos.cliente_id = Clientes.id
INNER JOIN 
    UbicacionCliente ON Clientes.id = UbicacionCliente.cliente_id;

```

```sql

SELECT 
    Productos.nombre AS Producto, 
    Proveedores.nombre AS Proveedor, 
    TiposProductos.tipo_nombre AS Tipo_Producto
FROM 
    Productos
INNER JOIN 
    Proveedores ON Productos.proveedor_id = Proveedores.id
INNER JOIN 
    TiposProductos ON Productos.tipo_id = TiposProductos.id;

```
```sql


SELECT 
    Empleados.nombre AS Empleado, 
    Pedidos.id AS Pedido_ID, 
    Clientes.nombre AS Cliente
FROM 
    Empleados
INNER JOIN 
    Pedidos ON Empleados.id = Pedidos.id
INNER JOIN 
    Clientes ON Pedidos.cliente_id = Clientes.id
INNER JOIN 
    UbicacionCliente ON Clientes.id = UbicacionCliente.cliente_id
WHERE 
    UbicacionCliente.ciudad = 'Ciudad B';

```

```sql

SELECT 
    Productos.nombre AS Producto, 
    SUM(DetallesPedido.cantidad) AS Total_Vendido
FROM 
    DetallesPedido
INNER JOIN 
    Productos ON DetallesPedido.producto_id = Productos.id
GROUP BY 
    Productos.id
ORDER BY 
    Total_Vendido DESC
LIMIT 5;
```

```sql

SELECT 
    Clientes.nombre AS Cliente, 
    UbicacionCliente.ciudad AS Ciudad, 
    COUNT(Pedidos.id) AS Total_Pedidos
FROM 
    Clientes
INNER JOIN 
    Pedidos ON Clientes.id = Pedidos.cliente_id
INNER JOIN 
    UbicacionCliente ON Clientes.id = UbicacionCliente.cliente_id
GROUP BY 
    Clientes.id, UbicacionCliente.ciudad;
```


```sql

SELECT 
    Clientes.nombre AS Cliente,
    Proveedores.nombre AS Proveedor,
    UbicacionCliente.ciudad AS Ciudad
FROM 
    UbicacionCliente
INNER JOIN 
    Clientes ON UbicacionCliente.cliente_id = Clientes.id
INNER JOIN 
    Proveedores ON UbicacionCliente.ciudad = Proveedores.direccion
ORDER BY 
    UbicacionCliente.ciudad;
```

```sql

SELECT 
    TiposProductos.tipo_nombre AS Tipo_Producto, 
    SUM(DetallesPedido.cantidad * Productos.precio) AS Total_Ventas
FROM 
    DetallesPedido
INNER JOIN 
    Productos ON DetallesPedido.producto_id = Productos.id
INNER JOIN 
    TiposProductos ON Productos.tipo_id = TiposProductos.id
GROUP BY 
    TiposProductos.id;
```

```sql

SELECT
    Empleados.nombre AS Empleado,
    Empleados.id AS EmpleadoID,
    Proveedores.nombre AS Proveedor
FROM
    Empleados
INNER JOIN
    Pedidos ON Empleados.id = Pedidos.idempleado
INNER JOIN
    DetallesPedido ON Pedidos.id = DetallesPedido.pedido_id
INNER JOIN
    Productos ON DetallesPedido.producto_id = Productos.id
INNER JOIN
    Proveedores ON Productos.proveedor_id = Proveedores.id
WHERE
    Proveedores.nombre = 'Proveedor Uno';
```

```sql

SELECT 
    Proveedores.nombre AS Proveedor, 
    SUM(DetallesPedido.cantidad * Productos.precio) AS Ingreso_Total
FROM 
    DetallesPedido
INNER JOIN 
    Productos ON DetallesPedido.producto_id = Productos.id
INNER JOIN 
    Proveedores ON Productos.proveedor_id = Proveedores.id
GROUP BY 
    Proveedores.id;

```

```sql

SELECT 
    p.nombre AS Producto, 
    p.precio AS Precio, 
    p.tipo_id AS TipoProductoID
FROM 
    Productos p
WHERE 
    p.precio = (
        SELECT 
            MAX(precio)
        FROM 
            Productos
        WHERE 
            tipo_id = p.tipo_id
    );
```

```sql

SELECT 
    c.nombre AS Cliente,
    MAX(p.total) AS TotalMaximo
FROM 
    Clientes c
JOIN 
    Pedidos p ON c.id = p.cliente_id
GROUP BY 
    c.id
ORDER BY 
    TotalMaximo DESC
LIMIT 1;

```

```sql

SELECT 
    e.nombre AS Empleado,
    e.salario AS Salario
FROM 
    Empleados e
WHERE 
    e.salario > (
        SELECT 
            AVG(salario)
        FROM 
            Empleados
    );
```



```sql

SELECT 
    p.nombre AS Producto, 
    COUNT(dp.producto_id) AS VecesPedidos
FROM 
    Productos p
JOIN 
    DetallesPedido dp ON p.id = dp.producto_id
GROUP BY 
    p.id
HAVING 
    COUNT(dp.producto_id) > 5;

```

```sql

SELECT 
    p.id AS PedidoID, 
    p.total AS TotalPedido
FROM 
    Pedidos p
WHERE 
    p.total > (
        SELECT 
            AVG(total)
        FROM 
            Pedidos
    );

```


```sql

SELECT 
    pr.nombre AS Proveedor, 
    COUNT(p.id) AS TotalProductos
FROM 
    Proveedores pr
JOIN 
    Productos p ON pr.id = p.proveedor_id
GROUP BY 
    pr.id
ORDER BY 
    TotalProductos DESC
LIMIT 3;
```

```sql

SELECT 
    p.nombre AS Producto,
    p.precio AS Precio
FROM 
    Productos p
WHERE 
    p.precio > (
        SELECT 
            AVG(precio)
        FROM 
            Productos
        WHERE 
            tipo_id = p.tipo_id
    );
```

```sql

SELECT 
    c.nombre AS Cliente,
    COUNT(p.id) AS TotalPedidos
FROM 
    Clientes c
JOIN 
    Pedidos p ON c.id = p.cliente_id
GROUP BY 
    c.id
HAVING 
    COUNT(p.id) > (
        SELECT 
            AVG(TotalPedidos)
        FROM (
            SELECT 
                cliente_id, 
                COUNT(id) AS TotalPedidos
            FROM 
                Pedidos
            GROUP BY 
                cliente_id
        ) AS PromedioPedidos
    );


```


```sql

SELECT 
    p.nombre AS Producto, 
    p.precio AS Precio
FROM 
    Productos p
WHERE 
    p.precio > (
        SELECT 
            AVG(precio)
        FROM 
            Productos
    );
```

```sql

SELECT 
    e.nombre AS Empleado,
    e.salario AS Salario,
    e.idpuesto AS DepartamentoID
FROM 
    Empleados e
WHERE 
    e.salario < (
        SELECT 
            AVG(salario)
        FROM 
            Empleados
        WHERE 
            idpuesto = e.idpuesto
    );

```
