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


# Prototypal Inheritance
> Prototypal inheritance, often referred to as prototype-based inheritance, is a key concept in Odoo development, particularly when it comes to creating new models that are similar to existing ones but need their own separate database tables. This is different from classical inheritance, where the new class shares the same table as the parent class.

### Purpose of Prototypal Inheritance
The main use of prototypal inheritance in Odoo is to:

- Create a new model that inherits behavior and structure from an existing model: This is useful when you want the new model to have all the fields and methods of the base model but need it to be stored separately in the database.
- Facilitate modular extensions: Allows developers to extend core functionality in modular ways without altering the base model's database schema.
- Enhance maintainability and clarity: Keeping separate models in separate tables can make the system more organized and easier to maintain, especially when dealing with complex data structures.
### How to Implement Prototypal Inheritance
> To implement prototypal inheritance in Odoo, you define a new model that specifies both _name (indicating it is a new model with its own table) and _inherit (linking it to the existing model from which it should inherit fields and methods).

### Steps for Implementing Prototypal Inheritance
- Define the New Model: Create a Python class in your module that specifies both _name (the technical name of the new model) and _inherit (the model it should inherit from).
- Use Inherited Fields: Automatically, all fields and methods from the parent model are available in the new model.
- Add Additional Fields: Optionally, you can add more fields specific to the new model.
- Example: Creating a Specialized Partner Model
> Suppose you want to create a specialized version of the res.partner model for VIP customers that includes additional fields and functionalities but should be stored in a separate database table.

#### Step 1: Define the New Model
Create a new Python file or edit an existing one in your custom module:

```
from odoo import models, fields

class ResPartnerVIP(models.Model):
    _name = 'res.partner.vip'  # This is the new model's technical name.
    _inherit = 'res.partner'   # This inherits from res.partner.

    vip_level = fields.Selection([
        ('gold', 'Gold'),
        ('platinum', 'Platinum'),
        ('diamond', 'Diamond')
    ], string="VIP Level", help="The VIP level of the partner.")
```
In this example:

> The new model res.partner.vip will have its own table in the database but inherit all fields and methods from res.partner.
A new field vip_level is added to differentiate VIP customers.
#### Step 2: Modify the View (if necessary)
To make the new field visible in the UI, you would modify the view accordingly, either by extending an existing view or creating a new one.

```
<odoo>
    <data>
        <record id="res_partner_vip_form_view" model="ir.ui.view">
            <field name="name">res.partner.vip.form</field>
            <field name="model">res.partner.vip</field>
            <field name="arch" type="xml">
                <form>
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="vip_level"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>
    </data>
</odoo>
```
### Best Practices for Prototypal Inheritance
- Clear Model Definition: Ensure that the purpose of the new model is clear and that it justifies the need for a separate table.
- Avoid Redundancy: Do not duplicate fields or methods that are inherited unless necessary.
- Document Differences: Clearly document how the new model differs from its parent to aid in maintenance and future development.
> Prototypal inheritance is a powerful feature in Odoo that enables robust data architecture and application design, allowing for specific customization while maintaining a clean and efficient database structure.
