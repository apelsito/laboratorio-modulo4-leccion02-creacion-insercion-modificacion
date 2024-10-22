# Laboratorio: Gestión de Bases de Datos 

## Contexto

Eres un analista de datos que trabaja para una empresa de logística que gestiona envíos y entregas para clientes en múltiples regiones. La empresa ha decidido mejorar su sistema de gestión de bases de datos para optimizar el manejo de la información relacionada con envíos, clientes, empleados y proveedores de servicios logísticos. Como parte del equipo, tu tarea es crear, modificar, insertar y actualizar los datos en la base de datos de la empresa.

### Objetivos del Laboratorio

- Crear una base de datos estructurada para gestionar la información de envíos, clientes, empleados y proveedores.
- Insertar datos en las tablas creadas, asegurando la consistencia y las relaciones entre ellas.
- Actualizar y modificar registros en función de los cambios de requisitos o nueva información.
- Eliminar registros obsoletos o incorrectos, manteniendo la integridad de la base de datos.
- Realizar consultas y reportes avanzados para extraer información relevante.

## Descripción del Proyecto

La empresa requiere una base de datos que contenga las siguientes entidades:

- **Clientes**: Información sobre los clientes que solicitan envíos.
- **Empleados**: Información sobre los empleados que gestionan los envíos.
- **Proveedores**: Empresas externas que proporcionan servicios de entrega y logística.
- **Envíos**: Detalles de los envíos realizados.
- **Detalle de Envíos**: Información específica sobre los productos incluidos en cada envío.
- **Regiones**: Información sobre las diferentes regiones a las que se realizan envíos.

## Esquema de la Base de Datos

El esquema de la base de datos es el siguiente:

![Esquema de la Base de Datos](https://github.com/Hack-io-Data/Imagenes/blob/main/02-Imagenes/SQL/Esquema-lab-logistica.png?raw=true)

## Ejercicios

### Ejercicio 1: Creación de la Base de Datos y Tablas

1. **Crear una base de datos** llamada `logistica`:
    ```sql
    CREATE DATABASE logistica;
    ```

2. Dentro de la base de datos, crea las siguientes tablas:
    - **Clientes**:
        ```sql
        CREATE TABLE clientes (
            id_cliente SERIAL PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL,
            email VARCHAR(100) UNIQUE NOT NULL,
            direccion VARCHAR(200) NOT NULL,
            telefono VARCHAR(15) UNIQUE NOT NULL,
            pais VARCHAR(50) NOT NULL
        );
        ```

    - **Empleados**:
        ```sql
        CREATE TABLE empleados (
            id_empleado SERIAL PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL,
            cargo VARCHAR(100) NOT NULL,
            email VARCHAR(100) UNIQUE NOT NULL
        );
        ```

    - **Proveedores**:
        ```sql
        CREATE TABLE proveedores (
            id_proveedor SERIAL PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL,
            contacto VARCHAR(100) NOT NULL,
            telefono VARCHAR(15) UNIQUE NOT NULL,
            direccion VARCHAR(200) NOT NULL
        );
        ```

    - **Envíos**:
        ```sql
        CREATE TABLE envios (
            id_envio SERIAL PRIMARY KEY,
            id_cliente INT NOT NULL,
            id_empleado INT NOT NULL,
            id_proveedor INT NOT NULL,
            fecha_envio DATE CHECK (fecha_envio < CURRENT_DATE),
            estado VARCHAR(100) NOT NULL,
            total NUMERIC(12, 2) NOT NULL,
            FOREIGN KEY (id_cliente) REFERENCES clientes (id_cliente)
                ON UPDATE RESTRICT ON DELETE CASCADE,
            FOREIGN KEY (id_empleado) REFERENCES empleados (id_empleado)
                ON UPDATE RESTRICT ON DELETE CASCADE,
            FOREIGN KEY (id_proveedor) REFERENCES proveedores (id_proveedor)
                ON UPDATE RESTRICT ON DELETE CASCADE
        );
        ```

    - **Detalle de Envíos**:
        ```sql
        CREATE TABLE detalle_envios (
            id_detalle SERIAL PRIMARY KEY,
            id_envio INT NOT NULL,
            producto VARCHAR(100) NOT NULL,
            cantidad INT NOT NULL,
            precio_unitario NUMERIC(12, 2) NOT NULL,
            FOREIGN KEY (id_envio) REFERENCES envios (id_envio)
                ON UPDATE RESTRICT ON DELETE CASCADE
        );
        ```

    - **Regiones**:
        ```sql
        CREATE TABLE regiones (
            id_region SERIAL PRIMARY KEY,
            nombre VARCHAR(100) UNIQUE NOT NULL,
            pais VARCHAR(100)
        );
        ```

### Ejercicio 2: Inserción de Datos

1. **Inserta al menos 10 clientes en la tabla `Clientes`**:
    ```sql
    INSERT INTO clientes (nombre, email, direccion, telefono, pais)
    VALUES 
    ('Lola', 'lola@gmail.com', 'Calle Lola Lolita Lola, 9', '689541236', 'España'),
    ('Gonzalo', 'tdah@gmail.com', 'Calle Sol, 15', '673254896', 'España'),
    ('Víctor', 'brasil@gmail.com', 'Calle Luna, 12', '644789521', 'España'),
    ('Ana', 'ana@gmail.com', 'Avenida de los Olivos, 23', '655214789', 'España'),
    ('Jeancha', 'jancha@gmail.com', 'Calle los Pinos, 45', '631478596', 'España'),
    ('Silvia', 'silvia@gmail.com', 'Calle del Mar, 67', '645789231', 'España'),
    ('Jaime', 'jaime@gmail.com', 'Calle de la Rosa, 19', '659874125', 'España'),
    ('Jaime Alumno', 'jaime2@gmail.com', 'Calle el Prado, 33', '622145879', 'España'),
    ('Adrián', 'adrian@gmail.com', 'Avenida de la Libertad, 10', '634895712', 'España'),
    ('David', 'david@gmail.com', 'Calle del Sol, 50', '645987321', 'España');
    ```

2. **Inserta al menos 5 empleados en la tabla `Empleados`**:
    ```sql
    INSERT INTO empleados (nombre, email, cargo)
    VALUES
    ('Carlos Martínez', 'carlos.martinez@empresa.com', 'Supervisor de Envíos'),
    ('Lucía Gómez', 'lucia.gomez@empresa.com', 'Gestor de Envíos'),
    ('Miguel Fernández', 'miguel.fernandez@empresa.com', 'Coordinador de Logística'),
    ('Laura Sánchez', 'laura.sanchez@empresa.com', 'Especialista en Operaciones'),
    ('Pablo Rodríguez', 'pablo.rodriguez@empresa.com', 'Jefe de Almacén');
    ```

3. **Inserta al menos 3 proveedores en la tabla `Proveedores`**:
    ```sql
    INSERT INTO proveedores (nombre, contacto, telefono, direccion)
    VALUES
    ('TransLogistics S.A.', 'Andrés Pérez', '912345678', 'Avenida de los Transportes, 45'),
    ('Global Shipping', 'María López', '913456789', 'Calle Comercio Internacional, 10'),
    ('Logística Express', 'Javier Gómez', '914567890', 'Paseo de la Industria, 22');
    ```

4. **Inserta al menos 8 envíos en la tabla `Envíos`**:
    ```sql
    INSERT INTO envios (id_cliente, id_empleado, id_proveedor, fecha_envio, estado, total)
    VALUES
    (1, 2, 1, '2024-08-05', 'En camino', 150),
    (2, 3, 2, '2024-08-06', 'Enviado', 250),
    (3, 1, 3, '2024-08-07', 'Pendiente', 320),
    (4, 4, 1, '2024-08-08', 'Entregado', 180),
    (5, 2, 2, '2024-08-09', 'En camino', 200),
    (6, 5, 3, '2024-08-10', 'Enviado', 275),
    (7, 3, 1, '2024-08-11', 'Pendiente', 150),
    (8, 4, 2, '2024-08-12', 'Entregado', 400);
    ```

5. **Inserta los detalles correspondientes en la tabla `Detalle de Envíos`**:
    ```sql
    INSERT INTO detalle_envios (id_envio, producto, cantidad, precio_unitario)
    VALUES
    (1, 'Iphone 13', 10, 15),
    (2, 'Dorayaki', 20, 25),
    (3, 'Bombilla Full HD', 2, 160),
    (4, 'Camiseta del Betis', 2, 90),
    (5, 'Ganas de vivir', 2, 100),
    (6, 'Condones Premium 100% No kid Guarantee', 5, 55),
    (7, 'No te pongas enfermo PRO edition', 2, 75),
    (8, 'Caja floja', 4, 100);
    ```

6. **Inserta al menos 5 regiones en la tabla `Regiones`**:
    ```sql
    INSERT INTO regiones (nombre, pais)
    VALUES
    ('Andalucía', 'España'),
    ('Cataluña', 'España'),
    ('Lombardía', 'Italia'),
    ('Île-de-France', 'Francia'),
    ('Baviera', 'Alemania');
    ```

### Ejercicio 3: Modificación y Actualización de Datos

1. **Actualizar el Estado de un Envío**:
    ```sql
    UPDATE envios 
    SET estado = 'Entregado'
    WHERE id_envio = 3;
    ```

2. **Modificar el Cargo de un Empleado**:
    ```sql
    UPDATE empleados 
    SET cargo = 'Director de Datos'
    WHERE id_empleado = 5;
    ```

3. **Incrementar el Precio Unitario de un Producto**:
    ```sql
    UPDATE detalle_envios 
    SET precio_unitario = precio_unitario * 1.10
    WHERE id_detalle = 1;
    ```

4. **Actualizar la Dirección de un Cliente**:
    ```sql
    UPDATE clientes 
    SET direccion = 'Calle Ave del Paraíso nº 1'
    WHERE id_cliente = 2;
    ```

5. **Cambiar el Proveedor de un Envío**:
    ```sql
    UPDATE envios 
    SET id_proveedor = 3
    WHERE id_envio = 4;
    ```

6. **Actualizar la Cantidad de un Producto en un Envío**:
    ```sql
    UPDATE detalle_envios
    SET cantidad = 5
    WHERE id_envio = 2;
    ```

7. **Actualizar el Total de un Envío**:
    ```sql
    UPDATE envios 
    SET total = total + 50
    WHERE id_envio = 5;
    ```

8. **Modificar el Contacto de un Proveedor**:
    ```sql
    UPDATE proveedores
    SET contacto = 'Nuevo Contacto 2'
    WHERE id_proveedor = 2;
    ```

9. **Cambiar el País de un Cliente**:
    ```sql
    UPDATE clientes 
    SET pais = 'Portugal'
    WHERE id_cliente = 6;
    ```

10. **Actualizar la Fecha de un Envío**:
    ```sql
    UPDATE envios 
    SET fecha_envio = '2024-08-10'
    WHERE id_envio = 7;
    ```

### Ejercicio 4: Eliminación de Datos

1. **Añadir una Columna `fecha_nacimiento` a la tabla `Clientes`**:
    ```sql
    ALTER TABLE clientes 
    ADD COLUMN fecha_nacimiento DATE;
    ```

2. **Añadir una Columna `codigo_postal` a la tabla `Proveedores`**:
    ```sql
    ALTER TABLE proveedores 
    ADD COLUMN codigo_postal VARCHAR(10);
    ```

3. **Eliminar la Columna `contacto` de la tabla `Proveedores`**:
    ```sql
    ALTER TABLE proveedores 
    DROP COLUMN contacto;
    ```

4. **Eliminar la Columna `pais` de la tabla `Regiones`**:
    ```sql
    ALTER TABLE regiones 
    DROP COLUMN pais;
    ```

5. **Modificar el Tipo de Dato de la Columna `telefono` en la tabla `Clientes`**:
    ```sql
    ALTER TABLE clientes 
    ALTER COLUMN telefono TYPE VARCHAR(15);
    ```

6. **Modificar el Tipo de Dato de la Columna `total` en la tabla `Envíos`**:
    ```sql
    ALTER TABLE envios 
    ALTER COLUMN total TYPE NUMERIC(12, 2);
    ```

7. **Añadir una Columna `fecha_contrato` a la tabla `Empleados`**:
    ```sql
    ALTER TABLE empleados
    ADD COLUMN fecha_contrato DATE;
    ```

8. **Eliminar la Columna `estado` de la tabla `Envíos`**:
    ```sql
    ALTER TABLE envios 
    DROP COLUMN estado;
    ```

9. **Modificar el Nombre de la Columna `nombre` en la tabla `Empleados` a `nombre_completo`**:
    ```sql
    ALTER TABLE empleados 
    RENAME COLUMN nombre TO nombre_completo;
    ```

### Ejercicio 5: Consultas SQL

1. **Listar todos los clientes que viven en España**:
    ```sql
    SELECT * FROM clientes WHERE pais = 'España';
    ```

2. **Obtener todos los envíos realizados por un empleado específico**:
    ```sql
    SELECT * FROM envios WHERE id_empleado = 3;
    ```

3. **Listar todos los productos incluidos en un envío específico**:
    ```sql
    SELECT producto FROM detalle_envios WHERE id_envio = 2;
    ```

4. **Encontrar todos los proveedores con un teléfono específico**:
    ```sql
    SELECT * FROM proveedores WHERE telefono = '912345678';
    ```

5. **Listar los empleados que tienen un cargo de "Supervisor de Envíos"**:
    ```sql
    SELECT * FROM empleados WHERE cargo = 'Supervisor de Envíos';
    ```

6. **Obtener todos los envíos que fueron realizados por el cliente con id_cliente = 5**:
    ```sql
    SELECT * FROM envios WHERE id_cliente = 5;
    ```

7. **Listar todas las regiones con su nombre y país**:
    ```sql
    SELECT * FROM regiones;
    ```

8. **Mostrar todos los productos cuyo precio unitario sea mayor que 50**:
    ```sql
    SELECT * FROM detalle_envios WHERE precio_unitario >= 50;
    ```

9. **Obtener todos los envíos realizados el 2024-08-05**:
    ```sql
    SELECT * FROM envios WHERE fecha_envio = '2024-08-05';
    ```

10. **Listar todos los clientes que tienen un número de teléfono que comienza con "6003"**:
    ```sql
    SELECT * FROM clientes WHERE telefono LIKE '6003%';
    ```
