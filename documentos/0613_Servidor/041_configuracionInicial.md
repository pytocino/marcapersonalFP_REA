# 4.1. Configuración inicial

En este primer apartado vamos a ver los primeros pasos que tenemos que dar con _Laravel_ para empezar a trabajar con bases de datos. Para esto vamos a ver, a continuación, cómo definir la configuración de acceso, cómo crear una base de datos y cómo crear la tabla de migraciones, necesaria para crear el resto de tablas.

## Configuración de la Base de Datos

Lo primero que tenemos que hacer para trabajar con bases de datos es completar la configuración. Como ejemplo, vamos a configurar el acceso a una base de datos tipo _MariaDB_. Si editamos el fichero con la configuración `config/database.php` podemos ver en primer lugar la siguiente línea:

```php
'default' => env('DB_CONNECTION', 'sqlite'),
```

Este valor indica el tipo de base de datos a utilizar por defecto. Como vimos en el primer capítulo _Laravel_ utiliza el sistema de variables de entorno (`.env`) para separar las distintas configuraciones de usuario o de máquina. El método `env('DB_CONNECTION', 'sqlite')` lo que hace es obtener el valor de la variable `DB_CONNECTION` del fichero `.env`. En caso de que dicha variable no esté definida devolverá el valor por defecto `sqlite`.

En este mismo fichero de configuración, dentro de la sección `connections`, podemos encontrar todos los campos utilizados para configurar cada tipo de base de datos, en concreto la base de datos tipo `mariadb` tiene los siguientes valores:

```php
        'mariadb' => [
            'driver' => 'mariadb',
            'url' => env('DB_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'laravel'),
            'username' => env('DB_USERNAME', 'root'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => env('DB_CHARSET', 'utf8mb4'),
            'collation' => env('DB_COLLATION', 'utf8mb4_unicode_ci'),
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],
```

Como se puede ver, básicamente los campos que tenemos que configurar para usar nuestra base de datos son: `host`, `database`, `username` y `password`. El `host` lo configuraremos para que alcance el contenedor de MariaDB. Los otros tres campos tenemos que configurarlos con el nombres de la base de datos a utilizar y el usuario y la contraseña de acceso. Para poner estos valores abrimos el fichero `.env` de la raíz del proyecto y los actualizamos:

```php
DB_CONNECTION=mariadb
DB_HOST=mariadb
DB_DATABASE=nombre-base-de-datos
DB_USERNAME=nombre-de-usuario
DB_PASSWORD=contraseña-de-acceso
```

## Crear la base de datos

Para crear la base de datos que vamos a utilizar en _MariaDB_ podemos utilizar la herramienta _PHPMyAdmin_ que hemos iniciado con _Laradock_. Para esto accedemos a la ruta:

[http://localhost:8081/](http://localhost:8081/)

La cual nos mostrará un panel para la gestión de las bases de datos de _MySQL_, que nos permite, además de realizar cualquier tipo de consulta *SQL*, crear nuevas bases de datos o tablas, e insertar, modificar o eliminar los datos directamente. En nuestro caso, pulsamos sobre la pestaña "Cuentas de usuarios" y agregamos un nuevo usuario, asegurándonos de seleccionar la opción _"Crear base de datos con el mismo nombre y otorgar todos los privilegios"_. Los datos que pongamos al usuario tienen que coincidir con los que hayamos indicado en el fichero de configuración de _Laravel_.

## Tabla de migraciones

A continuación, vamos a crear la tabla de migraciones. En la siguiente sección veremos en detalle que es esto, de momento solo decir que _Laravel_ utiliza las migraciones para poder definir y crear las tablas de la base de datos desde código, y de esta manera tener un _control de las versiones_ de las mismas.

Para poder empezar a trabajar con las migraciones es necesario, en primer lugar, crear la tabla de migraciones. Para esto tenemos que ejecutar el siguiente comando de Artisan:

```bash
php artisan migrate:install
```

_Si nos diese algún error tendremos que revisar la configuración que hemos puesto de la base de datos y si hemos creado la base de datos con el nombre, usuario y contraseña indicado._

> Si estás siguiendo este tutorial desde el principio, la tabla de migraciones la habrás creado en una fase anterior de la aplicación.

Si todo funciona correctamente, podremos ir al navegador y acceder de nuevo a nuestra base de datos con _PHPMyAdmin_ y comprobaremos que se nos habrá creado la tabla `migrations`. Con esto ya tenemos configurada la base de datos y el acceso a la misma. En las siguientes secciones veremos como añadir tablas y posteriormente como realizar consultas.
