
> The syntax <field name="active" eval="False"/> is used within an XML record in Odoo to set the active field of a record to False upon its creation or modification. This is particularly useful in data files where you are defining or modifying records and need to specify that they should be inactive by default.

### Context and Usage
In Odoo, the active field is commonly used as a way to 'soft delete' or hide records without actually removing them from the database. Setting active to False makes the record invisible in most views and searches, which by default filter on active=True.

### Example of Implementation
Suppose you're defining a new action or a specific configuration in a module, and you want it to be inactive initially. You can use <field name="active" eval="False"/> in your XML data file to achieve this. Here’s an example where we might define a new product category that should not be active initially:

```
<record id="product_category_inactive" model="product.category">
    <field name="name">Inactive Category</field>
    <field name="active" eval="False"/>
</record>
```
In this example, a new product category named "Inactive Category" is created and immediately set to inactive. This means it won't show up in product category lists that don't specifically include inactive records.

### Using active in View Definitions
While the example above demonstrates using active within a record definition, it's important to note that the <field name="active" eval="False"/> line isn't typically used within view definitions directly. Views don't inherently control the active state of records—they display it. However, you might control visibility of elements in a view based on the active state, but this is handled with domain filters or attributes, not with eval.

#### Handling active in Actions
If you're defining an action (like a window action to open a view) and you want to include inactive records in the results, you would adjust the domain of the action, rather than using eval:

```
<record id="action_view_all_categories" model="ir.actions.act_window">
    <field name="name">All Categories</field>
    <field name="res_model">product.category</field>
    <field name="view_mode">tree,form</field>
    <field name="domain">[]</field>  <!-- This includes active and inactive records -->
</record>
```
### Conclusion
> Using <field name="active" eval="False"/> in XML data files is a straightforward way to set records as inactive upon their creation or updating. It's a useful feature for managing the lifecycle of records, especially in configurations or setups where certain items should not be immediately available or visible in the user interface. Remember that active fields are usually not manipulated directly in view definitions but are rather managed through model definitions and action domains.


# Active
> In Odoo, the active attribute is used within view and model definitions to manage the visibility and usability of records or views. This attribute plays a crucial role in allowing records to be effectively 'hidden' without being permanently deleted, thereby supporting data retention and control over record availability.

### Purpose of active
The active attribute serves several purposes:

- Soft Deletion: Provides a way to hide records from view without actually deleting them from the database, which can be crucial for maintaining data integrity and historical records.
- Conditional Visibility: Allows administrators or developers to control whether a record or view should be currently active and accessible to users.
- Manage Obsolescence: Useful for deactivating outdated or no longer relevant data or configurations without removing historical data.
### How active Works
> The active attribute is a boolean field commonly added to Odoo models and is often included in view definitions to determine if the view should be used. Here’s how it typically functions:

- If active is set to True, the record or view is fully operational and visible.
- If active is set to False, the record or view becomes hidden from normal operations but remains in the database.
- This behavior allows for a soft-delete functionality where records are not shown in standard search results but still exist in the database and can be accessed or reactivated if necessary.

### Implementing active in Models
Here is an example of adding an active field to a custom Odoo model:

```
from odoo import models, fields

class CustomModel(models.Model):
    _name = 'custom.model'
    name = fields.Char("Name")
    active = fields.Boolean("Active", default=True)
```
In this model, active is defined as a boolean field with a default value of True. When creating new records, they will be active by default.

### Using active in Views
To control the visibility of inactive records, Odoo typically filters them out by default. However, you can modify this behavior in action definitions or search views by specifying domain filters.

```
<record id="action_show_all_custom_models" model="ir.actions.act_window">
    <field name="name">Custom Models</field>
    <field name="res_model">custom.model</field>
    <field name="view_mode">tree,form</field>
    <field name="domain">[]</field> <!-- Shows all records, including inactive ones -->
</record>
```
> In this action, all records including those marked as inactive (active = False) are shown because the domain is explicitly empty. Normally, you might see a domain like [('active', '=', True)] to filter only active records.

### Best Practices
- Default to Active: When adding an active field to a model, it's common practice to default this field to True so that records are active upon creation unless specified otherwise.
- Filtering in Views: Be cautious about how you display inactive records. It’s usually best to filter them out for regular users but provide administrative views or actions to manage all records.
- Use in Custom Modules: When creating custom modules or business objects, consider adding an active field to control their visibility and manage their lifecycle effectively.
>The active attribute is a simple yet powerful tool for managing record visibility and lifecycle in Odoo, providing flexibility in how data is maintained and accessed within the system.
