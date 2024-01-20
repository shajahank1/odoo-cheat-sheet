# Inheritance
Inheritance in Odoo is a powerful feature that allows you to extend or modify the functionality of existing models and views. It is crucial for customizing and building upon the standard features provided by Odoo. There are primarily two types of inheritance in Odoo: classical inheritance and prototypical (delegation) inheritance.

## 1. Classical Inheritance
Classical inheritance in Odoo is similar to traditional object-oriented inheritance. It is used to extend the functionality of existing models or to create new models based on existing ones.

Extending Models: You can add new fields, methods, or override existing methods in an existing model.
Creating New Models: You can create a new model that inherits from an existing one, carrying over all fields and behaviors.
Example: Extending a model to add a new field.

```
from odoo import models, fields

class LibraryBook(models.Model):
    _inherit = 'library.book'

    isbn = fields.Char('ISBN')
```
- 
In this example, LibraryBook extends the existing library.book model by adding a new field isbn.

## 2. Prototypical (Delegation) Inheritance
Prototypical inheritance, or delegation inheritance, is used in Odoo to modify the view of a model. This type of inheritance allows you to add, remove, or alter the XML elements of a view.

Modifying Views: You can inherit a view to modify its structure, such as adding new fields, hiding existing fields, or changing the layout.
Example: Inheriting a form view to add a new field.

```
<record id="view_library_book_form_inherit" model="ir.ui.view">
    <field name="name">library.book.form.inherit</field>
    <field name="model">library.book</field>
    <field name="inherit_id" ref="library.view_library_book_form"/>
    <field name="arch" type="xml">
        <xpath expr="//field[@name='author_id']" position="after">
            <field name="isbn"/>
        </xpath>
    </field>
</record>

```
In this example, the form view of library.book is extended to include the isbn field after the author_id field.

### Inheritance Best Practices
- Use Classical Inheritance for Business Logic: Extend models to add or modify business logic and data structure.
- Use Prototypical Inheritance for UI Modifications: Inherit views to customize the user interface without altering the underlying business logic.
- Prefer Composition Over Inheritance: Where possible, use delegation (like Many2one fields) to encapsulate functionality instead of creating deep inheritance hierarchies.
- Consistency and Maintainability: When extending models or views, aim to maintain consistency with Odoo's core structure and naming conventions to ensure maintainability.
> In summary, Odoo's inheritance mechanisms provide a flexible and powerful way to customize and extend the functionality of both the backend models and the frontend views. Understanding and utilizing these inheritance techniques is key to effective Odoo development and customization.


# Classical Inheritance 
Classical inheritance in Odoo, often referred to as model inheritance, is a mechanism that allows you to extend or modify the functionality of existing Odoo models (classes). This is similar to classical inheritance in object-oriented programming, where a new class (child class) inherits attributes and behaviors (methods) from an existing class (parent class).

In Odoo, there are primarily two ways to achieve classical inheritance:

## 1. Inheritance by Extension (_inherit)
This type of inheritance is used to add new features to an existing model or to modify its behavior. The existing model remains unchanged, but you extend its functionality.

- Usage: When you want to add new fields, methods, or override existing methods in an existing model without creating a new database table.
- Key Attribute: _inherit
**Example:**
```
from odoo import models, fields

class CustomPartner(models.Model):
    _inherit = 'res.partner'

    new_field = fields.Char("New Field")

    def new_method(self):
        # method logic
        pass
```
> In this example, CustomPartner extends res.partner by adding new_field and new_method. It doesn't create a new database table but adds to the res.partner model.
## 2. Inheritance by Prototype (_inherits)
This is a form of delegation inheritance where the new model inherits fields and behaviors from another model but still gets its own database table.

Usage: When you need a new model with its own table but want to include all fields and methods of another model.
Key Attribute: _inherits
Example:
```
class LibraryMember(models.Model):
    _name = 'library.member'
    _inherits = {'res.partner': 'partner_id'}

    partner_id = fields.Many2one('res.partner', required=True, ondelete="cascade")
    library_card_number = fields.Char("Library Card Number")
```
> In this example, LibraryMember is a new model that has its own table but includes all fields of res.partner. The partner_id field is a Many2one relation, creating a link between library.member and res.partner.
Key Differences and Considerations
### Inheritance by Extension:

- Does not create a new database table.
- Ideal for adding features to existing models.
- Maintains a single record for each entity.
### Inheritance by Prototype:

- Creates a new database table.
- Useful for creating distinct entities that share characteristics.
- Maintains separate records linked through relational fields.
> Classical inheritance in Odoo provides a powerful way to build upon existing models, either by extending their capabilities or by creating new models that share common features. This approach promotes reusability and maintainability in Odoo development.

--- 
# Prototypical (Delegation) Inheritance
Prototypical (delegation) inheritance in Odoo, also known as prototype inheritance, is a mechanism that allows you to create a new model which "inherits" or delegates part of its behavior and structure to another existing model. This form of inheritance creates a composition relationship between the new model and the existing model.

### Key Characteristics
- New Table Creation: Unlike classical inheritance (_inherit), prototypical inheritance (_inherits) creates a new database table for the new model.

- Shared Fields: The new model contains all fields of the delegated (parent) model, in addition to any new fields you define.

- Linked via a Many2one Field: The new model is linked to the existing model through a Many2one field. This relationship is essential for accessing and managing the inherited data.

### Implementation
- Define the New Model: When defining the new model, you use _inherits to specify the model you're inheriting from.
- Create a Many2one Field: A Many2one field is created in the new model, linking it to the inherited model.
**Example**
Suppose you have a model res.partner and you want to create a new model library.member that extends res.partner.

```
from odoo import models, fields

class LibraryMember(models.Model):
    _name = 'library.member'
    _inherits = {'res.partner': 'partner_id'}

    partner_id = fields.Many2one('res.partner', ondelete='cascade', required=True, auto_join=True)
    library_card_number = fields.Char("Library Card Number")
```
### In this example:

- LibraryMember is a new model that inherits from res.partner.
- It has its own database table but includes all fields from res.partner.
- partner_id is a Many2one field linking library.member to res.partner.
- Any record in library.member will be linked to a corresponding record in res.partner.
### Usage and Considerations
- Use Case: Ideal when you need a new model with distinct records but want to reuse the structure and logic of an existing model.

- Data Integrity: The ondelete='cascade' option ensures that if the record in the parent model (res.partner) is deleted, the corresponding record in library.member is also deleted.

- Separate Records: Each library.member record will have a corresponding res.partner record. This is suitable for scenarios where you need to model a "is-a" relationship (a library member is a partner) but also need to maintain separate records.

- Accessing Fields: In the new model, you can access and modify fields from the parent model as if they were directly in the new model.

> Prototypical inheritance is a powerful feature in Odoo that provides flexibility in model design, allowing for the extension of existing models while maintaining separate database entities. This approach is useful for modeling real-world relationships and business scenarios in Odoo applications.
