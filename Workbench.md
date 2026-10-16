# MySQL Workbench
---

## Índice
- [¿Qué es?](#qué-es)
- [Instalación](#instalación)
- [Paso a paso para utilizar Workbench](#paso-a-paso-para-utilizar-workbench)
  - [1. Conectarse al servidor](#1-conectarse-al-servidor)
  - [2. Conocer la interfaz](#2-conocer-la-interfaz)
  - [3. Crear una base de datos (schema)](#3-crear-una-base-de-datos-schema)
  - [4. Crear una tabla](#4-crear-una-tabla)
  - [5. Insertar y ver registros](#5-insertar-y-ver-registros)
  - [6. Ejecutar consultas SQL](#6-ejecutar-consultas-sql)
- [Relaciones entre tablas](#relaciones-entre-tablas)
  - [Crear una foreign key desde la interfaz](#crear-una-foreign-key-desde-la-interfaz)
  - [Diagrama EER](#diagrama-eer)
- [Importar y exportar](#importar-y-exportar)
- [Atajos útiles](#atajos-útiles)
- [Errores típicos](#errores-típicos)

---

## ¿Qué es?

👉**MySQL** es el sistema de gestión de bases de datos (el "motor" que guarda los datos y entiende SQL).

👉**MySQL Workbench** es la herramienta gráfica oficial de MySQL. Nos permite gestionar las bases de datos **con ventanas, botones y diagramas** en vez de escribirlo todo por consola.

👉Con Workbench puedes:
- Conectarte a uno o varios servidores MySQL.
- Crear bases de datos y tablas con formularios.
- Escribir y ejecutar consultas SQL en un editor con colores y autocompletado.
- Ver y editar los datos de una tabla como si fuera una hoja de cálculo.
- Dibujar las relaciones entre tablas en un **diagrama EER**.

> 💡 👀 *Workbench no sustituye a SQL: por debajo sigue enviando comandos SQL al servidor. De hecho, en muchas ventanas te enseña el SQL que va a ejecutar antes de aplicarlo.*

---

## Instalación

🔗 Descarga (versión Community, gratuita): [https://dev.mysql.com/downloads/](https://dev.mysql.com/downloads/)

💻 **Windows**: descarga el *MySQL Installer*. Elige la opción **Developer Default** o **Custom** y marca **MySQL Server** y **MySQL Workbench**. Durante la instalación te pedirá una contraseña para el usuario `root`: **apúntala**, la vas a necesitar.

💻 **Mac**: descarga e instala por separado **MySQL Community Server** (archivo `.dmg`) y **MySQL Workbench** (otro `.dmg`). Al instalar el servidor también te pedirá la contraseña de `root`.

💻 **Ubuntu / Linux**:

```bash
# Servidor MySQL
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation   # aquí defines la contraseña de root
```

Workbench en Ubuntu se descarga desde la web oficial (paquete `.deb`) o desde la tienda de aplicaciones (snap):

```bash
sudo snap install mysql-workbench-community
```

---

## Paso a paso para utilizar Workbench

### 1. Conectarse al servidor

Al abrir Workbench verás la pantalla de inicio con la sección **MySQL Connections**.

1. Si instalaste el servidor en tu ordenador, normalmente ya aparece una conexión llamada **Local instance MySQL** (o similar). Haz clic sobre ella.
2. Si no aparece, pulsa el **➕** que hay al lado de *MySQL Connections* y rellena:
   - **Connection Name**: el nombre que quieras (por ejemplo `local`).
   - **Hostname**: `127.0.0.1` (o `localhost`).
   - **Port**: `3306`.
   - **Username**: `root`.
   - **Password** → *Store in Keychain / Vault* → escribe la contraseña que pusiste al instalar.
3. Pulsa **Test Connection** para comprobar que todo funciona y luego **OK**.

> 💡 Si *Test Connection* falla, lo más probable es que el servidor MySQL no esté arrancado. Mira el apartado de [errores típicos](#errores-típicos).

### 2. Conocer la interfaz

Una vez dentro de la conexión verás tres zonas principales:

| Zona | Dónde está | Para qué sirve |
| --- | --- | --- |
| **Navigator** (panel izquierdo) | pestaña **Schemas** | lista de tus bases de datos, tablas, vistas, etc. |
| **Query editor** (centro) | pestaña *Query 1* | aquí escribes y ejecutas SQL |
| **Output / Result Grid** (abajo) | debajo del editor | resultados de las consultas y mensajes de error |

> 💡 👀 *Si no ves la lista de bases de datos, pulsa el icono de refrescar 🔄 que hay al lado de la palabra **SCHEMAS**.*

### 3. Crear una base de datos (schema)

En Workbench a las bases de datos se les llama **schemas**. Son lo mismo.

**Opción A: con la interfaz** 🖱

1. En la barra de herramientas superior, pulsa el icono de un cilindro con un ➕ (**Create a new schema**).
2. Escribe el nombre (por ejemplo `biblioteca`) y pulsa **Apply**.
3. Workbench te muestra el SQL que va a ejecutar (`CREATE SCHEMA biblioteca;`). Pulsa **Apply** otra vez y luego **Finish**.

**Opción B: con SQL** ⌨️

Escribe esto en el editor y ejecútalo (⚡ o `Ctrl + Enter`):

```sql
CREATE DATABASE biblioteca;
USE biblioteca;
```

> 💡 En ambos casos, haz **doble clic** sobre el schema en el Navigator para marcarlo como activo (se pone en **negrita**). Así las consultas que ejecutes irán a esa base de datos.

### 4. Crear una tabla

> 💡 👀 *Recuerda la convención: las tablas tienen nombres en plural y en minúscula.*

**Opción A: con la interfaz** 🖱

1. En el Navigator, despliega tu schema y haz clic derecho sobre **Tables → Create Table...**
2. Escribe el nombre de la tabla: `users`.
3. En la rejilla de columnas, añade una fila por cada columna. Las casillas de la derecha significan:
   - **PK**: Primary Key (clave primaria).
   - **NN**: Not Null (obligatorio).
   - **UQ**: Unique (no puede repetirse).
   - **AI**: Auto Increment (el número se genera solo).
4. Ejemplo para `users`:

| Column Name | Datatype | PK | NN | UQ | AI |
| --- | --- | --- | --- | --- | --- |
| id | INT | ✅ | ✅ |  | ✅ |
| name | VARCHAR(255) |  | ✅ |  |  |
| email | VARCHAR(255) |  | ✅ | ✅ |  |
| role | VARCHAR(50) |  |  |  |  |
| password | VARCHAR(255) |  | ✅ |  |  |

5. Pulsa **Apply**, revisa el SQL generado y pulsa **Apply → Finish**.

**Opción B: con SQL** ⌨️

Es exactamente lo que Workbench genera por ti:

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    role VARCHAR(50),
    password VARCHAR(255) NOT NULL
);
```

Para ver la estructura de una tabla ya creada: clic derecho sobre la tabla → **Alter Table...** (desde ahí también puedes modificarla).

### 5. Insertar y ver registros

**Ver los datos** 🖱

Pasa el ratón por encima del nombre de la tabla en el Navigator y pulsa el icono de la **rejilla** (⊞) que aparece a la derecha. Workbench ejecuta por ti:

```sql
SELECT * FROM biblioteca.users;
```

**Insertar desde la rejilla** 🖱

1. Con el resultado abierto, haz clic en la última fila vacía (la que tiene `NULL`).
2. Escribe los valores de cada columna. Deja `id` vacío, se rellena solo gracias al *auto increment*.
3. Pulsa **Apply** (abajo a la derecha). Verás el `INSERT` que se va a ejecutar. Pulsa **Apply → Finish**.

**Insertar con SQL** ⌨️

```sql
INSERT INTO users (name, email, role, password)
VALUES ('Celia', 'celia@email.com', 'admin', 'contraseñahaseada');
```

> 💡 👀 *Los cambios que haces en la rejilla **no se guardan** hasta que pulsas **Apply**. Si te equivocas, pulsa **Revert**.*

### 6. Ejecutar consultas SQL

En el editor puedes escribir varias sentencias seguidas. Hay dos rayos ⚡ en la barra de herramientas:

- ⚡ **Execute (all or selection)** → `Ctrl + Shift + Enter`: ejecuta todo el editor o solo lo que tengas seleccionado.
- ⚡ **Execute current statement** → `Ctrl + Enter`: ejecuta solo la sentencia donde está el cursor.

Los resultados aparecen abajo en **Result Grid**. Los errores aparecen en la pestaña **Output** con una ❌ roja: léelos, casi siempre te dicen la línea exacta del fallo.

```sql
-- 💁‍♀️ ver las bases de datos
SHOW DATABASES;

-- 💁‍♀️ usar una base de datos
USE biblioteca;

-- 💁‍♀️ ver las tablas
SHOW TABLES;

-- 💁‍♀️ ver la estructura de una tabla
DESCRIBE users;

-- 💁‍♀️ consultar datos
SELECT name, email FROM users WHERE role = 'admin';
```

> 💡 Si intentas ejecutar un `UPDATE` o `DELETE` sin `WHERE`, Workbench te lo bloquea con el error **"Safe update mode"**. Es una protección para que no borres toda la tabla sin querer. Puedes desactivarla en *Edit → Preferences → SQL Editor → Safe Updates*, pero mejor acostúmbrate a poner siempre el `WHERE`.

---

## Relaciones entre tablas

Las relaciones funcionan igual que en la guía de [MySQL](./MySQL.md): una **foreign key** en una tabla apunta a la **primary key** de otra.

- `1:1` uno a uno → foreign key con `UNIQUE`.
- `1:N` uno a muchos → foreign key normal en la tabla "muchos".
- `N:M` muchos a muchos → tabla intermedia con dos foreign keys.

Creamos una tabla `books` relacionada con `users` (un usuario puede tener muchos libros, `1:N`).

### Crear una foreign key desde la interfaz

1. Clic derecho en **Tables → Create Table...** y ponle de nombre `books`.
2. Añade las columnas:

| Column Name | Datatype | PK | NN | AI |
| --- | --- | --- | --- | --- |
| id | INT | ✅ | ✅ | ✅ |
| title | VARCHAR(100) |  | ✅ |  |
| author | VARCHAR(100) |  |  |  |
| user_id | INT |  |  |  |

3. Abre la pestaña **Foreign Keys** (abajo, junto a *Columns*).
4. En **Foreign Key Name** escribe `fk_books_users`.
5. En **Referenced Table** elige `biblioteca.users`.
6. En la rejilla de la derecha marca la columna `user_id` y en *Referenced Column* selecciona `id`.
7. Pulsa **Apply**. El SQL generado será algo así:

```sql
CREATE TABLE books (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    author VARCHAR(100),
    user_id INT,
    CONSTRAINT fk_books_users FOREIGN KEY (user_id) REFERENCES users(id)
);
```

> 💡 👀 *La columna de la foreign key (`user_id`) tiene que ser del **mismo tipo** que la columna a la que apunta (`users.id`). Si una es `INT` y la otra `BIGINT`, Workbench dará error.*

Para añadir una foreign key a una tabla que ya existe: clic derecho sobre la tabla → **Alter Table...** → pestaña **Foreign Keys**. Equivale a:

```sql
ALTER TABLE books
ADD CONSTRAINT fk_books_users FOREIGN KEY (user_id) REFERENCES users(id);
```

### Diagrama EER

Un diagrama **EER** (Enhanced Entity-Relationship) dibuja tus tablas y las líneas que las unen. Es muy útil para entender de un vistazo cómo está montada la base de datos.

**Generar el diagrama a partir de una base de datos que ya existe** (lo más habitual en clase):

1. Menú **Database → Reverse Engineer...** (`Ctrl + R`).
2. Elige la conexión y pulsa **Next**.
3. Marca el schema que quieres dibujar (`biblioteca`) y pulsa **Next** hasta el final.
4. Se abre una pestaña con el diagrama. Puedes mover las tablas arrastrándolas.

**Cómo leer el diagrama:**

- Cada caja es una tabla. La llave amarilla 🔑 es la primary key y el rombo rojo es una foreign key.
- Las líneas entre tablas son las relaciones. El extremo con "pata de gallo" es el lado **muchos**.
- Línea continua = la foreign key forma parte de la primary key (por ejemplo, en tablas intermedias `N:M`). Línea discontinua = foreign key normal.

**Diseñar primero y crear después:**

También puedes hacerlo al revés: dibujar las tablas en un diagrama nuevo (**File → New Model**), añadirlas con la herramienta de tabla, unirlas con las herramientas de relación `1:1`, `1:n`, `n:m` de la barra lateral y por último **Database → Forward Engineer...** para que Workbench cree todas las tablas en el servidor.

---

## Importar y exportar

**Exportar** (hacer una copia de tu base de datos en un archivo `.sql`):

1. Menú **Server → Data Export**.
2. Marca el schema que quieres exportar.
3. Elige **Export to Self-Contained File** y una ruta.
4. Pulsa **Start Export**.

**Importar** (cargar un archivo `.sql` que te han pasado):

1. Menú **Server → Data Import**.
2. Elige **Import from Self-Contained File** y selecciona el archivo.
3. En *Default Target Schema* elige la base de datos de destino (o crea una nueva con **New...**).
4. Pulsa **Start Import** y refresca el Navigator 🔄.

> 💡 Si el archivo `.sql` es pequeño, también puedes abrirlo con **File → Open SQL Script**, pulsar ⚡ y listo.

---

## Atajos útiles

| Atajo | Qué hace |
| --- | --- |
| `Ctrl + Enter` | ejecuta la sentencia donde está el cursor |
| `Ctrl + Shift + Enter` | ejecuta todo el editor (o la selección) |
| `Ctrl + T` | abre una nueva pestaña de consulta |
| `Ctrl + B` | formatea (embellece) el SQL seleccionado |
| `Ctrl + /` | comenta o descomenta la línea |
| `Ctrl + R` | Reverse Engineer (generar diagrama) |
| `Ctrl + S` | guarda el script `.sql` |

> 💡 En Mac, cambia `Ctrl` por `Cmd`.

---

## Errores típicos

**❌ "Can't connect to MySQL server on 'localhost'"**
El servidor MySQL no está arrancado. Arráncalo:
- Windows: *Servicios* → busca `MySQL80` → *Iniciar*.
- Mac: *Preferencias del sistema → MySQL → Start MySQL Server*.
- Ubuntu: `sudo systemctl start mysql`.

**❌ "Access denied for user 'root'@'localhost'"**
La contraseña es incorrecta. Edita la conexión (clic derecho → *Edit Connection*) y vuelve a guardarla.

**❌ "No database selected"**
No has marcado ningún schema como activo. Haz doble clic sobre él en el Navigator o ejecuta `USE nombre_base;`.

**❌ "Error Code: 1175. You are using safe update mode"**
Has hecho un `UPDATE` o `DELETE` sin `WHERE` sobre una clave. Añade el `WHERE` (ver [ejecutar consultas](#6-ejecutar-consultas-sql)).

**❌ "Cannot add foreign key constraint"**
La columna de la foreign key y la columna referenciada no tienen el mismo tipo, o la tabla a la que apuntas todavía no existe. Crea primero la tabla "padre" (`users`) y luego la "hija" (`books`).

**❌ No aparece la tabla que acabo de crear**
Pulsa el icono de refrescar 🔄 al lado de **SCHEMAS**.
