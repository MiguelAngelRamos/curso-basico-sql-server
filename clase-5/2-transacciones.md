###  ¿Qué es una TRANSACTION?

- Una **transacción** es un bloque de operaciones SQL que se ejecutan como una sola unidad lógica de trabajo.
- Garantiza que todas las operaciones dentro de la transacción se realicen correctamente o, en caso de error, ninguna tenga efecto (principio de “todo o nada”).

- Inicia con `BEGIN TRANSACTION`

### ¿Qué es COMMIT?

- **COMMIT** finaliza la transacción y guarda de forma permanente todos los cambios realizados durante la transacción en la base de datos.
- Después de un `COMMIT`, los cambios no se pueden deshacer.

### ¿Cuál es la contraparte de COMMIT?

- La contraparte es **ROLLBACK**.
- **ROLLBACK** deshace todos los cambios realizados en la transacción actual, dejando la base de datos como estaba antes de iniciar la transacción.

```sql


BEGIN TRANSACTION
UPDATE users
set first_name = 'Adalina'
WHERE username = 'sun61';
ROLLBACK;

BEGIN TRANSACTION
UPDATE users
set first_name = 'Adalina'
WHERE username = 'sun61';
COMMIT;


BEGIN TRANSACTION
UPDATE users
SET followers = followers + 1
WHERE username = 'sun61';
-- Simulando el Error
IF @@ROWCOUNT = 0
BEGIN
	ROLLBACK;
END
ELSE
BEGIN
	COMMIT;
END
```

## ROLLBACK

```sql
-- ROLLBACK Automático
BEGIN TRY
    BEGIN TRANSACTION;
    UPDATE users
    SET followers = followers + 1
    WHERE username = 'ironman'; --  0 FILAS AFECTADAS lo que significa que no existe un username con Iroman
    -- El cual pueda actualizar me devuelve 0 filas afectadas

    IF @@ROWCOUNT = 0
       THROW 50001, 'No se encontró el usuario.',1;

    COMMIT;
    PRINT 'Transaccion completada'
END TRY
BEGIN CATCH
    ROLLBACK;
    PRINT 'Se ejecuto el rollback por un error'
END CATCH
```