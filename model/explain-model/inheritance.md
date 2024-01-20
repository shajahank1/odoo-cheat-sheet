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




