## Inheritance
> In Odoo, inheritance is a fundamental concept that allows developers to extend and modify the behavior of existing models and views without altering the original code. This approach is central to Odoo’s modular architecture, making it easy to customize and upgrade the system.

### Types of Inheritance in Odoo
> Odoo supports several types of inheritance, each suited for different purposes:

- Classical Inheritance (Model Extension): Extending an existing model by adding new fields, methods, or overriding existing ones.
- Prototypal Inheritance (Model Inheritance): Creating a new model that inherits all fields and behaviors of another but is stored as a separate table in the database.
- Delegation Inheritance: Creating a new model that links back to another model for extending functionalities.
- View Inheritance: Modifying or extending existing views.
### 1. Classical Inheritance: Extending Models
To extend an existing model, you inherit from it and add or override fields and methods. This doesn't create a new database table but adds new columns to the existing table of the parent model.

Example: Extending the res.partner model to add a loyalty points field.

```
from odoo import models, fields

class Partner(models.Model):
    _inherit = 'res.partner'

    loyalty_points = fields.Integer("Loyalty Points")
```
In this example, Partner class inherits from res.partner and adds a new field loyalty_points. All res.partner records can now have loyalty points associated with them.

### 2. Prototypal Inheritance: Creating New Models
When you need a new model that has all characteristics of an existing one but should have its own database table, use prototypal inheritance.

Example: Creating a res.customer model that inherits from res.partner.

```
from odoo import models, fields

class Customer(models.Model):
    _name = 'res.customer'
    _inherit = 'res.partner'
    _description = "Customer with specific attributes"

    customer_grade = fields.Selection([
        ('silver', 'Silver'),
        ('gold', 'Gold'),
        ('platinum', 'Platinum')
    ], "Customer Grade")
```
Here, res.customer will have its own table but inherits all fields and behaviors from res.partner, plus it adds a customer_grade field.

### 3. Delegation Inheritance
This type of inheritance is used when you want to extend a model but keep the base data in its original model.

Example: Extending res.users with additional settings without cluttering the original users table.

```
from odoo import models, fields

class UserSettings(models.Model):
    _name = 'res.user.settings'
    _inherits = {'res.users': 'user_id'}

    user_id = fields.Many2one('res.users', required=True, ondelete='cascade', auto_join=True, index=True)
    theme = fields.Char("UI Theme")
```
### 4. View Inheritance
View inheritance allows you to modify or extend existing views. You specify the base view you are extending and use XPath expressions to locate and modify the XML structure.

Example: Adding a field to the existing res.partner form view.

```
<odoo>
    <data>
        <record id="view_res_partner_form_inherit" model="ir.ui.view">
            <field name="name">res.partner.form.inherit</field>
            <field name="model">res.partner</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="arch" type="xml">
                <xpath expr="//field[@name='email']" position="after">
                    <field name="loyalty_points"/>
                </xpath>
            </field>
        </record>
    </data>
</odoo>
```
In this example, the loyalty_points field is added right after the email field in the res.partner form view.

### Conclusion
> Inheritance in Odoo provides a robust mechanism for extending and customizing functionality without modifying the original codebase, ensuring that customizations are maintainable and that the core application can be updated without overwriting custom features. Each type of inheritance serves different needs, from simple field additions to creating entirely new models based on existing ones.






