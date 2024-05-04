## inherit_id
> In Odoo, inherit_id is a crucial attribute used in XML view definitions when you need to extend or modify an existing view. This approach allows developers to add to, remove from, or alter parts of the user interface without needing to rewrite the entire view. It's fundamental to Odoo's modular and extensible architecture.

### Purpose of inherit_id
The primary purpose of using inherit_id is to facilitate view inheritance, enabling developers to:

- Enhance or Customize Views: Add new fields, groups, or other elements to an existing view.
- Modify Existing Layouts: Change attributes or positioning of elements already present in the base view.
- Maintain Upgradability: Make modifications in a way that does not interfere with updates to the base application or module.
### How to Implement inherit_id
> To use inherit_id in a view, you must reference the external ID of the existing view that you intend to extend or modify. This ID can typically be found in the original module's XML files or through the Odoo user interface in developer mode.

### Steps for Implementing View Inheritance
- Identify the Base View: Determine the view you want to extend. This could be a form, tree, search, or any other type of view provided by Odoo or developed in other modules.
- Create an Inherited View: Define a new XML record in your module's data file that uses the inherit_id to reference the base view.
- Modify the View: Use Xpath expressions to target and modify specific parts of the view structure.
#### Example
> Suppose you want to add a new field to the partner form provided by the base contacts module in Odoo. You would proceed as follows:

#### Step 1: Define the Field in Python (if it's a new field)
First, ensure the field exists in the model. If it's a new field, you would add it to the res.partner model either by inheritance in your custom module or directly if you're modifying the base module.

```
from odoo import models, fields

class ResPartner(models.Model):
    _inherit = 'res.partner'

    custom_field = fields.Char(string="Custom Field")
```
#### Step 2: Inherit the Form View Using inherit_id
Create an XML file within your module that extends the res.partner form view using inherit_id.

```
<odoo>
    <data>
        <record id="res_partner_form_inherit" model="ir.ui.view">
            <field name="name">res.partner.form.inherit</field>
            <field name="model">res.partner</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="arch" type="xml">
                <xpath expr="//field[@name='function']" position="after">
                    <field name="custom_field"/>
                </xpath>
            </field>
        </record>
    </data>
</odoo>
```
**In this XML:**

- inherit_id: Specifies the base view to inherit from. Here, ref="base.view_partner_form" indicates that this view extends the view_partner_form from the base module (base).
- xpath: Targets where in the base view the changes should occur. The expr="//field[@name='function']" targets the function field in the base view.
- position="after": Indicates that the new field should be added right after the targeted field.
### Usage Tips
- Understand Xpath: Knowing how to use Xpath effectively is critical as it allows you to pinpoint exactly where in the base view your changes should be applied.
- Check Module Dependencies: Ensure that your module depends on any other module whose views you are inheriting. This dependency should be specified in your module's __manifest__.py file.
- Use Developer Mode: Odoo's developer mode can help you find the external IDs of views and fields, which is invaluable for setting inherit_id and writing Xpath expressions.
> Using inherit_id in this way allows you to customize Odoo's functionality to fit specific business needs without losing the ability to upgrade Odoo or other base modules, which is a key advantage of Odoo's modular design.
