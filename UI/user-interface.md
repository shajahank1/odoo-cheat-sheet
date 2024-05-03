# UI
> In Odoo, the user interface (UI) plays a crucial role in providing an accessible and efficient experience for users. Odoo's UI is built using a combination of server-side Python, client-side JavaScript, and XML templates that define the layout and structure of views. Here's a breakdown of how the UI works in Odoo and an example to illustrate it:

### Components of Odoo UI
- **Views:** Views are the most critical component in Odoo's UI, defining how data is displayed. There are several types of views in Odoo:
Form Views: For editing a single record.
- **List** (or Tree) Views**: For displaying multiple records in a list.
- **Kanban Views**: Visual boards for managing tasks.
- **Graph Views**: For displaying data graphically.
- **Pivot Views:** For creating pivot tables for data analysis.
- **Calendar Views:** For displaying records in a calendar format.
- **Gantt Views:** For showing records in a timeline.
- **Menus and Actions:** Menus organize the access to different models and views. Actions are configurations that define what happens when a menu item is clicked, such as opening a specific view.
- **Widgets:** Widgets are reusable UI components that enhance fields in views, such as tags for many2many fields or date pickers for date fields.
- **QWeb Templates:** Odoo uses QWeb, an XML-based templating engine, for generating HTML. These templates are used in reports, website modules, and parts of the web client.
#### Example: Creating a Simple Form View in Odoo
To give you a practical example, here's how you would define a simple form view for a custom model model.example:

```
<odoo>
  <data>
    <!-- Action to open the form -->
    <record id="example_model_action" model="ir.actions.act_window">
      <field name="name">Example Model</field>
      <field name="res_model">model.example</field>
      <field name="view_mode">form</field>
    </record>

    <!-- Menu item to access the form -->
    <menuitem id="example_model_menu" name="Example Model"
              action="example_model_action" parent="base.menu_custom"/>

    <!-- Form view definition -->
    <record id="example_model_form_view" model="ir.ui.view">
      <field name="name">model.example.form</field>
      <field name="model">model.example</field>
      <field name="arch" type="xml">
        <form string="Example Model">
          <sheet>
            <group>
              <field name="name"/>
              <field name="description"/>
            </group>
          </sheet>
        </form>
      </field>
    </record>
  </data>
</odoo>
```
#### This XML does a few things:

Defines an action (ir.actions.act_window) to open the form view of our model.example.
Creates a menu item (menuitem) under a parent menu (e.g., base.menu_custom) that uses the defined action.
Sets up a form view (ir.ui.view) for model.example, showing fields name and description grouped together on the form.



## Generic structure
Basic views generally share the common minimal structure defined below. Placeholders are denoted in all caps.
```
<record id="ADDON.MODEL_view_TYPE" model="ir.ui.view">
  <field name="name">NAME</field>
  <field name="model">MODEL</field>
  <field name="arch" type="xml">
    <VIEW_TYPE>
      <views/>
    </VIEW_TYPE>
  </field>
</record>

```
### View types
- Form
Display and edit the data from a single record.

- List
View and edit multiple records.

- Search
Apply filters and perform searches. The results are displayed in the current list, kanban… view.

- Kanban
Display records as “cards”, configurable as a small template.

- Qweb
Templating of reporting, website…

- Graph
Visualize aggregations over a number of records or record groups.

- Pivot
Display aggregations as a pivot table.

- Calendar
Display records as events in a daily, weekly, monthly, or yearly calendar.

- Cohort Enterprise feature
Display and understand the way some data changes over a period of time.

- Gantt Enterprise feature
Display records as a Gantt chart.

- Grid Enterprise feature
Display computed information in numerical cells; are hardly configurable.

- Map Enterprise feature
Display records on a map, and the routes between them.

> The XML snippet you've provided is a typical structure for defining a view in Odoo. Each element and attribute has a specific purpose in setting up how data is presented in the Odoo user interface. Here's a detailed explanation of each part of this XML structure:  

1.  ```<record> Element:```
- id="ADDON.MODEL_view_TYPE": This is the unique identifier for this record in the XML data file. The id is used to reference this view elsewhere in the Odoo modules or modify it through inheritance. The convention typically includes the module (or addon) name, the model it affects, and the type of view.
- model="ir.ui.view": This specifies that the record being defined is a view. The ir.ui.view model manages all views within Odoo.

2. ```<field name="name">:```
- NAME: The value here is a human-readable name for the view. It is used primarily for identification in the Odoo backend and isn't usually shown to end users. It's helpful for developers when they manage views from the technical settings.

3. ```<field name="model">:```
- MODEL: This specifies the database model that the view will be used for. For example, res.partner for partners or sale.order for sales orders. This tells Odoo which model's records are to be displayed or manipulated using this view.

4. ```<field name="arch" type="xml">: ```
- This field contains the architecture of the view, defining its structure and layout in XML format. The type="xml" attribute specifies that the content of the field is XML.
- Inside the <arch> element, you define the actual structure of the UI using one or more specific view types.

#### ```<VIEW_TYPE>:```
> This is a placeholder for the actual type of the view, such as ``` <form>, <tree>, <kanban>, <pivot>, <graph>, or <calendar>. ``` Each view type has its own set of sub-elements and attributes that can be used to configure the display of data.
- ```<form>:``` Used for displaying and editing single records.
- ```<tree>:``` Used for listing records in a tabular format.
- ```<kanban>: ```Provides a card-based interface suitable for managing tasks or other items in a workflow.
- ``` <pivot> ``` and
- ``` <graph>: ```These are used for displaying data in a pivot table or graphical format respectively.
- ``` <calendar>:``` Displays records on a calendar.

#### ```<views/>:```
- This element is typically used in action definitions to embed multiple views. However, its presence alone in a view definition (<VIEW_TYPE>) does not have any functional impact unless specifically handled by custom modules or used in a context where multiple embedded views might be defined.
- This XML structure sets the foundation for how data will be presented in the Odoo interface, and can be extended or inherited to modify existing views or add new functionalities.
