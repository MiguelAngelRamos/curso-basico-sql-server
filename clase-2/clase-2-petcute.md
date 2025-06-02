```sql
CREATE DATABASE PetCuteDB;
GO

USE PetCuteDB;
GO

-- Tabla: Clientes
CREATE TABLE Clientes (
    ClienteID INT PRIMARY KEY IDENTITY(1,1),
    Nombre NVARCHAR(100) NOT NULL,
    Apellido NVARCHAR(100) NOT NULL,
    Telefono NVARCHAR(20),
    Correo NVARCHAR(100),
    FechaRegistro DATE DEFAULT GETDATE()
);

-- Tabla: Mascotas
CREATE TABLE Mascotas (
    MascotaID INT PRIMARY KEY IDENTITY(1,1),
    Nombre NVARCHAR(100) NOT NULL,
    Especie NVARCHAR(50) NOT NULL,
    Raza NVARCHAR(50),
    FechaNacimiento DATE,
    ClienteID INT FOREIGN KEY REFERENCES Clientes(ClienteID)
);

-- Tabla: Veterinarios
CREATE TABLE Veterinarios (
    VeterinarioID INT PRIMARY KEY IDENTITY(1,1),
    Nombre NVARCHAR(100) NOT NULL,
    Especialidad NVARCHAR(100),
    Correo NVARCHAR(100)
);

-- Tabla: Servicios
CREATE TABLE Servicios (
    ServicioID INT PRIMARY KEY IDENTITY(1,1),
    Nombre NVARCHAR(100) NOT NULL,
    Precio DECIMAL(10,2) NOT NULL CHECK (Precio > 0)
);

-- Tabla: Atenciones (historial de atención médica)
CREATE TABLE Atenciones (
    AtencionID INT PRIMARY KEY IDENTITY(1,1),
    Fecha DATETIME DEFAULT GETDATE(),
    MascotaID INT FOREIGN KEY REFERENCES Mascotas(MascotaID),
    VeterinarioID INT FOREIGN KEY REFERENCES Veterinarios(VeterinarioID),
    ServicioID INT FOREIGN KEY REFERENCES Servicios(ServicioID),
    Observaciones NVARCHAR(255)
);


INSERT INTO Clientes (Nombre, Apellido, Telefono, Correo)
VALUES 
('Carlos', 'Ruiz', '912345678', 'carlos@correo.com'),
('Ana', 'Soto', '987654321', 'ana@correo.com'),
('Pedro', 'López', '954789632', 'pedro@correo.com'),
('Lucía', 'Rojas', '912356789', 'lucia@correo.com'),
('Jorge', 'Mena', '943217896', 'jorge@correo.com'),
('Sofía', 'Torres', '987123456', 'sofia@correo.com'),
('Raúl', 'Navarro', '915678234', 'raul@correo.com'),
('Camila', 'Fernández', '933789412', 'camila@correo.com'),
('Felipe', 'Contreras', '967341289', 'felipe@correo.com'),
('Isabel', 'Pizarro', '974523890', 'isabel@correo.com');


INSERT INTO Mascotas (Nombre, Especie, Raza, FechaNacimiento, ClienteID)
VALUES
('Luna', 'Perro', 'Poodle', '2018-05-10', 1),
('Milo', 'Gato', 'Siamés', '2020-03-21', 2),
('Rocky', 'Perro', 'Labrador', '2017-09-15', 3),
('Nina', 'Gato', 'Persa', '2019-11-02', 4),
('Thor', 'Perro', 'Pitbull', '2016-06-18', 5),
('Kira', 'Gato', 'Mestizo', '2021-01-12', 6),
('Bobby', 'Perro', 'Beagle', '2018-08-24', 7),
('Tom', 'Gato', 'Angora', '2020-10-03', 8),
('Max', 'Perro', 'Pastor Alemán', '2015-07-30', 9),
('Pelusa', 'Conejo', 'Enano', '2022-04-01', 10);


INSERT INTO Veterinarios (Nombre, Especialidad, Correo)
VALUES
('Dra. Paula Varela', 'Medicina General', 'paula@petcute.cl'),
('Dr. Jaime Castro', 'Cirugía', 'jaime@petcute.cl'),
('Dra. Camila Salas', 'Dermatología', 'camila@petcute.cl'),
('Dr. Nicolás Gómez', 'Odontología', 'nicolas@petcute.cl');


INSERT INTO Servicios (Nombre, Precio)
VALUES
('Consulta General', 20000),
('Vacunación', 15000),
('Desparasitación', 10000),
('Cirugía Menor', 60000),
('Limpieza Dental', 25000);


INSERT INTO Atenciones (MascotaID, VeterinarioID, ServicioID, Observaciones)
VALUES
(1, 1, 1, 'Consulta de rutina.'),
(2, 2, 2, 'Vacuna triple felina.'),
(3, 1, 3, 'Desparasitación oral.'),
(4, 3, 1, 'Problemas de piel.'),
(5, 2, 4, 'Cirugía de esterilización.'),
(6, 4, 5, 'Limpieza dental por sarro.'),
(7, 1, 1, 'Chequeo general.'),--
-+

-
  
(8, 1, 2, 'Vacuna antirrábica.'),
(9, 2, 4, 'Cirugía por hernia.'),
(10, 3, 3, 'Desparasitación preventiva.');
```