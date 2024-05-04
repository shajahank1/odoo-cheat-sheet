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


# Classical inheritance
> Classical inheritance, often referred to as model extension in Odoo, is a powerful mechanism that allows developers to add new fields, methods, or modify existing ones within an existing model without needing to redefine the entire model. This approach leverages Python's inheritance capabilities and is extensively used in Odoo to enhance and customize the functionality of built-in models.

### Purpose of Classical Inheritance
Classical inheritance is used in Odoo to:

- Extend existing models with additional fields or functionality.
- Override methods to change the default behavior of standard operations.
- Maintain compatibility with upstream updates, ensuring that customizations are preserved even when the base application is updated.
### How to Implement Classical Inheritance
> To implement classical inheritance in Odoo, you inherit from an existing model and specify the _inherit attribute with the name of the model you want to extend. Here's the step-by-step process:

#### 1. Identify the Model to Extend
First, determine which existing Odoo model's functionality you need to extend or modify.

#### 2. Create a New Python Class that Inherits from the Existing Model
In your custom module, define a new class that inherits from the existing model. Use the _inherit attribute to specify the model you are extending.

#### 3. Add or Override Fields and Methods
In your new class, add new fields, or override existing methods to modify their behavior.

### Example: Extending the res.partner Model
Suppose you want to add a customer loyalty points field to the res.partner model, which is used for managing customers and other types of partners in Odoo.

#### Step 1: Create a Python File in Your Module
In your custom module, create a new Python file or edit an existing one, and define your new class.

#### Step 2: Define the Class
Here’s how you might define your class to add a loyalty_points field:

```
from odoo import models, fields

class Partner(models.Model):
    _inherit = 'res.partner'

    loyalty_points = fields.Integer("Loyalty Points", help="Stores customer loyalty points.")
```
In this example:

_inherit = 'res.partner' tells Odoo that you are extending the res.partner model.
loyalty_points is a new integer field added to the res.partner model.
#### Step 3: Update the View (optional)
If you want the new field to be visible in the UI, you must modify the view. This can be done using view inheritance:

```
<odoo>
    <data>
        <record id="view_partner_form_inherit" model="ir.ui.view">
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
This XML snippet extends the existing partner form view to include the loyalty_points field right after the email field.

### Best Practices for Classical Inheritance
- Minimize Overrides: Only override methods when necessary. Excessive use of overrides can lead to maintenance issues and conflicts with other modules.
- Use Super: When overriding methods, make sure to call super() to keep the base class's behavior unless explicitly intending to replace it.
- Document Changes: Always document your changes and the reasons behind method overrides or field additions to aid future developers and maintainers of the module.
> Classical inheritance is one of the most common ways to customize and extend Odoo functionality, allowing for a high degree of integration and compatibility with the core system and other modules.


