Comandos MySQL
==============

Nota: Si descargó el proyecto desde la versión portable en la nube, entonces
puede iniciar en el [Creación de tabla](#crear-una-tabla).

Creación de base de datos
-------------------------

```sql
CREATE DATABASE `tallerucc`;
```

Creación de usuario
-------------------

Crear un usuario y asignarle permisos. Estos datos servirán luego en el fichero config.php

```sql
CREATE USER 'usuarioucc'@'localhost' IDENTIFIED BY 'MyJfDlNTWZ4zIS)Y';
GRANT ALL ON tallerucc.* TO 'usuarioucc'@'localhost';
```

Crear una tabla
---------------

```sql
CREATE TABLE `clientes` (
  `cliente_id` INT NOT NULL AUTO_INCREMENT COMMENT 'Llave primaria',
  `entity_id` INT NOT NULL COMMENT 'ID de la entidad',
  `nombre` VARCHAR(128) NOT NULL COMMENT 'Nombre del cliente',
  `direccion` TINYTEXT NOT NULL COMMENT 'Dirección del cliente',
  `telefono` VARCHAR(16) NOT NULL COMMENT 'Teléfono del cliente',
  `fecha_nacimiento` DATE NOT NULL COMMENT 'Fecha de nacimiento',
  `creation_user` INT NOT NULL,
  `creation_time` DATETIME NOT NULL,
  `edition_user` INT NOT NULL,
  `edition_time` DATETIME NOT NULL,
  `status` TINYINT NOT NULL DEFAULT '1',
  PRIMARY KEY (`cliente_id`)
) ENGINE = InnoDB COMMENT = 'Registro de clientes';
```

Relacionar tabla clientes con tabla entidades
---------------------------------------------

```sql
ALTER TABLE `clientes` ADD CONSTRAINT `cliente_entidad` FOREIGN KEY (`entity_id`) REFERENCES `entities`(`entity_id`) ON DELETE CASCADE ON UPDATE CASCADE;
```

Inserción de un módulo nuevo
----------------------------

```sql
INSERT INTO `app_modules` (`module_id`, `module_name`, `module_url`, `module_icon`, `module_key`, `module_description`, `default_order`, `status`) VALUES (NULL, 'Clientes', 'Clientes', 'clientes_modulo', 'C', '1', '1', '1');
```

Inserción de un elemento nuevo
------------------------------

```sql
INSERT INTO `app_elements` (`element_id`, `element_key`, `element_name`, `singular_name`, `element_gender`, `unique_element`, `module_id`, `method_name`, `is_creatable`, `is_updatable`, `is_deletable`, `table_name`) VALUES (NULL, 'clientes', 'Clientes', 'cliente', 'M', '0', '3', 'DetalleCliente', '1', '1', '1', 'clientes');
```

Inserción de un método nuevo
----------------------------

```sql
INSERT INTO `app_methods` (`method_id`, `module_id`, `method_name`, `method_url`, `method_icon`, `method_description`, `default_order`, `element_id`, `permissions`, `status`) VALUES (NULL, '3', 'Lista de clientes', 'ListaClientes', 'clientes_metodo', '', '1', '7', '8', '1');
```
