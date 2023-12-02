Plantillas de código PHP
========================

Plantilla básica de una clase
--------------------------

```php
<?php
class NOMBRE_CLASE extends Controller
{
  /**
   * Constructor de la clase
   * 
   * Inicializa la propiedad module con el nombre de la clase
   */
  public function __construct()
  {
    parent::__construct();
    $this->module = get_class($this);
    $this->view->data["module"] = $this->module;
  }

  /**
   * Vista principal
   * 
   * Muestra un menú con las principales actividades que se pueden realizar en el módulo.
   * 
   * @return void
   */
  public function index()
  {
    $this->session_required("html", $this->module);
    $this->view->standard_menu();
    $this->view->data["nav"] = $this->view->render("main/nav", true);
    $module = appModulesModel::findBy("module_url", $this->module);
    $this->view->data["title"] = _($module->getModuleName());
    $this->view->data["methods"] = availableMethodsModel::where("user_id", Session::get("user_id"))
    ->where("module_id", $module->getModuleId())
    ->orderBy("method_order")->getAllArray();
    $this->view->data["content"] = $this->view->render("generic_menu", true);
    $this->view->render("main");
  }

  # OTROS MÉTODOS DE LA CLASE A CONTINUACIÓN:
}
?>
```

Vista para lista de elementos
-----------------------------

```php
/**
 * Usuarios
 * 
 * Muestra una lista de usuarios del sistema
 * 
 * @return void
 */
public function METODO_VISTA_LISTA()
{
  $this->check_permissions("read", "NOMBRE_ELEMENTO");
  $this->view->data["title"] = _("TITULO_DE_VISTA");
  $this->view->standard_list();
  $this->view->data["nav"] = $this->view->render("main/nav", true);
  $this->view->data["print_title"] = _("TITULO_DE_IMPRESION");
  $this->view->data["print_header"] = $this->view->render("print_header", true);
  $this->view->data["content"] = $this->view->render("RUTA_DE_VISTA", true);
  $this->view->render('main');
}
```

Carga de lista de elementos y descarga de Excel
-----------------------------------------------

```php
/**
 * Cargar tabla de usuarios
 * 
 * Devuelve, en formato JSON o en un archivo Excel, la lista de usuarios.
 * @param string $response El modo de respuesta (JSON o Excel)
 * 
 * @return void
 */
public function METODO_CARGA_LISTA($response = "JSON")
{
  $this->check_permissions("read", "NOMBRE_ELEMENTO");
  $data = Array();
  $items = userDataModel::getAllArray();
  foreach($items as $key => $item)
  {
    # Tareas adicionales después de la consulta
  }
  $data["content"] = $items;
  if($response == "Excel")
  {
    $data["title"] = _("TITULO_EXCEL");
    $data["headers"] = Array(... ENCABEZADOS ...);
    $data["fields"] = Array(... CAMPOS ...);
    excel::create_from_table($data, "NOMBRE_ARCHIVO_" . Date("YmdHis") . ".xlsx");
  }
  else
  {
    $this->json($data);
  }
}
```

Vista de detalles
-----------------

```php
/**
 * Detalles de un elemento
 * 
 * Muestra una hoja con los datos del un elemento.
 * @param int $LLAVE_PRIMARIA ID del elemento a consultar
 * 
 * @return void
 */
public function DetalleElemento($elemento_id)
{
  $this->check_permissions("read", "NOMBRE_ELEMENTO");
  $this->view->data["title"] = _("TITULO_VISTA");
  $this->view->standard_details();
  $this->view->data["nav"] = $this->view->render("main/nav", true);
  $this->view->data["content_id"] = "METODO_CARGA_DETALLES(SIN loader)";
  $this->view->data["content"] = $this->view->render("content_loader", true);
  $this->view->render('main');
}
```

Carga de detalles
-----------------

```php
/**
 * Carga de detalles de un elemento
 * 
 * Muestra una hoja con los detalles de un elemento. Este método puede ser invocado por a través
 * de DetallesElemento (embedded) y directamente para ser mostrado en un jAlert (standalone); por
 * ejemplo, para el elemento con ID 1, se podría visitar:
 * - Modulo/DetalleElemento/1/ (embedded)
 * - Modulo/detalle_elemento_loader/1/standalone/ (standalone)
 * @param int $LLAVE_PRIMARIA ID del elemento
 * @param string $mode Modo en que se mostrará la vista
 * 
 * @return void
 */
public function detalle_elemento_loader($user_id = "", $mode = "embedded")
{
  $this->check_permissions("read", "NOMBRE_ELEMENTO", $mode);
  if(empty($user_id))
  {
    $user_id = $_POST["id"];
  }
  $user = MODELO_ELEMENTO::findBy("user_id", $user_id)->toArray();
  $this->view->data = array_merge($this->view->data, $user);
  
  $this->userActions($user);
  $this->view->data["print_title"] = _("TITULO_DE_IMPRESION");
  $this->view->data["print_header"] = $this->view->render("print_header", true);
  if($mode == "standalone")
  {
    $this->view->data["title"] = _("TITULO_DE_VENTANA");
    $this->view->standard_details();
    $this->view->add("styles", "css", Array(
      'styles/standalone.css'
    ));
    $this->view->restrict[] = "embedded";
    $this->view->data["content"] = $this->view->render('RUTA_DE_VISTA', true);
    $this->view->render('clean_main');
  }
  else
  {
    $this->view->render("RUTA_DE_VISTA");
  }
}
```

Vista de formulario para inserción
----------------------------------

```php
/**
 * Nuevo elemento
 * 
 * Muestra un formulario que permite registrar nuevos elementos en el sistema, y asignarles
 * permisos a diferentes módulos. Un usuario autorizado para registrar elementos, sólo puede
 * otorgar permisos que le han sido otorgados.
 * 
 * @return void
 */
public function NUEVO_ELEMENTO()
{
  $this->check_permissions("create", "users");
  $this->view->data["title"] = _("TITULO_VISTA");
  $this->view->standard_form();
  $this->view->data["nav"] = $this->view->render("main/nav", true);
  $this->view->restrict[] = "edition";
  $this->view->data["content"] = $this->view->render("RUTA_DE_VISTA", true);
  $this->view->render('main');
}
```

Vista de formulario para edición
--------------------------------

```php
/**
 * Editar un elemento
 * 
 * Permite editar los datos de un elemento.
 * 
 * @return void
 */
public function EDITAR_ELEMENTO($LLAVE_PRIMARIA)
{
  $this->check_permissions("update", "users");
  $this->view->data["title"] = _("TITULO_DE_VISTA");
  $this->view->standard_form();
  $this->view->data["nav"] = $this->view->render("main/nav", true);
  $this->view->restrict[] = "creation";
  $this->view->data["content"] = $this->view->render("RUTA_DE_VISTA", true);
  $this->view->render('main');
}
```

Carga de datos a editar
----------------------

```php
/**
 * Carga de datos de formulario.
 * 
 * Imprime, en formato JSON, los datos iniciales para la carga de formularios.
 * 
 * @return void
 */
public function load_form_data()
{
  $data = Array();
  if($_POST["method"] == "METODO_EDITAR")
  {
    $data["update"] = usersModel::find($_POST["id"])->toArray();
  }
  $this->json($data);
}
```

Guardar un elemento
-------------------

```php
/**
 * Guardar un elemento
 * 
 * Crea o actualiza un elemento en la base de datos.
 * 
 * @return void
 */
public function METODO_GUARDAR()
{
  $this->check_permissions(empty($_POST["LLAVE_PRIMARIA"]) ? "create" : "update", "NOMBRE_ELEMENTO");
  $data = Array("success" => false);

  # Validaciones

  $elemento = MODELO_ELEMENTO::find($_POST["LLAVE_PRIMARIA"]);
  $elemento->set(Array(
    # CAMPOS
    ));
  $elemento->save();
  if(!empty($_POST["LLAVE_PRIMARIA"]))
  {
    $this->setUserLog("update", "NOMBRE_ELEMENTO", $user->getLLAVE_PRIMARIA());
  }
  else
  {
    $this->setUserLog("create", "NOMBRE_ELEMENTO", $user->getLLAVE_PRIMARIA());
  }
  $data["success"] = true;
  $data["title"] = _("Success");
  $data["message"] = _("Changes have been saved");
  $data["theme"] = "green";
  $data["reload_after"] = true;
  $this->json($data);
}
```

Eliminar un elemento
--------------------

```php
/**
 * Eliminar usuario
 * 
 * Elimina un usuario e imprime la respuesta en formato JSON.
 * 
 * @return void
 */
public function METODO_ELIMINAR()
{
  $this->check_permissions("delete", "NOMBRE_ELEMENTO");
  $data = Array("deleted" => false);
  if(empty($_POST["id"]))
  {
    $this->json($data);
    return;
  }
  $elemento = MODELO_ELEMENTO::find($_POST["id"]);
  $affected = $elemento->delete();
  $data["deleted"] = $affected > 0;
  $this->setUserLog("delete", "users", $user->getLLAVE_PRIMARIA());
  $this->json($data);
}
```

Clase ORM de la tabla de clientes
---------------------------------

```php
<?php
/**
 * Model for clientes
 * 
 * Generated by BlackPHP
 */

class clientesModel
{
  use ORM;

  /** @var int $cliente_id Llave primaria */
  private $cliente_id;

  /** @var string $nombre Nombre del cliente */
  private $nombre;

  /** @var string $direccion Dirección del cliente */
  private $direccion;

  /** @var string $telefono Teléfono del cliente */
  private $telefono;

  /** @var string $fecha_nacimiento Fecha de nacimiento */
  private $fecha_nacimiento;

  /** @var int $creation_user - */
  private $creation_user;

  /** @var string $creation_time - */
  private $creation_time;

  /** @var int $edition_user - */
  private $edition_user;

  /** @var string $edition_time - */
  private $edition_time;

  /** @var int $status - */
  private $status;


  /** @var string $_table_name Nombre de la tabla */
  private static $_table_name = "clientes";

  /** @var string $_table_type Tipo de tabla */
  private static $_table_type = "BASE TABLE";

  /** @var string $_primary_key Llave primaria */
  private static $_primary_key = "cliente_id";

  /** @var bool $_timestamps La tabla usa marcas de tiempo para la inserción y edición de datos */
  private static $_timestamps = true;

  /** @var bool $_soft_delete La tabla soporta borrado suave */
  private static $_soft_delete = true;

  /** @var int|null $_deleted_status Valor a asignar en caso de borrado suave. */
  private static $_deleted_status = 0;

  /**
   * Constructor de la clase
   * 
   * Se inicializan las propiedades de la clase.
   * @param bool $default Determina si se utilizan, o no, los valores por defecto
   * definidos en la base de datos.
   **/
  public function __construct($default = true)
  {
    if($default)
    {
      $this->status = 1;
    }
  }

  public function getClienteId()
  {
    return $this->cliente_id;
  }

  public function setClienteId($value)
  {
    $this->cliente_id = $value === null ? null : (int)$value;
  }

  public function getNombre()
  {
    return $this->nombre;
  }

  public function setNombre($value)
  {
    self::validateStringSize($value, 128);
    $this->nombre = $value === null ? null : (string)$value;
  }

  public function getDireccion()
  {
    return $this->direccion;
  }

  public function setDireccion($value)
  {
    self::validateStringSize($value, 255);
    $this->direccion = $value === null ? null : (string)$value;
  }

  public function getTelefono()
  {
    return $this->telefono;
  }

  public function setTelefono($value)
  {
    self::validateStringSize($value, 16);
    $this->telefono = $value === null ? null : (string)$value;
  }

  public function getFechaNacimiento()
  {
    return $this->fecha_nacimiento;
  }

  public function setFechaNacimiento($value)
  {
    $this->fecha_nacimiento = $value === null ? null : (string)$value;
  }

  public function getCreationUser()
  {
    return $this->creation_user;
  }

  public function setCreationUser($value)
  {
    $this->creation_user = $value === null ? null : (int)$value;
  }

  public function getCreationTime()
  {
    return $this->creation_time;
  }

  public function setCreationTime($value)
  {
    $this->creation_time = $value === null ? null : (string)$value;
  }

  public function getEditionUser()
  {
    return $this->edition_user;
  }

  public function setEditionUser($value)
  {
    $this->edition_user = $value === null ? null : (int)$value;
  }

  public function getEditionTime()
  {
    return $this->edition_time;
  }

  public function setEditionTime($value)
  {
    $this->edition_time = $value === null ? null : (string)$value;
  }

  public function getStatus()
  {
    return $this->status;
  }

  public function setStatus($value)
  {
    $this->status = $value === null ? null : (int)$value;
  }
}
?>
```
