```sql

SELECT * FROM users;

-- BETWEEN
-- Obtener el nombre (first_name), apellido(last_name) y cantidad de followers(seguidores) entre mayor o igual a 4600 y menor o igual 4700;
-- SELECT * FROM tabla WHERE columna BETWEEN valor1 AND valor2;

SELECT first_name, last_name, followers FROM users WHERE followers BETWEEN 4600 AND 4700 ORDER BY followers DESC;

-- Con operadores de comparación
SELECT first_name, last_name, followers FROM users WHERE followers >= 4600 AND followers <= 4700 ORDER BY followers DESC;

-- Obtener el nombre (first_name), apellido(last_name) y cantidad de followers(seguidores) entre mayor 4600 y menor 4700;
SELECT first_name, last_name, followers FROM users WHERE followers > 4600 AND followers < 4700;

/* operador logico "and" se conoce "y"
  v and v = v
  v and f = f
  f and v = f

*/

-- Uso de Like "Buscar Patrones"
-- Obtener todos los registros de la ultima conexion con la IP '221.x.x.x'
--                                                              221.455.144.21
--                                                              221.314.131.21

SELECT first_name, last_name, last_connection FROM users WHERE last_connection LIKE '221.%.%.%';

```