# Express Profesional

Plantilla de una API Express con arquitectura MVC basada en servicios, MySQL, Sequelize y definiciones SQL en archivos JavaScript.

## Requisitos

- Node.js instalado.
- XAMPP con el servicio MySQL iniciado.
- Un usuario de MySQL con permisos para crear bases de datos y tablas.

## Instalación

Desde la raíz del proyecto:

```bash
npm install
```

Configura las credenciales en `.env`:

```env
PORT=3000
NODE_ENV=development

DB_HOST=localhost
DB_PORT=3306
DB_NAME=express_profesional
DB_USER=root
DB_PASSWORD=tu_password
```

En una instalación habitual de XAMPP, `DB_USER` suele ser `root`. Si el usuario no tiene contraseña, deja `DB_PASSWORD` vacío. No subas `.env` al repositorio.

## Comandos disponibles

### Iniciar en producción

```bash
npm start
```

Ejecuta `src/server.js`. Antes de abrir el puerto, conecta con MySQL y crea/verifica la base y sus tablas.

### Iniciar en desarrollo

```bash
npm run dev
```

Usa `node --watch`, por lo que reinicia el proceso cuando cambian los archivos JavaScript.

### Crear el catálogo geográfico de Venezuela

```bash
npm run db:geo
```

Genera `data/geografia.db` con tablas relacionadas de estados, municipios y parroquias. El catálogo alimenta los selectores dependientes de residencia, lugar de nacimiento, ubicación del centro de votación y contacto de emergencia en los formularios de alta y edición. Los teléfonos móviles y del contacto usan selección de operadora; el fijo usa código de área. Sus datos fuente y licencia se encuentran en `src/config/Database/venezuela-geography.json` y `src/config/Database/GEOGRAPHY-DATA-LICENSE.txt`.

Los formularios también registran las fechas de ingreso y egreso (si aplica) y la descripción del servicio en la función policial, además de la institución policial anterior y sus antecedentes cuando corresponda. Para el expediente académico de la carrera policial se registra la fecha de ingreso según el comunicado DICIPOL y se adjunta el archivo PDF/JPG/PNG (máximo 10 MB). El comunicado se conserva en `data/private/dicipol`, fuera de los archivos públicos, y se puede descargar desde el expediente con una sesión iniciada.

La trayectoria incluye fechas de ingreso y egreso de la función policial y del curso de formación, y permite adjuntar el documento de ingreso a la función policial y un PDF de respaldo del curso (máximo 10 MB cada uno). Los archivos se guardan en `data/private/trayectoria-policial` y su descarga requiere una sesión iniciada.

Si el funcionario tiene otros cursos de formación, se pueden agregar hasta 10 por envío con nombre, fechas de ingreso y egreso y un PDF de respaldo por curso. Los documentos se guardan de forma privada y se descargan con sesión iniciada.

Se pueden registrar hasta 10 cursos de formación adicionales por envío; cada curso incluye nombre, fechas de ingreso y egreso y un PDF de respaldo. Los cursos se conservan en el expediente y sus documentos se descargan con sesión iniciada.

Los datos académicos se registran en un módulo independiente: nivel de estudio, grado, especialidad, estatus, institución y fechas de ingreso y egreso. Al seleccionar un nivel superior a bachiller se habilitan los detalles de formación y la opción de registrar otra carrera. El título puede adjuntarse como PDF de hasta 10 MB y se almacena en `data/private/formacion-academica`; su descarga requiere una sesión iniciada.

El tipo de vivienda se selecciona entre propia, alquilada, familiar, cedida, prestada, asignada u otra. Los valores se validan al guardar y se pueden actualizar desde el formulario de edición.

La gravedad del inmueble se selecciona como deteriorado o alto riesgo. Para hijos, personas mayores a cargo y personas con condiciones médicas, el formulario pregunta primero si existen; al responder que sí, solicita la cantidad y la valida como un entero positivo. Estos datos se conservan también al editar el expediente.

La cantidad de núcleos familiares y de personas que viven en la vivienda se ingresa como números enteros; el formulario muestra ejemplos de referencia. Si hay personas con condiciones médicas a cargo, se debe indicar la cantidad y describir sus condiciones (hasta 1000 caracteres). La descripción se guarda y se muestra en el expediente.

El módulo de contacto de emergencia permite responder si se desean agregar contactos adicionales. Cada uno incluye nombre, estado, municipio, parroquia, referencia de dirección, teléfono y parentesco; los selectores geográficos dependientes se comportan igual que los demás módulos y los contactos se conservan al editar el expediente.

Se pueden adjuntar documentos adicionales al expediente con su fecha de emisión y vencimiento. El expediente marca los documentos vencidos y los que vencen dentro de los próximos 30 días; los archivos se almacenan en `data/private/expedientes`, fuera de los archivos públicos, y su adjunto y descarga requieren una sesión iniciada.

El Carnet de la Patria registra código y serial, y permite adjuntar las fotos JPG/PNG del frente y reverso (máximo 10 MB por foto). Las imágenes se guardan en `data/private/carnet-patria` y solo se descargan con una sesión iniciada.

La partida de nacimiento admite un archivo PDF, JPG o PNG de hasta 10 MB. Se almacena fuera del directorio público en `data/private/partidas-nacimiento` y su descarga requiere una sesión iniciada.

Los años de servicio en la administración pública se calculan automáticamente como años completos a partir de la fecha de ingreso; no se puede ingresar manualmente el total.

La talla de zapato se registra numéricamente junto con el sistema seleccionado (EU o EEUU).

La talla de camisa se selecciona entre XS, S, M, L, XL, XXL y XXXL.

La talla de pantalón se ingresa como un número mayor que cero.

La talla de cinturón también se ingresa como un número mayor que cero.

El expediente permite adjuntar un documento de formación militar en PDF o una foto del carnet militar en JPG/PNG (máximo 10 MB). El archivo se guarda en `data/private/formacion-militar` y su descarga requiere una sesión iniciada.

### Crear o verificar la base y las tablas

```bash
npm run db:setup
```

Este comando ejecuta `src/config/Database/Insertion.js`. La base de datos se crea si todavía no existe y las tablas se crean o verifican mediante SQL.

### Insertar datos de ejemplo

```bash
npm run seed
```

Verifica primero la base y las tablas, y después inserta los usuarios definidos en `src/seeds/userSeed.js`. El seed se puede ejecutar varias veces porque ignora registros duplicados.

## Estructura del proyecto

```text
express-profesional/
├── package.json
├── vercel.json
├── .env
├── public/
│   ├── images/
│   ├── javascripts/
│   └── stylesheets/
├── src/
│   ├── app.js
│   ├── server.js
│   ├── config/
│   │   ├── env.js
│   │   └── Database/
│   │       ├── database.js
│   │       ├── Insertion.js
│   │       └── tables/
│   │           └── 1_user.js
│   ├── controllers/
│   │   └── userController.js
│   ├── middlewares/
│   │   ├── errorHandler.js
│   │   └── notFound.js
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   ├── index.js
│   │   └── userRoutes.js
│   ├── seeds/
│   │   └── userSeed.js
│   ├── services/
│   │   └── userService.js
│   └── utils/
│       └── response.js
└── views/
    ├── error.ejs
    └── index.ejs
```

La aplicación activa está dentro de `src/`. Los archivos `app.js`, `routes/` y `bin/www` que están en la raíz pertenecen al scaffold original y no son utilizados por los scripts actuales.

## Arquitectura MVC basada en servicios

### `src/server.js`

Es el punto de entrada. Importa la aplicación Express, ejecuta `initializeDatabase()` y solo inicia `app.listen()` cuando la conexión y las tablas están listas. Si MySQL falla, el proceso termina y el servidor no queda ejecutándose sin base de datos.

### `src/app.js`

Configura Express, pero no abre el puerto. Registra:

- `morgan('dev')` para mostrar las peticiones en consola.
- `express.json()` para recibir JSON.
- `express.urlencoded()` para recibir formularios.
- Las rutas bajo el prefijo `/api`.
- El middleware 404 y el manejador global de errores al final.

Separar `app.js` de `server.js` permite importar la aplicación en pruebas sin iniciar un servidor real.

### `routes`

Las rutas solo declaran endpoints y delegan el trabajo al controlador:

- `src/routes/index.js` registra los módulos de rutas.
- `src/routes/userRoutes.js` registra las rutas de usuarios.

La composición actual es:

```text
/api
└── /users
    ├── GET /
    └── GET /:id
```

### `controllers`

Los controladores reciben `req`, `res` y `next`. No contienen consultas SQL ni reglas de persistencia. Llaman a los servicios, esperan sus resultados y construyen la respuesta HTTP.

El controlador de usuarios expone `getAll` y `getOne`.

### `services`

Los servicios contienen la lógica de negocio y son el punto intermedio entre controladores y modelos:

- `getAllUsers()` consulta todos los usuarios.
- `getUserById(id)` consulta por clave primaria y genera un error 404 si no existe.

Esto permite reutilizar la lógica desde otros controladores, tareas o pruebas sin duplicar consultas.

### `models`

Los modelos son la única capa que usa Sequelize ORM. `src/models/User.js` define el modelo `User`, sus tipos, validaciones, tabla asociada y timestamps.

El modelo representa la tabla `users` con estas propiedades:

- `id`: entero sin signo, autoincremental y clave primaria.
- `name`: texto obligatorio de hasta 100 caracteres.
- `email`: texto obligatorio, válido como email y único.
- `createdAt` y `updatedAt`: fechas administradas por Sequelize y la tabla SQL.

Los servicios usan métodos Sequelize como `findAll()`, `findByPk()` y `bulkCreate()`.

## Base de datos y tablas

### Conexión

`src/config/Database/database.js` crea una instancia única de Sequelize usando las variables exportadas por `src/config/env.js`. El dialecto configurado es `mysql` y el driver utilizado es `mysql2`.

`env.js` carga `.env` mediante `dotenv` y centraliza:

- `PORT`
- `NODE_ENV`
- `DB_HOST`
- `DB_PORT`
- `DB_NAME`
- `DB_USER`
- `DB_PASSWORD`

### Definiciones SQL en archivos JavaScript

Los archivos de `src/config/Database/tables/` son JavaScript, pero contienen SQL como un string exportado. Por ejemplo, `1_user.js` exporta una sentencia `CREATE TABLE IF NOT EXISTS users (...)`.

La definición SQL y el modelo ORM tienen responsabilidades distintas:

- `tables/`: estructura física SQL de la base de datos.
- `models/`: representación ORM para consultar y modificar datos desde JavaScript.

No se usa `sequelize.sync()`. Las tablas se crean mediante las sentencias SQL de `tables/`.

### Orden de creación

Cada archivo de tabla debe comenzar con un número, seguido de guion bajo y un nombre:

```text
1_user.js
2_productos.js
3_ordenes.js
4_detalles_orden.js
```

`Insertion.js`:

1. Lee únicamente archivos `.js` directamente dentro de `tables`.
2. Rechaza archivos que no cumplan el formato numérico.
3. Ordena por el número inicial.
4. Importa cada archivo dinámicamente.
5. Busca un export que contenga una sentencia que empiece por `CREATE TABLE`.
6. Ejecuta las sentencias en el orden numérico encontrado.

Por tanto, una tabla con dependencias debe tener un número menor que las tablas que la referencian. Por ejemplo, una tabla de usuarios puede ser `1_user.js` y una tabla de pedidos `2_orders.js`.

Cada nuevo archivo debe exportar una sentencia SQL, por ejemplo:

```js
export const productTableSql = `
  CREATE TABLE IF NOT EXISTS products (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR(150) NOT NULL,
    PRIMARY KEY (id)
  );
`;
```

Después, su modelo Sequelize debe estar en `src/models/Product.js`, no dentro de `tables`.

## Seed

`src/seeds/userSeed.js` contiene tres usuarios de ejemplo. Su flujo es:

1. Ejecutar `initializeDatabase()`.
2. Insertar los usuarios con `User.bulkCreate()`.
3. Ignorar duplicados.
4. Cerrar la conexión Sequelize.

El seed no define la estructura de la tabla. La estructura pertenece al archivo numerado de `tables`.

## Endpoints actuales

### `GET /`

Comprueba que la API está disponible:

```json
{
  "message": "API Express Profesional funcionando"
}
```

### `GET /api/users`

Devuelve todos los usuarios:

```json
{
  "success": true,
  "data": []
}
```

Después de ejecutar `npm run seed`, `data` contiene los usuarios de ejemplo.

### `GET /api/users/:id`

Devuelve un usuario por su identificador. Si no existe, responde con estado HTTP 404.

Las respuestas exitosas usan el formato común de `src/utils/response.js`:

```json
{
  "success": true,
  "data": {}
}
```

## Manejo de errores

- `notFound.js` convierte las rutas inexistentes en errores 404.
- Los controladores capturan errores de sus operaciones asíncronas y los pasan a `next`.
- `errorHandler.js` centraliza la respuesta de error.
- En desarrollo incluye el `stack`; en otros entornos no lo expone.

El manejador de errores debe permanecer registrado después de todas las rutas.

## Flujo completo de una petición

Para `GET /api/users/1`:

```text
Cliente
  -> app.js
  -> /api
  -> /users
  -> userController.getOne
  -> userService.getUserById
  -> models/User.js
  -> Sequelize
  -> MySQL
  -> respuesta JSON
```

La ruta conoce al controlador, el controlador conoce al servicio y el servicio conoce al modelo. Así, cada capa tiene una responsabilidad concreta.

## Flujo de arranque

```text
npm start o npm run dev
  -> src/server.js
  -> initializeDatabase()
  -> crear base si no existe
  -> cargar tables/*.js en orden numérico
  -> ejecutar CREATE TABLE IF NOT EXISTS
  -> iniciar Express en PORT
```

## Solución de problemas

### `Access denied for user`

Revisa `DB_USER` y `DB_PASSWORD` en `.env`. Comprueba que MySQL de XAMPP esté iniciado.

### `Unknown database`

`db:setup` intenta crear la base automáticamente. Si el usuario no tiene permisos `CREATE`, créala desde phpMyAdmin o concede esos permisos.

### Archivo de tabla rechazado

Renombra el archivo para que cumpla el patrón:

```text
numero_nombre.js
```

Por ejemplo, `User.js` debe convertirse en `1_user.js`.

### El seed no inserta datos

Ejecuta primero:

```bash
npm run db:setup
npm run seed
```

También verifica que el email no esté repetido, ya que la tabla tiene una restricción única.
