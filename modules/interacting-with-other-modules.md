# Interacting with other modules
https://www.odoo.com/documentation/17.0/developer/tutorials/getting_started/14_other_module.html

Interacting with other modules in Odoo is a common requirement, especially when you want to extend or enhance the functionality of existing modules or when your module needs to work in tandem with others. This interaction can be achieved in several ways, such as inheriting from existing models, calling methods from other modules, or using module data.

## 1. Inheriting from Existing Models
One of the most common ways to interact with other modules is by inheriting from their models. This allows you to extend or modify the behavior of existing models.

**Example**:
Suppose you want to extend the res.partner model from the Contacts module to add additional fields.

```
from odoo import fields, models

class ResPartner(models.Model):
    _inherit = 'res.partner'

    custom_field = fields.Char(string='Custom Field')
```
### XML View Customization:

```
<record id="view_partner_form_inherit" model="ir.ui.view">
    <field name="name">res.partner.form.inherit</field>
    <field name="model">res.partner</field>
    <field name="inherit_id" ref="base.view_partner_form"/>
    <field name="arch" type="xml">
        <xpath expr="//field[@name='function']" position="after">
            <field name="custom_field"/>
        </xpath>
    </field>
</record>
```
> In this example, the res.partner model is extended to add a custom_field. The form view is also extended to include this new field.

## 2. Calling Methods from Other Modules
You can also interact with other modules by calling their methods.

**Example**:
If you want to use a method from the Sales module in your custom module:

```
sale_order = self.env['sale.order'].browse(sale_order_id)
sale_order.some_method_from_sale_module()
```
In this example, you retrieve a sale.order record and call a method defined in the Sales module.

## 3. Using Data from Other Modules
Another way to interact is by using data (like records or configuration settings) defined in other modules.

**Example:**
If your module needs to use product data from the Inventory module:

```
products = self.env['product.product'].search([('type', '=', 'product')])
```
Here, you search for products defined in the Inventory module from your custom module.

## 4. Dependency Management
When interacting with other modules, ensure your module correctly defines its dependencies.

__manifest__.py Example:
```
'depends': ['base', 'sale']
```
In the module manifest, list all the modules your module depends on.

### Implementing the Interaction
#### To successfully interact with other modules:
- Understand the External Module: Study the models, methods, and data structures of the module you want to interact with.
- Define Dependencies: Clearly state the dependencies in your module's __manifest__.py.
- Extend or Inherit Properly: Use inheritance to extend models or views. Override methods if necessary.
- Respect the Module's Logic: When calling methods from other modules, ensure that you understand their side effects and interactions.
- Test Thoroughly: Especially when interacting with critical modules like Sales, Inventory, or Accounting, thorough testing is crucial to avoid disruptions.
> By following these practices, you can effectively and safely interact with other modules in Odoo, allowing you to build more complex and integrated business applications.
