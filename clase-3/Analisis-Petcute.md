```sql
-- Análisis

SELECT * FROM Mascotas;

-- Necesito el total de mascotas por especie
SELECT Especie, COUNT(*) AS Cantidad FROM Mascotas GROUP BY Especie;

-- Clientes con más de 1 mascota
SELECT ClienteID, COUNT(*) as Mascotas FROM Mascotas GROUP BY ClienteID HAVING COUNT(*) > 1;
-- Total de Mascotas que tienen los Clientes
SELECT ClienteID, COUNT(*) as TotalMascotas FROM Mascotas GROUP BY ClienteID;

SELECT * FROM Atenciones;
SELECT * FROM Servicios;

-- Antenciones por tipo de servicio
SELECT Servicios.Nombre, COUNT(*) AS TotalAtenciones FROM Atenciones
INNER JOIN Servicios ON Atenciones.ServicioID = Servicios.ServicioID
GROUP BY Servicios.Nombre;

```

