Ejercicio
=========

Descripción
-----------

En el proyecto realizado, se requiere que el registro de clientes también capture el número de cédula de cada cliente.

Pasos
-----

- Aregar un campo ```cedula``` en la tabla. (Usando el comando, o desde PHPMyAdmin).

```sql
ALTER TABLE `clientes` ADD `cedula` VARCHAR(16) NOT NULL AFTER `nombre`;
```

- Agregar la propiedad en la clase ORM.

```php
  private $cedula;
```

- Agregar los respectivos métodos get y set en la clase del ORM

```php
  public function getCedula()
  {
    return $this->cedula;
  }

  public function setCedula($value)
  {
    self::validateStringSize($value, 16);
    $this->cedula = $value === null ? null : (string)$value;
  }
```

- Agregar un campo en el formulario HTML

```html
<div class="form_entry">
  <label>C&eacute;dula:</label>
  <input type="text" name="cedula" maxlength="16" class="update_input" required>
</div>
```

- Agregar el campo en el arreglo PHP a la hora de guardar el cliente

```php
  $user->set(Array(
      // Otros campos
      "cedula" => $_POST["cedula"],
      // Otros campos
    ));
```

- Agregar el campo en la tabla de clientes en la sección thead

```html
        <th class="pc_cell">_(Last login)</th>
```

- Agregar el campo en la tabla en la sección tbody

```html
        <td>\{\{cedula\}\}</td>
```

- Agregar el campo en el Excel

```php
    $data["headers"] = Array(..., "Cedula", ...);
    $data["fields"] = Array(..., "cedula", ...);
```

- Agregar el campo en la vista de detalles

```html
      <tr>
        <td class="data_sheet_field">C&eacute;dula</td>
        <td>\{\{ cedula \}\}</td>
      </tr>
```
