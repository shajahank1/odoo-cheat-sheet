# UI
> In Odoo, the user interface (UI) plays a crucial role in providing an accessible and efficient experience for users. Odoo's UI is built using a combination of server-side Python, client-side JavaScript, and XML templates that define the layout and structure of views. Here's a breakdown of how the UI works in Odoo and an example to illustrate it:

### Components of Odoo UI
- Views: Views are the most critical component in Odoo's UI, defining how data is displayed. There are several types of views in Odoo:
Form Views: For editing a single record.
- List (or Tree) Views: For displaying multiple records in a list.
- Kanban Views: Visual boards for managing tasks.
- Graph Views: For displaying data graphically.
- Pivot Views: For creating pivot tables for data analysis.
- Calendar Views: For displaying records in a calendar format.
- Gantt Views: For showing records in a timeline.
- Menus and Actions: Menus organize the access to different models and views. Actions are configurations that define what happens when a menu item is clicked, such as opening a specific view.
- Widgets: Widgets are reusable UI components that enhance fields in views, such as tags for many2many fields or date pickers for date fields.
- QWeb Templates: Odoo uses QWeb, an XML-based templating engine, for generating HTML. These templates are used in reports, website modules, and parts of the web client.
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
