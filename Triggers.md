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
