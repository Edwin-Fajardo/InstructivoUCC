Plantillas de código HTML
=========================

Vista para lista de elementos
-----------------------------

```html
<div class="list_options">
  <div class="action_icons">
    <a href="#" class="filter_button phone_inline">
      <img src="public/images/filter.png" alt="_(Filter)">
    </a>
    <a href="/{{ module }}/NewUser/">
      <img src="public/images/add.png" alt="_(New user)">
    </a>
    <a href="#" class="download_link" data-url="/{{ module }}/user_table_loader/Excel/">
      <img src="public/images/excel.png" alt="_(Export to Excel)">
    </a>
    <a href="#" class="print_button" data-print=".data_viewer">
      <img src="public/images/printer.png" alt="_(Print)">
    </a>
  </div>
  <div class="action_filter">
    <input class="data_search" type="text" placeholder="_(Type a word for search)">
  </div>
</div>
{{ print_header }}
<div class="content_viewer">
  <table class="data_viewer" data-source="content" id="user_table">
    <thead>
      <tr>
        <th>_(User)</th>
        <th>_(Complete name)</th>
        <th class="pc_cell">_(Role)</th>
        <th class="pc_cell">_(Last login)</th>
      </tr>
    </thead>
    <tbody>
      <tr class="template" data-id="{{user_id}}" data-alert="{{ module }}/user_details_loader/{{user_id}}/standalone/" data-title="_(User details)">
        <td>{{nickname}}</td>
        <td>{{user_name}}</td>
        <td class="pc_cell">{{role_name}}</td>
        <td class="pc_cell">{{last_login}}</td>
      </tr>
    </tbody>
    <tfoot>

    </tfoot>
  </table>
  <div class="loading_data">
    <img src="public/images/loading.gif" class="medium_icon">
    <span>_(Loading)...</span>
  </div>
  <div class="loading_error">
    <img src="public/images/error.png" class="medium_icon">
    <span>
      _(Error!)<br>
      _(Failed to load page).
    </span>
  </div>
  <div class="empty_table_message">_(No records found!)</div>
</div>
```

Filtros
-------

Formulario
----------

Formulario básico con botones guardar y cancelar.

```html
<div class="content_viewer">
  <div class="form_content medium_form">
    <form action="save_user" data-reset="{{ module }}/Users">
      <div class="form_title">
        _(Edit user)
        <a class="close_form" href="#"><img src="public/images/close.png"></a>
      </div>
      <div class="form_body">
        .
        .Cuerpo del formulario
        .
      </div>
      <div class="form_foot">
        <span class="button_group">
          <!-- edition -->
          <button class="red_button delete_button" type="button" data-url="{{ module }}/delete_user/" data-next="{{ module }}/Users/">
            <img src="public/images/outline/delete.png" class="x-small_icon">
            _(Delete)
          </button>
          <!-- /edition -->
          <button class="blue_button" type="submit">
            <img src="public/images/outline/save.png" class="x-small_icon">
            _(Save)
          </button>
          <button class="grey_button cancel_button" type="reset">
            <img src="public/images/outline/cancel.png" class="x-small_icon">
            _(Cancel)
          </button>
        </span>
      </div>
    </form>
    <div class="sending">
      <img src="public/images/loading.gif" class="small_icon">
      <span>_(Saving)...</span>
    </div>
    <div class="error">
      <img src="public/images/error.png" class="small_icon">
      <span>_(Failed to save)</span>
    </div>
    <div class="success">
      <img src="public/images/success.png" class="small_icon">
      <span>_(Success)</span>
    </div>
  </div>
</div>
```

Entrada oculta para edición
---------------------------

```html
<input type="hidden" name="nombre_del_campo" class="update_input">
```

Entrada de texto
----------------

```html
<div class="form_entry">
  <label>_(Complete name):</label>
  <input type="text" name="user_name" maxlength="64" class="update_input" required>
</div>
```

Entrada de texto largo
----------------------

```html
<div class="form_entry">
  <label>_(Address):</label>
  <textarea name="customer_address"  maxlength="256" class="notes update_input"></textarea>
</div>
```

Entrada de selector
-------------------

Entrada con selector que se convertirá automáticamente en select2.

```html
<div class="form_entry">
  <label>_(Role)</label>
  <select class="data_selector update_input" name="role_id" data-source="roles" data-search="none" data-placeholder="_(Select)"></select>
</div>
```

Vista de detalles
Tabla de detalles
Tabla anidada en vista de detalles
