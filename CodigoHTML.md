Plantillas de código HTML
=========================

El presente documento incluye bloques de código básicos para crear las vistas principales de un CRUD. En el código también encontrará elementos a renderizar, como se detalla a continuación:

> **\{\{ variable \}\}**: El framework sustituirá esta expresión por el valor de una variable PHP dentro del arreglo ```$this->data```.  
> **\[[ arreglo ]]** y **\[[/ arreglo ]]**: Todo lo que se encuentre dentro de estas dos expresiones se sustituirá por los valores dentro de un arreglo asociativo de dos dimensiones.  
> **_(Expresión)**: Todo lo que se encuentre en la expresión será traducido por gettext.  
> **\<!-- comentario -->** y **\<!-- /comentario -->**: El contenido de este bloque se ocultará en determinadas condiciones.

Vista para lista de elementos
-----------------------------

```html
<div class="list_options">
  <div class="action_icons">
    <a href="#" class="filter_button phone_inline">
      <img src="public/images/filter.png" alt="_(Filter)">
    </a>
    <a href="/\{\{ module \}\}/METODO_NUEVO_REGISTRO/">
      <img src="public/images/add.png" alt="_(New user)">
    </a>
    <a href="#" class="download_link" data-url="/\{\{ module \}\}/METODO_DE_CARGA_DE_LISTA_loader/Excel/">
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
\{\{ print_header \}\}
<div class="content_viewer">
  <table class="data_viewer" data-source="content" id="METODO_DE_CARGA_DE_LISTA">
    <thead>
      <tr>
        <th>Campo 1</th>
        <th>Campo 2</th>
        <th class="pc_cell">Campo 3</th>
        <th class="pc_cell">Campo 4</th>
      </tr>
    </thead>
    <tbody>
      <tr class="template" data-id="\{\{llave_primaria\}\}" data-alert="\{\{ module \}\}/METODO_CARGA_DETALLES_loader/\{\{llave_primaria\}\}/standalone/" data-title="TITULO_DE_VENTANA">
        <td>\{\{CAMPO_1\}\}</td>
        <td>\{\{CAMPO_2\}\}</td>
        <td class="pc_cell">\{\{CAMPO_3\}\}</td>
        <td class="pc_cell">\{\{CAMPO_4\}\}</td>
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

Formulario
----------

Formulario básico con botones guardar y cancelar.

```html
<div class="content_viewer">
  <div class="form_content medium_form">
    <form action="METODO_PARA_GUARDAR" data-reset="\{\{ module \}\}/METODO_LISTA">
      <div class="form_title">
        _(TITULO_DE_VENTANA)
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
          <button class="red_button delete_button" type="button" data-url="\{\{ module \}\}/METODO_PARA_ELIMINAR/" data-next="\{\{ module \}\}/METODO_LISTA/">
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
<input type="hidden" name="NOMBRE_DEL_CAMPO" class="update_input">
```

Entrada de texto
----------------

```html
<div class="form_entry">
  <label>ETIQUETA_DEL_CAMPO:</label>
  <input type="text" name="NOMBRE_DEL_CAMPO" maxlength="64" class="update_input" required>
</div>
```

Entrada de texto largo
----------------------

```html
<div class="form_entry">
  <label>ETIQUETA_DEL_CAMPO:</label>
  <textarea name="NOMBRE_DEL_CAMPO"  maxlength="256" class="notes update_input"></textarea>
</div>
```

Entrada de selector
-------------------

Entrada con selector que se convertirá automáticamente en select2.

```html
<div class="form_entry">
  <label>ETIQUETA_DEL_CAMPO</label>
  <select class="data_selector update_input" name="NOMBRE_DEL_CAMPO" data-source="ARREGLO_FUENTE" data-search="none" data-placeholder="_(Select)"></select>
</div>
```

Vista de detalles
-----------------

```html
\{\{ print_header \}\}
<div class="details_content">
  <!-- embedded -->
  <div class="details_title no_print">
    TITULO_DE_VENTANA
    <a class="close_details" href="#"><img src="public/images/close.png"></a>
  </div>
  <!-- /embedded -->
  <div class="details_body">
    .
    . CUERPO_DE_DETALLES
    .
  </div>
  <div class="details_users">
    <!-- created -->
    _(Created by): \{\{ cr_user_name \}\}, _(timeon) \{\{ cr_time \}\}<br>
    <!-- /created -->
    <!-- edited -->
    _(Last edition by): \{\{ ed_user_name \}\}, _(timeon) \{\{ ed_time \}\}
    <!-- /edited -->
  </div>
  <div class="details_foot no_print">
    <span class="button_group">
      <!-- no_self -->
      <button class="delete_button red_button" data-url="\{\{ module \}\}/METODO_PARA_ELIMINAR" data-next="\{\{ module \}\}/METODO_LISTA">
        <img src="public/images/outline/delete.png" class="x-small_icon">
        _(Delete)
      </button>
      <!-- /no_self -->
      <button class="blue_button link_button" data-href="\{\{ module \}\}/METODO_PARA_EDITAR/\{\{ LLAVE_PRIMARIA \}\}/">
        <img src="public/images/outline/edit.png" class="x-small_icon">
        _(Edit)
      </button>
      <button class="blue_button print_button" data-print=".details_body">
        <img src="public/images/outline/print.png" class="x-small_icon">
        _(Print)
      </button>
      <button class="blue_button pdf_button" data-content=".details_body">
        <img src="public/images/outline/pdf.png" class="x-small_icon">
        PDF
      </button>
    </span>
  </div>
</div>
```

Tabla de detalles
-----------------

```html
    <table class="data_sheet_table">
      <tr>
        <td class="data_sheet_field">ETIQUETA_DEL_CAMPO</td>
        <td>\{\{ NOMBRE_DEL_CAMPO \}\}</td>
      </tr>
      .
      . OTROS_CAMPOS
      .
    </table>
```

Tabla anidada en vista de detalles
----------------------------------

```html
    <p>ETIQUETA_DE_LA_TABLA:</p>
    <table class="show_details_table">
      <thead>
        <th class="pc_cell">No.</th>
        <th>ENCABEZADO_1</th>
        <th>ENCABEZADO_2</th>
        .
        .
        .
      </thead>
      <tbody>
        [[ INDICE_DE_ARREGLO ]]
        <tr>
          <td class="right pc_cell">\{\{ num \}\}</td>
          <td>\{\{ NOMBRE_CAMPO_1 \}\}</td>
          <td>\{\{ NOMBRE_CAMPO_2 \}\}</td>
          .
          .
          .
        </tr>[[/ INDICE_DE_ARREGLO ]]
      </tbody>
    </table>
```
