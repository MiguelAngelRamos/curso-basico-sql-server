```sql
/* Crear base de datos para la práctica */
IF DB_ID('TiendaDB') IS NULL
    CREATE DATABASE TiendaDB;
GO
USE TiendaDB;
GO

/* 1. Tabla Cliente */
IF OBJECT_ID('dbo.Cliente', 'U') IS NOT NULL DROP TABLE dbo.Cliente;
CREATE TABLE dbo.Cliente (
    ClienteID       INT IDENTITY(1,1)          NOT NULL,
    Nombre          NVARCHAR(80)               NOT NULL,
    Apellido        NVARCHAR(80)               NOT NULL,
    Correo          NVARCHAR(120)              NULL UNIQUE,
    FechaRegistro   DATETIME2                  NOT NULL DEFAULT SYSDATETIME(),
    CONSTRAINT PK_Cliente PRIMARY KEY (ClienteID)
);
GO

/* 2. Tabla Categoria */
IF OBJECT_ID('dbo.Categoria', 'U') IS NOT NULL DROP TABLE dbo.Categoria;
CREATE TABLE dbo.Categoria (
    CategoriaID     INT IDENTITY(1,1)          NOT NULL,
    Nombre          NVARCHAR(100)              NOT NULL UNIQUE,
    CONSTRAINT PK_Categoria PRIMARY KEY (CategoriaID)
);
GO

/* 3. Tabla Producto */
IF OBJECT_ID('dbo.Producto', 'U') IS NOT NULL DROP TABLE dbo.Producto;
CREATE TABLE dbo.Producto (
    ProductoID      INT IDENTITY(1,1)          NOT NULL,
    Nombre          NVARCHAR(120)              NOT NULL,
    Precio          DECIMAL(10,2)              NOT NULL CHECK (Precio >= 0),
    Stock           INT                        NOT NULL CHECK (Stock >= 0),
    CategoriaID     INT                        NOT NULL,
    CONSTRAINT PK_Producto     PRIMARY KEY (ProductoID),
    CONSTRAINT FK_Producto_Categoria FOREIGN KEY (CategoriaID)
        REFERENCES dbo.Categoria (CategoriaID)
        ON UPDATE CASCADE
        ON DELETE NO ACTION
);
GO

/* 4. Tabla Pedido */
IF OBJECT_ID('dbo.Pedido', 'U') IS NOT NULL DROP TABLE dbo.Pedido;
CREATE TABLE dbo.Pedido (
    PedidoID        INT IDENTITY(1,1)          NOT NULL,
    ClienteID       INT                        NOT NULL,
    FechaPedido     DATETIME2                  NOT NULL DEFAULT SYSDATETIME(),
    Total           AS (
        SELECT SUM(Cantidad * PrecioUnitario)
        FROM dbo.DetallePedido dp
        WHERE dp.PedidoID = PedidoID
    ) PERSISTED,                -- Computed column
    CONSTRAINT PK_Pedido PRIMARY KEY (PedidoID),
    CONSTRAINT FK_Pedido_Cliente FOREIGN KEY (ClienteID)
        REFERENCES dbo.Cliente (ClienteID)
        ON UPDATE CASCADE
        ON DELETE NO ACTION
);
GO

/* 5. Tabla DetallePedido */
IF OBJECT_ID('dbo.DetallePedido', 'U') IS NOT NULL DROP TABLE dbo.DetallePedido;
CREATE TABLE dbo.DetallePedido (
    DetalleID       INT IDENTITY(1,1)          NOT NULL,
    PedidoID        INT                        NOT NULL,
    ProductoID      INT                        NOT NULL,
    Cantidad        SMALLINT                   NOT NULL CHECK (Cantidad > 0),
    PrecioUnitario  DECIMAL(10,2)              NOT NULL CHECK (PrecioUnitario >= 0),
    CONSTRAINT PK_DetallePedido PRIMARY KEY (DetalleID),
    CONSTRAINT FK_Detalle_Pedido   FOREIGN KEY (PedidoID)
        REFERENCES dbo.Pedido (PedidoID)
        ON UPDATE CASCADE
        ON DELETE CASCADE,
    CONSTRAINT FK_Detalle_Producto FOREIGN KEY (ProductoID)
        REFERENCES dbo.Producto (ProductoID)
        ON UPDATE CASCADE
        ON DELETE NO ACTION
);
GO

/* Índices de apoyo (rendimiento): */
CREATE INDEX IX_Producto_Categoria   ON dbo.Producto (CategoriaID);
CREATE INDEX IX_Pedido_Cliente       ON dbo.Pedido   (ClienteID);
CREATE INDEX IX_Detalle_PedidoID     ON dbo.DetallePedido (PedidoID);
GO
```