Procedimiento para desarrollo de taller
=======================================

Nota: Si descargó el proyecto desde la versión portable en la nube, entonces
puede iniciar en el paso [Configuración de parámetros](#configuración-de-parámetros).

Instalación de XAMPP
--------------------

Instale XAMPP para Windows con soporte para PHP 8.2

Enlace de descarga de XAMPP: <https://www.apachefriends.org/download.html>

Instalación de Git para Windows
-------------------------------

Enlace de descarga de Git para Windows: <https://git-scm.com/download/win>

Clonación de repositorio
------------------------

Modo HTTP:

```bat
cd carpeta\vacía\del\proyecto
git clone https://github.com/Edwin-Fajardo/InstructivoUCC.git .
```

Modo SSH:

```bat
cd carpeta\vacía\del\proyecto
git clone git@github.com:Edwin-Fajardo/InstructivoUCC.git .
```

Carga de base de datos
----------------------

Inicie los servicios de XAMPP (Apache y Mysql), y seguidamente, ingrese a <http://127.0.0.1/phpmyadmin>.

Cree una nueva base de datos con el nombre tallerucc.

Cree un usuario llamado usuariotaller, asígnele una contraseña, y dele todos permisos para la base de datos tallerucc.

En el proyecto clonado, busque el archivo ```db/mysql/db_structure.sql```, y impórtelo en la base de datos creada.

Asimismo, importe el archivo ```db/mysql/initial_data.sql```

Configuración de acceso a base de datos
---------------------------------------

En el proyecto, copie el archivo ```default_config.php``` a un nuevo archivo llamado ```config.php```.

Configure los parámetros de acuerdo con la base de datos creada.

Configuración de parámetros
---------------------------

En el fichero ```app_info.json``` encontrará información sobre la aplicación. Cambie los parámetros siguientes:

Agregar una nueva tabla
-----------------------

Desde PHPMyAdmin, agregue una tabla llamada **clientes** con los siguientes campos:
|Campo|Tipo|
|-----|----|
|cliente_id|varchar(128)|

Alternativamente, puede utilizar el comando desde la sección de [Comandos MySQL](CodigoMySQL).

Agregar una clase en el ORM para control de la tabla
----------------------------------------------------

Agregar un nuevo módulo en el código PHP
----------------------------------------

Crear las vistas respectivas en HTML
------------------------------------

Agregar un elemento y sus permisos en la base de datos
------------------------------------------------------

Agregar el acceso del menú
--------------------------

Pruebas
-------

Agregar un ID de entidad
------------------------
