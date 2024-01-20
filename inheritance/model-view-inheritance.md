# Models and Views Inheritance
In Odoo, both models and views can be extended or inherited to add or modify functionality. This is a key feature for customizing and enhancing the behavior of existing applications without altering the core code. Let's explore how inheritance works for both models and views.

## Models Inheritance
In Odoo, there are two main types of model inheritance: Classical Inheritance and Prototypal (Delegation) Inheritance.

Classical Inheritance (_inherit)

Used to add new fields or methods to an existing model, or to modify existing methods.
Does not create a new database table; instead, it extends the existing model's table.
Example:
```
from odoo import models, fields

class CustomPartner(models.Model):
    _inherit = 'res.partner'

    custom_field = fields.Char("Custom Field")
```
> In this example, a new field custom_field is added to the existing res.partner model.
Prototypal (Delegation) Inheritance (_inherits)

Used to create a new model that 'inherits' fields from another model.
Creates a new database table and establishes a 'parent-child' relationship between the new model and the existing one.
Example:
```
class ExtendedProduct(models.Model):
    _name = 'extended.product'
    _inherits = {'product.product': 'product_id'}

    product_id = fields.Many2one('product.product', required=True, ondelete="cascade")
    extra_feature = fields.Char("Extra Feature")
```
> Here, extended.product extends product.product, adding an extra_feature field.
## Views Inheritance
Odoo also allows for the extension and modification of views. This is particularly useful for adding or altering fields, groups, pages, or other elements in the UI.

### Inheriting and Extending Views
> You can inherit a view to add or modify elements without changing the original view.
Done using XPath expressions to locate and alter specific parts of the view.
Example:
```
<record id="view_partner_form_inherit" model="ir.ui.view">
    <field name="name">res.partner.form.inherit</field>
    <field name="inherit_id" ref="base.view_partner_form"/>
    <field name="arch" type="xml">
        <xpath expr="//field[@name='street']" position="after">
            <field name="custom_field"/>
        </xpath>
    </field>
</record>
```
This XML snippet extends the res.partner form view to add a custom_field after the street field.
### Best Practices
- Classical Inheritance: Ideal for extending functionality without needing separate data storage.
- Prototypal Inheritance: Best when a distinct model is needed, but with shared characteristics of an existing model.
- View Inheritance: Useful for customizing the UI without modifying original view definitions. Always use XPath carefully to target the correct elements.
- Model and view inheritance in Odoo provide a powerful way to customize and extend applications, allowing developers to build upon existing structures and functionalities efficiently.
----
# Exercise: Model and View Inheritance
Certainly! Let's walk through an exercise to demonstrate model and view inheritance in Odoo. We'll use a practical scenario where we extend an existing model and its view.

## Scenario
Suppose we want to extend the res.partner model (which represents partners/contacts in Odoo) to add a new field, is_preferred_customer. We also want to display this field on the partner form view.

### Step 1: Model Inheritance
First, let's create a new module (e.g., custom_partner_extension). In this module, we'll extend the res.partner model to add the is_preferred_customer field.

models/custom_partner.py:

```
from odoo import models, fields

class CustomPartner(models.Model):
    _inherit = 'res.partner'

    is_preferred_customer = fields.Boolean("Is a Preferred Customer?", default=False)
```
> This Python file extends res.partner by adding a new boolean field is_preferred_customer.

### Step 2: View Inheritance
Now, we'll extend the form view of res.partner to include the new field.

views/custom_partner_view.xml:

```
<odoo>
    <data>
        <record id="view_partner_form_inherit" model="ir.ui.view">
            <field name="name">res.partner.form.inherit</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="arch" type="xml">
                <xpath expr="//group[@name='sales_purchases']" position="inside">
                    <field name="is_preferred_customer"/>
                </xpath>
            </field>
        </record>
    </data>
</odoo>
```
> In this XML, we are extending the existing partner form view. We use XPath to locate the <group> with the name sales_purchases and add our new field inside this group.

### Step 3: Module Manifest and Initialization
Define the module manifest (__manifest__.py) and initialize the module by including Python files and view XML files.

__manifest__.py:

```
{
    'name': 'Custom Partner Extension',
    'version': '1.0',
    'depends': ['base'],
    'data': [
        'views/custom_partner_view.xml',
    ],
    'demo': [],
}
```
__init__.py:

```
from . import models
```
models/__init__.py:

```
from . import custom_partner
```
### Conclusion
> With these steps, we have successfully extended an existing model (res.partner) by adding a new field, and we've modified its form view to display this new field. This exercise demonstrates the power and flexibility of model and view inheritance in Odoo, allowing for effective customization of existing functionalities.
