Procedimiento para desarrollo de taller
=======================================

Nota: Si descargó el proyecto desde la versión portable en la nube, entonces
puedes iniciar en el paso 6.

Paso 1: Instalación de XAMPP
--------------------

Instale XAMPP para Windows con soporte para PHP 8.2

Enlace de descarga de XAMPP: <https://www.apachefriends.org/download.html>

Paso 2: Instalación de Git para Windows
---------------------------------------

Enlace de descarga de Git para Windows: <https://git-scm.com/download/win>

Paso 3: Clonación de repositorio
-------------------

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

Paso 4: Carga de base de datos
----------------------

Inicie los servicios de XAMPP (Apache y Mysql), y seguidamente, ingrese a <http://127.0.0.1/phpmyadmin>.

Cree una nueva base de datos con el nombre tallerucc.

Cree un usuario llamado usuariotaller, asígnele una contraseña, y dele todos permisos para la base de datos tallerucc.

En el proyecto clonado, busque el archivo ```db/mysql/db_structure.sql```, y impórtelo en la base de datos creada.

Asimismo, importe el archivo ```db/mysql/initial_data.sql```

Paso 5: Configuración de acceso a base de datos
-----------------------------------------------

En el proyecto, copie el archivo ```default_config.php``` a un nuevo archivo llamado ```config.php```.

Configure los parámetros de acuerdo con la base de datos creada.

Paso 6: Configuración de parámetros de la App
---------------------------------------------

Paso 7: Agregar una nueva tabla
-------------------------------

Paso 8: Agregar un nuevo módulo en el código
--------------------------------------------
