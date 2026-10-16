
# MySQL
---
Indice:
- [¿Qué es?](#qué-es)
- [Paso a paso para utilizar MySQL](#paso-a-paso-para-utilizar-mysql)
- [Relaciones entre tablas](#relaciones-entre-tablas)


## ¿Qué es?

👉**SQL** es un lenguaje para hacer consultas a datos.

👉**MySQL** Es un sistema de gestión de Bases de datos relacionales. Es un conjunto de programas que nos permite gestionar todos los datos sin tener que programar demasiado. Nos da las funciones de SQL ya configuradas👌

👉**Workbench** es un entorno de desarrollo integrado que nos permite gestionarlo todo con una interfaz gráfica más amigable. 👁👉 [Guía de Workbench](./Workbench.md)
    
👉**RDBMS:**  (Relational Database Management System, o Sistema de Gestión de Bases de Datos Relacional) es un tipo de software que gestiona y organiza bases de datos en las que los datos se almacenan y estructuran en **tablas** que están relacionadas entre sí. 
    
👉 La **concurrencia de datos** en un **RDBMS** se refiere a la capacidad del sistema para permitir que **múltiples usuarios o procesos accedan y modifiquen los mismos datos simultáneamente**, sin que esto afecte la integridad, consistencia o precisión de los datos.

---
## Paso a paso para utilizar MySQL
    

Primero de todo instalamos **MySQL Workbench** 💻

( versión comunitaria gratuita) [https://dev.mysql.com/downloads/](https://dev.mysql.com/downloads/)

💻Windows: tiene un instalador que instala todo de una.

💻Mac: hay que descargarse e instalar por un lado MySQL Server y por otro lado MySQL Workbench.

💻Ubuntu: se instala MySQL por comando de consola y se descarga de la app store workbench.




### comandos básicos

Antes de ponernos a crear bases de datos y tablas, tengamos en cuenta esta convención:

> 💡 👀 *las tablas tienen nombres en plural y se escriben en minúscula*

Existen varias formas de crear bases de datos, si hacemos uso de mysql solo por consola:

primero buscas la ruta donde tienes el archivo mysql.exe que suele ser un path de este palo:

`C:\Program Files\MySQL\MySQL Server 8.2\bin`

```mysql

mysql -u root -p
-- 💁‍♀️Escribes  tu contraseña
-- 💁‍♀️ comando para visualizar las bases de datos que tienes ya creadas.
show databases;

-- 💁‍♀️ si vas a usar una base de datos que ya existe
use el_nombre_de_la_basededatos;

-- 💁‍♀️ si vas a crear una nueva base de datos
create database nombre_nueva_base_de_datos;
use nombre_nueva_base_de_datos;

-- 💁‍♀️ si vas a crear una tabla por ejemplo una de user
create table users(id INT PRIMARY KEY NOT NULL AUTO_INCREMENT, name VARCHAR(255) NOT NULL, email VARCHAR(255) NOT NULL, role VARCHAR(255), password VARCHAR(255)NOT NULL);

-- 💁‍♀️si quieres ver tus tablas
show tables;

-- 💁‍♀️si quieres ver la estructura concreta de una tabla
describe nombre_de_la_tabla;
```


```sql
-- 💁‍♀️si quieres añadir por terminal un registro dentro de una tabla
INSERT INTO users (name, email, password, role )
VALUES 
('Celia', 'celia@email.com', 'contraseñahaseada', 'admin');
```
---

## Relaciones entre tablas 

### one-to-one
En SQL, una relación **one-to-one** (uno a uno) entre dos tablas significa que cada registro en una tabla está relacionado con exactamente un registro en la otra tabla, y viceversa. Esto se puede lograr mediante el uso de una clave primaria y una clave externa.

> la clave foránea se coloca en la tabla que representa la entidad dependiente para hacer una one to one utilizamos la palabra `UNIQUE` para que se ese campo no pueda repetirse
> 

```sql
CREATE TABLE dni (
  Id INT PRIMARY KEY,
    user_id INT UNIQUE, -- Clave foránea que relaciona con user_id
    dni_number VARCHAR(100),
    FechaVencimiento DATE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

```
### one-to-many
una relación **one-to-many** (uno a muchos) en una base de datos significa que un registro en una tabla está relacionado con uno o más registros en otra tabla

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    user_name VARCHAR(50),
    email VARCHAR(100)
);
```

```sql
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(100),
    author VARCHAR(100),
    user_id INT,
    FOREIGN KEY (User_ID) REFERENCES users(user_id)
);
```
### many-to-many
Una relación **many-to-many** (muchos a muchos) en una base de datos ocurre cuando un conjunto de registros en una tabla está relacionado con un conjunto de registros en otra tabla, y viceversa. Esto significa que un registro en **una tabla puede estar relacionado con varios registros en la otra tabla**, y a su vez, un registro **en la segunda tabla puede estar relacionado con varios registros en la primera tabla.**

```sql
CREATE TABLE categories (
    category_id INT PRIMARY KEY,
    category_name VARCHAR(50)
);
```

```sql
CREATE TABLE books (
book_id INT PRIMARY KEY,
title VARCHAR(100),
author VARCHAR(100)
);
```

```sql
CREATE TABLE books_categories (
book_id INT,
category_id INT,
PRIMARY KEY (Book_ID, category_id),
FOREIGN KEY (Book_ID) REFERENCES books(book_id),
FOREIGN KEY (Category_ID) REFERENCES categories(category_id)
);
```

`PRIMARY KEY (book_id, category_id),`

Esto es fundamental en una tabla de asociación many-to-many, ya que queremos asegurarnos de que no haya duplicados de relaciones entre libros y categorías. Al hacer que la combinación de **`book_id`** y **`category_id`** sea la clave primaria, garantizamos que cada libro solo pueda estar asociado con una categoría una vez, y viceversa.

La clave primaria compuesta indica que la combinación de valores en estas columnas identificará de manera única cada fila en la tabla **`Books_Categories`**. Por ejemplo, en una fila específica de **`Books_Categories`**, el par de valores **`(book_id, category_id)`** debe ser único en toda la tabla, lo que significa que no puede haber duplicados de esa combinación.

Entonces, no es un "array" en el sentido de una estructura de datos que contiene múltiples valores. Más bien, es una combinación de valores individuales que identifican de manera única una fila en la tabla. En este caso, es una combinación de un **`book_id`** y un **`category_id`**, cada uno de los cuales es un identificador único de una tabla relacionada (**`books`** y **`categories`** respectivamente).


### Comandos de relaciones entre tablas


```sql
-- 💁‍♀️si quiere crear una tabla con una foreign key
CREATE TABLE books(
id int auto_increment,
PRIMARY KEY (id),
title varchar(50) NOT NULL,
writer varchar(50) NOT NULL,
book_description text(500) NOT NULL,
id_user INT,
FOREIGN KEY (id_user) REFERENCES users(id),
createAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
updateAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP  
ON UPDATE CURRENT_TIMESTAMP 
);
```


```sql
-- 💁‍♀️si quiere añadir una foreign key a una tabla ya creada añadiendo una columna
 ALTER TABLE files
 ADD id_user INT,
 ADD CONSTRAINT fk_files_users FOREIGN KEY (id_user) REFERENCES users(id);
```

### notaciones


`n:m`

many-to-many

La notación "N:M" o "M:N" se refiere a una relación donde muchos elementos de un conjunto pueden estar relacionados con muchos elementos del otro conjunto. Ambos términos indican lo mismo: una relación many-to-many.

`1:N` 

uno a muchos

`1:1`

uno a uno

