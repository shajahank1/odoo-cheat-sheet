# Menu
> In Odoo, there are two ways we can define menus and actions, but they have slightly different purposes and implementations:

### Using <menuitem> Tag:
- This approach is used to define menu items directly within the XML data file (usually data.xml or similar).
- It allows you to define the menu structure and associate actions directly with menu items.
- You can include submenus (nested menus) and specify actions for each menu item.
```
<menuitem id="evora_care_main_menu" name="Evora Care">
    <menuitem id="menu_financial_summary" name="Financial Report" sequence="100">
        <menuitem id="menu_fin_report_summary" name="Summary" action="action_financial_summary"/>
        <menuitem id="menu_fin_report_month" name="Month wise" action="action_financial_summary_month"/>
        <menuitem id="menu_fin_report_year" name="Year wise" action="action_financial_summary_year"/>
    </menuitem>
</menuitem>
```
### Using <record> Tag:
- This approach is used to define menu items and other records (such as actions, views, etc.) within the Python code or XML data file.
- It allows you to define records separately and then reference them where needed.
- You can define menu items using the ir.ui.menu model and specify the parent menu, action, and sequence.
```
<record id="menu_evoracare_project" model="ir.ui.menu">
    <field name="name">Projects</field>
    <field name="parent_id" ref="evora_care_main_menu"/>
    <field name="action" ref="project.open_view_project_all"/>
    <field name="sequence">10</field>
</record>
```
### Key Differences:

- The <menuitem> approach directly defines menu items and actions within the XML file, providing a more concise way to structure menus and actions.
- The <record> approach separates the definition of menu items into individual records, which can be referenced elsewhere in the code. This allows for more modular and reusable code.
- The <record> approach is often preferred when you have a larger number of menu items or when you want to manage menu definitions separately from other parts of the module.
> Both approaches are valid and useful, and the choice between them depends on factors such as personal preference, the complexity of your menu structure, and your specific requirements for modularity and reusability.
