Plantillas de código PHP
========================

Plantilla básica de una clase
--------------------------

```php
class NombreClase extends Controller
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
public function Users()
{
  $this->check_permissions("read", "users");
  $this->view->data["title"] = _("Users");
  $this->view->standard_list();
  $this->view->data["nav"] = $this->view->render("main/nav", true);
  $this->view->data["print_title"] = _("Users");
  $this->view->data["print_header"] = $this->view->render("print_header", true);
  $this->view->data["content"] = $this->view->render("settings/user_list", true);
  $this->view->render('main');
}
```

Carga de lista de elementos y descarga de Excel
-----------------------------------------------

Vista de detalles
-----------------

Carga de detalles
-----------------

Vista de formulario
-------------------

Carga de elementos previos
--------------------------

Carga de datos a editar
----------------------

Guardar un elemento
-------------------

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
public function delete_user()
{
  $this->check_permissions("delete", "users");
  $data = Array("deleted" => false);
  if(empty($_POST["id"]))
  {
    $this->json($data);
    return;
  }
  $user = usersModel::find($_POST["id"]);
  $affected = $user->delete();
  $data["deleted"] = $affected > 0;
  $this->setUserLog("delete", "users", $user->getUserId());
  $this->json($data);
}
```
