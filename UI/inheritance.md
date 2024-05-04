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

# Delegation inheritance
> Delegation inheritance in Odoo is a powerful feature that allows one model to "borrow" or delegate some of its functionality to another model. This is done through a specific kind of inheritance where a model delegates the storage of some of its fields to another model, effectively using those fields as if they were its own. It combines the advantages of having a separate database table for a new model while still leveraging the functionality and fields from another existing model.

### Purpose of Delegation Inheritance
#### Delegation inheritance is used to:

- Split large models: Useful for breaking down large models into more manageable sub-models while maintaining a logical connection between them.
- Reuse existing functionality: Allows models to reuse and extend the functionality of other models without complex dependencies or duplicating code.
- Maintain clean architecture: Helps in keeping the database schema cleaner and more organized by segregating data across related models.
### How to Implement Delegation Inheritance
> Delegation inheritance is implemented by defining a new model that includes a special Many2one field linking back to the original model. This field uses the _inherits dictionary to specify which model is being delegated and which field acts as the link.

### Steps for Implementing Delegation Inheritance
- Define the Delegate Model: Create a new model that includes a Many2one field pointing back to the original model.
- Specify the _inherits Mapping: In the class definition, set the _inherits attribute to map the linking field to the original model.
- Add Additional Fields: Optionally, you can add more fields to the delegate model.
### Example: Creating a Customer Profile Model
Let's say you want to create a detailed customer profile that extends the res.partner model without modifying the original res.partner table.

#### Step 1: Define the Delegate Model
You would define a new model in Python that inherits from res.partner using delegation:

```
from odoo import models, fields

class CustomerProfile(models.Model):
    _name = 'customer.profile'
    _description = 'Customer Profile'
    _inherits = {'res.partner': 'partner_id'}

    partner_id = fields.Many2one('res.partner', required=True, ondelete='cascade', auto_join=True, index=True)
    loyalty_points = fields.Integer("Loyalty Points")
    preferences = fields.Text("Preferences")
```
##### In this example:

> The new model customer.profile uses _inherits to indicate that it inherits from res.partner via the partner_id field.
Fields like loyalty_points and preferences are specific to the customer.profile model.
partner_id is a mandatory link back to the res.partner model, ensuring that each customer profile is directly associated with a specific partner.
#### Step 2: Use the Delegate Model in Views
When defining views for the customer.profile, you can include fields from both res.partner and customer.profile.

```
<odoo>
    <data>
        <record id="customer_profile_form_view" model="ir.ui.view">
            <field name="name">customer.profile.form</field>
            <field name="model">customer.profile</field>
            <field name="arch" type="xml">
                <form string="Customer Profile">
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="email"/>
                            <field name="loyalty_points"/>
                            <field name="preferences"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>
    </data>
</odoo>
```
> In this form view, name and email are inherited fields from res.partner, seamlessly integrated into the customer profile form.

### Best Practices for Delegation Inheritance
- Use for Logical Extensions: Apply delegation inheritance when extending a model logically fits the application's data structure.
- Maintain Clear Links: Ensure the Many2one linking field is always properly set and handled to avoid orphan records.
- Document Model Relationships: Clearly document how models are related through delegation to aid understanding and maintenance.
> Delegation inheritance is a sophisticated feature that allows for extensive customization and extension of existing models in Odoo, facilitating powerful data architecture setups tailored to specific business needs.



#  View Inheritance
> View inheritance in Odoo is a mechanism that allows developers to modify, extend, or enhance existing views without needing to recreate the entire view from scratch. This feature is crucial for adding customizations to standard views provided by Odoo or other installed modules, enabling developers to adjust the UI to better fit specific business requirements while maintaining upgrade compatibility.

### Purpose of View Inheritance
> View inheritance serves several key purposes:

- Customization: Modify existing UIs to add new fields, hide existing elements, change styles, or restructure layouts.
- Extension: Add new functionalities or components to existing views, such as buttons, fields, groups, or even entirely new sections.
- Maintenance and Upgradability: By inheriting views rather than modifying them directly, custom changes remain separate from the base application code, making it easier to manage updates and module upgrades.
### How to Implement View Inheritance
> View inheritance in Odoo is achieved by creating a new view that specifies which existing view it extends using the inherit_id attribute. Developers use XPath expressions to pinpoint where changes should be made within the existing structure.

### Steps for Implementing View Inheritance
- Identify the Target View: Determine which view you want to modify. This could be a core Odoo view or a view from another module.
- Create a New Inheritance View: Define an XML record for the new view in your module. Use the inherit_id to specify the view you are extending.
- Modify the View Using XPath: Use XPath expressions to locate and specify how the view should be modified or extended.
### Example: Extending a Partner Form View
Suppose you want to add a custom field to the standard res.partner form view. You'll first need to add the field to the res.partner model, and then create an inheritance view to include it in the UI.

#### Step 1: Extend the Model

```
from odoo import models, fields

class ResPartner(models.Model):
    _inherit = 'res.partner'

    customer_level = fields.Selection([
        ('standard', 'Standard'),
        ('premium', 'Premium')
    ], string="Customer Level")
```
#### Step 2: Create the Inheritance View
You'll then define an XML record that extends the existing partner form view:

```
<odoo>
    <data>
        <record id="res_partner_form_inherit" model="ir.ui.view">
            <field name="name">res.partner.form.inherit</field>
            <field name="model">res.partner</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="arch" type="xml">
                <xpath expr="//group[@name='top']" position="inside">
                    <field name="customer_level"/>
                </xpath>
            </field>
        </record>
    </data>
</odoo>
```
##### In this XML:

- The inherit_id points to the existing partner form view.
- XPath is used to insert the customer_level field inside the top group of the form.
#### Tips for Effective View Inheritance
- Use Correct XPath: Ensure your XPath expressions are correctly targeting the elements you wish to modify. Incorrect XPath can lead to unexpected UI issues or failures to apply your customizations.
- Positioning Attributes: Understand how to use position attributes in XPath (before, after, inside, replace) to precisely control where new elements are placed or how existing elements are modified.
- Testing: Thoroughly test inherited views to ensure that they behave as expected across all relevant scenarios.
> View inheritance is a core feature of Odoo's extensibility, allowing developers to build on top of existing functionality in a maintainable and upgrade-friendly manner. This makes it an essential tool for tailoring Odoo installations to the specific needs of businesses without compromising the integrity or upgradability of the underlying system.



