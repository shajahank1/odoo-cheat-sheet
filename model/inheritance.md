# Inheritance
> Odoo provides robust and flexible inheritance mechanisms, crucial for extending and customizing its functionality. Understanding these mechanisms is key for effective Odoo development. There are two primary types of inheritance in Odoo: Classical Inheritance and Prototype Inheritance.

## 1. Classical Inheritance (_inherit)
Classical Inheritance is used to extend or modify an existing model and is similar to classical inheritance in object-oriented programming.

### How it Works: 
In this type of inheritance, you define a new model that sets _inherit with the name of an existing model. The new model inherits all characteristics (fields, methods) of the parent model. You can add new fields or override existing methods in the new model.

### Key Features:
Does not create a new table in the database if the _name attribute is not set; instead, it extends the existing model.
If _name is also set (different from the inherited model), it creates a new model and database table, but shares the same Python class.

### Usage and Functionalities:
- Extend Existing Models: Add new fields or methods to an existing model.
- Override Methods: Modify the behavior of existing methods in the parent model.
- Create New Models: If _name is also set, create a new model that shares the same table as the parent.
**Example**:
Extending res.partner to add a new field and override a method:

```
class ExtendedPartner(models.Model):
    _inherit = 'res.partner'

    membership_status = fields.Char('Membership Status')

    def name_get(self):
        # Customized name_get method
        # [...]
```
## 2. Prototype Inheritance (_inherits)
Prototype Inheritance, or Delegation Inheritance, embeds an existing model within a new model, creating a composition relationship.

### How it Works: 
In this type, the new model includes a pointer (Many2one field) to the inherited model. This means both models will exist independently in the database, but the new model can access all fields and methods of the inherited model. Changes in either model reflect in the other.

### Key Features:
Creates a new database table for the new model.
Establishes a composition relationship with the inherited model.

### Usage and Functionalities:
- Create a New Model: The new model has its own database table.
- Link to an Existing Model: Establishes a Many2one relationship with the inherited model.
- Independence of Records: Records in the new model are linked but independent of the inherited model.
**Example**:
Creating custom.employee that inherits hr.employee:

```
class CustomEmployee(models.Model):
    _name = 'custom.employee'
    _inherits = {'hr.employee': 'employee_id'}

    employee_id = fields.Many2one('hr.employee', required=True, ondelete="cascade")
    additional_info = fields.Text('Additional Info')
    # [...]
```
### Key Differences:
- Classical Inheritance (_inherit):
1. Extends or modifies an existing model.
2. Can add fields/methods, override methods, or create a new model linked to the parent's table.
- Prototype Inheritance (_inherits):
1. Creates a new model with its own table.
2. Establishes a linked relationship with an existing model.
3. Useful for creating extended models while maintaining independence from the parent model.
### Best Practices:
>Choose Classical Inheritance for straightforward extensions or modifications.
>Opt for Prototype Inheritance when you need a new model that includes all aspects of an existing model but also needs its own identity and table.
>Understanding and effectively using these inheritance types allows for the development of sophisticated and customizable applications within the Odoo framework.
