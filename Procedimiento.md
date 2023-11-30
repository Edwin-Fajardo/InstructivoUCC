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
git clone https://github.com/RedTeleinformatica/BlackPHP.git .
```

Modo SSH:

```bat
cd carpeta\vacía\del\proyecto
git clone git@github.com:RedTeleinformatica/BlackPHP.git .
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

Instalación para una entidad
----------------------------

Desde el navegador, acceda a <http://test1.tallerucc.ni>. Esta acción, cuando la entidad aún no existe, lo dirigirá a la vista de inicio de sesión de instalador.

Acceda con las siguientes credenciales:  
Usuario: **admin**  
Contraseña: **instaladorUCC**

Llene los datos con las indicaciones que se darán en el momento.

Agregar una nueva tabla
-----------------------

Para el óptimo funcionamiento del Framework, se requiere que una tabla _entidad_ contenga en su estructura, los campos siguientes:

**Llave primaria**: (No importa el nombre).  
**Id de la entidad**: (entity_id) servirá para identificar la entidad (empresa, negocio, organización) a la que pertenece el registro.  
**Usuario creador**: (creation_user) Id del usuario que creó el registro.  
**Fecha y hora de creación**: (creation_time).  
**Usuario editor**: (edition_user) Id del usuario que hizo la última modificación en el registro.  
**Fecha y hora de edición**: (edition_time) Fecha y hora de la última modificación en el registro.  
**Estado**: (status), por defecto 1, este campo puede ser utilizado con valores distintos, pero se entenderá que el valor cero (o nulo en algunos casos), significa que el elemento ha sido eliminado.

Desde PHPMyAdmin, agregue una tabla llamada **clientes** con los siguientes campos:
|Campo|Tipo|Longitud|
|-----|----|--------|
|cliente_id|int||
|entity_id|int||
|nombre|varchar|128|
|direccion|tinytext||
|telefono|varchar|16|
|fecha_nacimiento|date||
|creation_user|int||
|creation_time|datetime||
|edition_user|int||
|edition_time|datetime||
|status|tinyint||

Alternativamente, puede utilizar el comando desde la sección de [Comandos MySQL](CodigoMySQL).

Agregar una clase en el ORM para control de la tabla
----------------------------------------------------

En la carpeta ```models/ORM/```, agregue un archivo llamado ```clientesModel.php```, con el código de acuerdo con el ejemplo.

Agregar un nuevo módulo en el código PHP
----------------------------------------

En la carpeta ```controllers```, cree un fichero con el nombre ```Clientes.php```, con el código de la clase controlador desde el ejemplo.

Crear las vistas respectivas en HTML
------------------------------------

Agregar un elemento y sus permisos en la base de datos
------------------------------------------------------

Agregar el acceso del menú
--------------------------

Pruebas
-------
