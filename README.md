# TallerMysql2

## 1
DELIMITER //
CREATE FUNCTION calcular_bonificacion(id_empleado INT) RETURNS DECIMAL(10,2)
NOT DETERMINISTIC
READS SQL DATA
BEGIN 
    DECLARE salario DECIMAL(10,2);
    DECLARE bonificacion DECIMAL(10,2);
    
    SELECT salario INTO salario
    FROM empleados
    WHERE id = id_empleado;
    
    IF salario < 2000 THEN
        SET bonificacion = salario * 0.10;
    ELSEIF salario BETWEEN 2000 AND 5000 THEN
        SET bonificacion = salario * 0.07;
    ELSE
        SET bonificacion = salario * 0.05;
    END IF;
    
    RETURN bonificacion;
    
END //

DELIMITER ;

SELECT calcular_bonificacion(1);

## 2
DELIMITER //

CREATE FUNCTION calcular_edad(id_cliente INT) RETURNS INT
DETERMINISTIC
READS SQL DATA
BEGIN 
    DECLARE fecha_nacimiento DATE;
    DECLARE edad INT;
    
    SELECT fecha_nacimiento INTO fecha_nacimiento
    FROM clientes
    WHERE id = id_cliente;
    
    SET edad = TIMESTAMPDIFF(YEAR, fecha_nacimiento, CURDATE());
    
    RETURN edad; 

END //

DELIMITER ;

SELECT calcular_edad(1);

## 3
DELIMITER //
CREATE FUNCTION formatear_telefono(telefono CHAR(10))
RETURNS VARCHAR(14)
DETERMINISTIC
BEGIN
    RETURN CONCAT('(', SUBSTRING(telefono, 1, 3), ') ', SUBSTRING(telefono, 4, 3), '-', SUBSTRING(telefono, 7, 4));
END //
DELIMITER ;
SELECT 
    id,
    nombre,
    formatear_telefono(telefono) AS telefono_formateado
FROM contactos;

CREATE TABLE contactos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    telefono CHAR(10)
);
INSERT INTO contactos (nombre, telefono) VALUES
('Simon Rubiano', '3158579904'),
('Paola Ortiz', '3008544780'),
('Carlos Ruiz', '3014534444');

## 4
CREATE TABLE productos(
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    precio DECIMAL(10, 2)
);

INSERT INTO productos (nombre, precio) VALUES
('Lampara', 35.99),
('Tablet', 150.00),
('Portatil Gamer', 999.999);

DELIMITER //

CREATE FUNCTION clasificar_precio(id_proucto INT) RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN 
    DECLARE precio DECIMAL(10,2) ;
    DECLARE categoria VARCHAR(10);
    
    SELECT precio INTO precio
    FROM productos
    WHERE id = id_producto;
    
    IF precio < 50 THEN
       SET categoria = 'bajo';
    ELSEIF precio BETWEEN 50 AND 200 THEN
       SET categoria = 'medio';
    ELSE 
       SET  categoria = 'alto';
    END IF;
    
    RETURN categoria;
    
END //

DELIMITER ;
    
SELECT clasificar_precio(1)

