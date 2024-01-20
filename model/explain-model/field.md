# Fields
Fields in Odoo are a fundamental part of models, representing the various properties or attributes of a business object. Each field in an Odoo model corresponds to a column in a database table. Understanding the different types of fields and their usage is crucial for effective Odoo development.

## Types of Fields
### Basic Fields:
- Char: A string field, used for short text.
- Text: A longer text field without a length limit.
- Boolean: A true/false field.
- Integer: Stores whole numbers.
- Float: Stores decimal numbers, with optional precision.
### Date and Time Fields:

- Date: Stores a date.
- Datetime: Stores date and time.
### Relational Fields:

- Many2one: A many-to-one relationship. Links to another model (e.g., a customer to a sales order).
- One2many: A one-to-many relationship. Inverse of a Many2one field (e.g., all sales orders of a customer).
- Many2many: A many-to-many relationship. Links multiple records from one model to multiple records of another model.
### Special Fields:

- Selection: A dropdown list with a set of options.
- Binary: Used to store binary data like files or images.
- Reference: A reference to any record of any model.
### Computed Fields:

Computed fields have their value calculated on the fly, usually based on other field values. They can be stored in the database or calculated each time they are accessed.
### Example of Defining Fields
Here's an example of an Odoo model with various types of fields:

```
from odoo import models, fields

class LibraryBook(models.Model):
    _name = 'library.book'
    _description = 'Library Book'

    # Basic Fields
    name = fields.Char('Title', required=True)
    description = fields.Text('Description')
    active = fields.Boolean('Active?', default=True)
    num_pages = fields.Integer('Number of Pages')
    price = fields.Float('Price')

    # Date and Time Fields
    date_published = fields.Date('Published Date')
    last_borrowed_on = fields.Datetime('Last Borrowed On')

    # Relational Fields
    author_id = fields.Many2one('library.author', string='Author')
    borrower_ids = fields.Many2many('res.partner', string='Borrowers')
    order_ids = fields.One2many('library.order', 'book_id', string='Orders')

    # Special Fields
    state = fields.Selection([
        ('available', 'Available'),
        ('borrowed', 'Borrowed'),
        ('lost', 'Lost')
    ], default='available')

    # Computed Field
    is_available = fields.Boolean(compute='_compute_is_available')

    def _compute_is_available(self):
        for record in self:
            record.is_available = record.state == 'available'
```
In this example, LibraryBook has various fields including basic types like Char and Float, relational types like Many2one, and a computed field is_available.

### Usage and Best Practices
- Field Attributes: Fields can have various attributes like string (field label), required (mandatory field), readonly (non-editable), help (tooltip text), etc.
- Computed Fields: For computed fields, always define a compute method. Optionally, you can store their values in the database.
- Relational Fields: When defining relational fields, you reference other models. It's important to ensure that these references are correctly set up.
> Fields are essential in defining the structure and behavior of business objects in Odoo, and they play a crucial role in the interaction between the user interface and the database.
