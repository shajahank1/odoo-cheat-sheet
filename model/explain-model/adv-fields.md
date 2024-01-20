# Advanced Fields
Odoo provides several advanced field types that allow for more complex behavior and interactions than basic field types. These advanced fields enable developers to implement sophisticated features in their Odoo applications. Here are some of the key advanced field types:

### 1. Many2one
Description: Creates a many-to-one relationship between two models.
Usage: Used to link a record to another record in a different model.
Example:
```
partner_id = fields.Many2one('res.partner', string='Customer')
```
### 2. One2many
Description: Represents a one-to-many relationship. It's a virtual field, meaning it doesn't correspond to a column in the database.
Usage: Used to link a record to multiple records in another model.
Example:
```
order_line_ids = fields.One2many('sale.order.line', 'order_id', string='Order Lines')
```
### 3. Many2many
Description: Creates a many-to-many relationship between two models.
Usage: Used to link multiple records in a model to multiple records in another model.
Example:
```
tag_ids = fields.Many2many('blog.tag', string='Tags')
```
### 4. Binary
Description: Stores binary data, such as files or images.
Usage: Commonly used for storing documents, pictures, etc.
Example:
```
document = fields.Binary('Document')
```
### 5. Reference
Description: A reference field can hold a reference to any record of any model, similar to a pointer in programming languages.
Usage: Useful for dynamic references where the exact model may vary.
Example:
```
ref = fields.Reference([('res.partner', 'Partner'), ('product.product', 'Product')], string='Reference')
```
### 6. Monetary
Description: Used for monetary values and always associated with a currency.
Usage: Ideal for financial and accounting data.
Example:
```
price = fields.Monetary('Price', 'currency_id')
```
### 7. Html
Description: Stores HTML code and renders it in the view.
Usage: Used for rich text content that requires HTML formatting.
Example:
```
description = fields.Html('Description')
```
### 8. Selection
Description: Provides a set of selectable options (stored as string or integer in the database).
Usage: Used when the value should be selected from a predefined list.
Example:
```
state = fields.Selection([
    ('draft', 'Draft'), 
    ('confirmed', 'Confirmed'), 
    ('done', 'Done')
], default='draft', string='Status')
```
> These advanced fields enable developers to build rich and interactive applications in Odoo, catering to complex business requirements and data relationships. Proper understanding and usage of these fields are crucial for effective Odoo development.

--- 
# eg Relational, Computed, and Related Fields
 Let's explore examples of Relational, Computed, and Related fields in Odoo using a hypothetical scenario.

## Scenario
Assume we have two models: library.book and library.author. Each book has an author, and each author can have multiple books. We also want to compute and display the total number of books written by an author on the author's form view.

### 1. Relational Fields: Many2one and One2many
library.book model (Book Model):

```
from odoo import models, fields

class LibraryBook(models.Model):
    _name = 'library.book'
    _description = 'Library Book'

    name = fields.Char('Title', required=True)
    author_id = fields.Many2one('library.author', string='Author')
```
library.author model (Author Model):

```
class LibraryAuthor(models.Model):
    _name = 'library.author'
    _description = 'Library Author'

    name = fields.Char('Name', required=True)
    book_ids = fields.One2many('library.book', 'author_id', string='Books')
```
### 2. Computed Field: Count of Books by an Author
In the library.author model, we add a computed field to count the books:

```
from odoo import api

class LibraryAuthor(models.Model):
    # ... existing code ...

    book_count = fields.Integer(string='Number of Books', compute='_compute_book_count')

    @api.depends('book_ids')
    def _compute_book_count(self):
        for author in self:
            author.book_count = len(author.book_ids)
```
### 3. Related Field: Display Author's Name in Book Model
To display the author's name directly in the library.book model without needing to access the author_id relation:

```
class LibraryBook(models.Model):
    # ... existing code ...

    author_name = fields.Char(string='Author Name', related='author_id.name', readonly=True)
```
XML Views for Models
Form View for library.author:

```
<record id="library_author_form_view" model="ir.ui.view">
    <field name="name">library.author.form</field>
    <field name="model">library.author</field>
    <field name="arch" type="xml">
        <form>
            <sheet>
                <group>
                    <field name="name"/>
                    <field name="book_count" readonly="1"/>
                </group>
                <notebook>
                    <page string="Books">
                        <field name="book_ids"/>
                    </page>
                </notebook>
            </sheet>
        </form>
    </field>
</record>
```
Form View for library.book:
```
<record id="library_book_form_view" model="ir.ui.view">
    <field name="name">library.book.form</field>
    <field name="model">library.book</field>
    <field name="arch" type="xml">
        <form>
            <sheet>
                <group>
                    <field name="name"/>
                    <field name="author_id"/>
                    <field name="author_name" readonly="1"/>
                </group>
            </sheet>
        </form>
    </field>
</record>
```
In these examples, we've utilized relational fields (Many2one and One2many) to establish a relationship between books and authors, a computed field (book_count) to count the number of books per author, and a related field (author_name) to display the author's name directly in the book records. This setup demonstrates how these fields can be effectively used to model complex data relationships and computations in Odoo.


