# jardineria-sql
Consultas SQL para la base de datos Jardinería
Listar información básica de las oficinas:
sql
SELECT codigo_oficina, ciudad, pais, telefono FROM oficina;
Obtener los empleados por oficina:
sql
SELECT nombre, apellido1, apellido2, codigo_oficina FROM empleado ORDER BY codigo_oficina;
Calcular el promedio de salario (límite de crédito) de los clientes por región:
sql
SELECT region, AVG(limite_credito) AS promedio_limite_credito FROM cliente GROUP BY region;
Listar clientes con sus representantes de ventas:
sql
SELECT c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1) AS representante_ventas 
FROM cliente c 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
Obtener productos disponibles y en stock:
sql
SELECT codigo_producto, nombre_producto, cantidad_en_stock FROM producto WHERE cantidad_en_stock > 0;
Productos con precios por debajo del promedio:
sql
SELECT * FROM producto WHERE precio_venta < (SELECT AVG(precio_venta) FROM producto);
Pedidos pendientes por cliente:
sql
SELECT p.codigo_pedido, p.estado, c.nombre_cliente 
FROM pedido p 
JOIN cliente c ON p.codigo_cliente = c.codigo_cliente 
WHERE p.estado <> 'Entregado';
Total de productos por categoría (gama):
sql
SELECT gama, COUNT(*) AS total_productos 
FROM producto GROUP BY gama;
Ingresos totales generados por cliente:
sql
SELECT c.nombre_cliente, SUM(p.total) AS ingresos_totales 
FROM cliente c 
JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
GROUP BY c.nombre_cliente;
Pedidos realizados en un rango de fechas:
sql
SELECT codigo_pedido FROM pedido WHERE fecha_pedido BETWEEN '2025-01-01' AND '2025-01-31';
Detalles de un pedido específico:
sql
SELECT dp.codigo_pedido, dp.codigo_producto, dp.cantidad, dp.precio_total 
FROM detalle_pedido dp WHERE dp.codigo_pedido = 'especificar_codigo';
Productos más vendidos:
sql
SELECT p.nombre_producto, SUM(dp.cantidad) AS total_vendido 
FROM detalle_pedido dp 
JOIN producto p ON dp.codigo_producto = p.codigo_producto 
GROUP BY p.nombre_producto ORDER BY total_vendido DESC;
Pedidos con un valor total superior al promedio:
sql
SELECT * FROM pedido WHERE (cantidad * precio_unitario) > (SELECT AVG(cantidad * precio_unitario) FROM pedido);
Clientes sin representante de ventas asignado:
sql
SELECT * FROM cliente WHERE codigo_empleado_rep_ventas IS NULL;
Número total de empleados por oficina:
sql
SELECT codigo_oficina, COUNT(*) AS total_empleados 
FROM empleado GROUP BY codigo_oficina;
Pagos realizados en una forma específica:
sql
SELECT * FROM pago WHERE forma_pago = 'Tarjeta de Crédito';
Ingresos mensuales:
sql
SELECT MONTH(fecha_pago) AS mes, SUM(total) AS ingresos_totales 
FROM pago GROUP BY mes;
Clientes con múltiples pedidos:
sql
SELECT nombre_cliente FROM cliente c 
JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
GROUP BY c.codigo_cliente HAVING COUNT(p.codigo_pedido) > 1;
Pedidos con productos agotados:
sql
SELECT DISTINCT p.codigo_pedido 
FROM detalle_pedido dp 
JOIN producto p ON dp.codigo_producto = p.codigo_producto 
WHERE p.cantidad_en_stock = 0;
Promedio, máximo y mínimo del límite de crédito de los clientes por país:
sql
SELECT pais, AVG(limite_credito) AS promedio_limite_credito,
       MAX(limite_credito) AS maximo_limite_credito,
       MIN(limite_credito) AS minimo_limite_credito 
FROM cliente GROUP BY pais;
Historial de transacciones de un cliente:
sql
SELECT fecha_pago, total, forma_pago 
FROM pago WHERE codigo_cliente = 'especificar_codigo';
Empleados sin jefe directo asignado:
sql
SELECT * FROM empleado WHERE codigo_jefe IS NULL;
Productos cuyo precio supera el promedio de su categoría (gama):
sql
SELECT * FROM producto p WHERE precio_venta > (SELECT AVG(precio_venta) FROM producto WHERE gama = p.gama);
Promedio de días de entrega por estado:
sql
SELECT estado, AVG(DATEDIFF(fecha_entrega, fecha_pedido)) AS promedio_dias_entrega 
FROM pedido GROUP BY estado;
Clientes por país con más de un pedido:
sql
SELECT pais, COUNT(DISTINCT codigo_cliente) AS cantidad_clientes 
FROM cliente c JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
GROUP BY pais HAVING COUNT(p.codigo_pedido) > 1;
