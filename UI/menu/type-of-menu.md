# Types of Menu
> In Odoo, there are several types of menus used to organize and navigate the user interface. These menus serve different purposes and provide users with access to various functionalities within the system. Here are the main types of menus in Odoo:

## Top-Level Menus:
Top-level menus are the main navigation options displayed at the top of the Odoo interface.
They typically represent major functional areas or modules within the system, such as Sales, Purchases, Inventory, Accounting, CRM, etc.
Clicking on a top-level menu item reveals submenus or specific actions related to that functional area.
## Submenus:
Submenus are menus that appear below a top-level menu item when it is hovered over or clicked.
They provide access to more specific functionalities or views related to the parent menu item.
Submenus can be hierarchical, with multiple levels of nesting to organize related functionalities.
## Contextual Menus:
Contextual menus are menus that appear in response to a specific context or user action.
They provide relevant actions or options based on the current context, such as when right-clicking on a record in a list view or form view.
Contextual menus typically include options for editing, deleting, duplicating, or performing other actions on the selected record.
## Action Menus:
Action menus are menus that appear in response to clicking on a button or link within a view.
They provide options for performing specific actions related to the current view or context.
Action menus can include options such as saving, discarding changes, printing, exporting, or performing custom actions defined by the developer.
## Favorites Menu:
The Favorites menu allows users to bookmark frequently used views or records for quick access.
Users can add items to their Favorites menu by clicking on the star icon next to a view or record.
The Favorites menu provides a convenient way for users to access their most commonly used functionalities without navigating through menus.
## Search Menus:
Search menus provide options for filtering and searching records within a view.
They typically include filters, search fields, and other options for refining the displayed data based on specific criteria.
Search menus are commonly found in list views, allowing users to quickly find and access the records they need.
These are the main types of menus in Odoo that help users navigate the system, access functionalities, and perform actions efficiently. Each type of menu serves a specific purpose and contributes to the overall usability and user experience of the Odoo interface.


## explain each one and how to implement each one with eg

### Top-Level Menus:
- Explanation: Top-level menus are the main navigation options displayed at the top of the Odoo interface. They represent major functional areas or modules within the system.
- Example: A top-level menu for the Sales module might include options like "Quotations", "Sales Orders", "Customers", etc.
- Implementation: Define top-level menus using <menuitem> tags in XML files. Set the parent attribute to False to indicate that these are top-level menus.
```
<menuitem id="menu_sales" name="Sales" parent="False"/>
```
###  Submenus:
- Explanation: Submenus appear below a top-level menu item when it is hovered over or clicked. They provide access to more specific functionalities related to the parent menu item.
- Example: A submenu under "Sales" might include options like "Quotations", "Sales Orders", etc.
- Implementation: Define submenus using <menuitem> tags in XML files and set the parent attribute to the ID of the parent menu.
```
<menuitem id="menu_sales_quotations" name="Quotations" parent="menu_sales"/>
```
- Explanation: Contextual menus appear in response to a specific context or user action. They provide relevant actions or options based on the current context.
- Example: Right-clicking on a record in a list view might show a contextual menu with options like "Edit", "Delete", etc.
- Implementation: Contextual menus are usually handled by the Odoo framework and are automatically generated based on the context of the user action.
### Action Menus:
- Explanation: Action menus appear in response to clicking on a button or link within a view. They provide options for performing specific actions related to the current view or context.
- Example: A form view might have an action menu with options like "Save", "Discard", "Print", etc.
- Implementation: Define action menus within the view definition using <button> or <field> tags with appropriate attributes.
```
<button name="action_save" string="Save" type="object" class="btn-primary"/>
```
### Favorites Menu:
- Explanation: The Favorites menu allows users to bookmark frequently used views or records for quick access.
- Example: Users can add commonly used customers to their Favorites menu for quick access to their details.
- Implementation: Favorites functionality is provided by the Odoo framework and is available out of the box in views that support it, such as list views and form views.
### Search Menus:
- Explanation: Search menus provide options for filtering and searching records within a view. They include filters, search fields, and other options for refining displayed data based on specific criteria.
- Example: A list view might have search menus with filters for different product categories or date ranges.
- Implementation: Define search menus within the view definition using <filter> tags with appropriate attributes.
```
<filter string="Product Category" name="filter_product_category" domain="[('product_category_id', '=', active_id)]"/>
```
> These are the main types of menus in Odoo, along with examples and implementation details for each. Depending on your requirements, you can use one or more of these menu types to create a user-friendly and efficient navigation experience in your Odoo modules.
