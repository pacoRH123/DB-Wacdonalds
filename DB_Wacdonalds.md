#### Script

```sql
-- Creación de la base de datos
CREATE DATABASE IF NOT EXISTS WacDonalds;
USE WacDonalds;

-- Tabla Usuario
CREATE TABLE IF NOT EXISTS Usuario (
    id_usuario INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(100) UNIQUE NOT NULL,
    contraseña VARCHAR(255) NOT NULL,
    teléfono VARCHAR(20)
);

-- Tabla Administrador
CREATE TABLE IF NOT EXISTS Administrador (
    id_administrador INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    correo VARCHAR(100) UNIQUE NOT NULL,
    contraseña VARCHAR(255) NOT NULL,
    teléfono VARCHAR(20)
);

-- Tabla Sucursal
CREATE TABLE IF NOT EXISTS Sucursal (
    id_sucursal INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    dirección TEXT NOT NULL,
    teléfono VARCHAR(20),
    horario_apertura TIME,
    horario_cierre TIME,
    id_administrador INT,
    FOREIGN KEY (id_administrador) REFERENCES Administrador(id_administrador)
);

-- Tabla Producto
CREATE TABLE IF NOT EXISTS Producto (
    id_producto INT AUTO_INCREMENT PRIMARY KEY,
    id_sucursal INT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    descripción TEXT,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT NOT NULL DEFAULT 0,
    FOREIGN KEY (id_sucursal) REFERENCES Sucursal(id_sucursal)
);

-- Tabla Orden
CREATE TABLE IF NOT EXISTS Orden (
    id_orden INT AUTO_INCREMENT PRIMARY KEY,
    id_usuario INT NOT NULL,
    id_sucursal INT NOT NULL,
    fecha DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    estado VARCHAR(50) NOT NULL DEFAULT 'Pendiente',
    FOREIGN KEY (id_usuario) REFERENCES Usuario(id_usuario),
    FOREIGN KEY (id_sucursal) REFERENCES Sucursal(id_sucursal)
);

-- Tabla Detalle_Orden
CREATE TABLE IF NOT EXISTS Detalle_Orden (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_orden INT NOT NULL,
    id_producto INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (id_orden) REFERENCES Orden(id_orden),
    FOREIGN KEY (id_producto) REFERENCES Producto(id_producto)
);

-- Tabla Metodo_Pago
CREATE TABLE IF NOT EXISTS Metodo_Pago (
    id_metodo_pago INT AUTO_INCREMENT PRIMARY KEY,
    tipo VARCHAR(50) NOT NULL,
    detalles TEXT
);

-- Tabla Pago
CREATE TABLE IF NOT EXISTS Pago (
    id_pago INT AUTO_INCREMENT PRIMARY KEY,
    id_orden INT NOT NULL,
    id_metodo_pago INT NOT NULL,
    monto DECIMAL(10, 2) NOT NULL,
    fecha_pago DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_pago_orden FOREIGN KEY (id_orden) REFERENCES Orden(id_orden) ON DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT fk_pago_metodo FOREIGN KEY (id_metodo_pago) REFERENCES Metodo_Pago(id_metodo_pago) ON DELETE RESTRICT ON UPDATE CASCADE
);
