# inheritance
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

