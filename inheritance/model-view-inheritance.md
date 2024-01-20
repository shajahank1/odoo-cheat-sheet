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
