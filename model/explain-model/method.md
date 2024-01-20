# Methods
Methods in Odoo models are essentially Python functions that define the behavior or operations that can be performed on records. They are used for implementing business logic, manipulating data, and interacting with other models or external systems. Understanding how to effectively use methods is crucial for Odoo development.

## Types of Methods in Odoo
### CRUD Methods:

- Create (create): Method to create new records.
- Read (read): Method to read data from existing records.
- Update (write): Method to update existing records.
- Delete (unlink): Method to delete records.
### Action Methods:

Custom methods for specific actions, like processing an order, calculating values, etc.
### Compute Methods:

Methods used to compute the values of computed fields.
### Onchange Methods:

Triggered when fields' values are changed in the UI, typically used to update other fields based on those changes.
### Constraints Methods:

Methods that implement business logic validations.
Decorators
@api.model: Used when the method doesn't need access to record data (e.g., a setup method).
@api.multi / @api.returns: Used when the method operates on recordsets (multiple records).
@api.one: Used when the method is intended to work on a single record. Each record in the recordset will be processed one at a time.
@api.depends: Used for compute methods to define field dependencies.
@api.onchange: Used for onchange methods.
Example of Defining Methods
Here's an example showcasing various types of methods in an Odoo model:

```
from odoo import api, models, fields, exceptions

class LibraryBook(models.Model):
    _name = 'library.book'

    name = fields.Char('Title')
    state = fields.Selection([
        ('available', 'Available'),
        ('borrowed', 'Borrowed'),
        ('lost', 'Lost')
    ], default='available')

    # Action Method
    def borrow_book(self):
        for book in self:
            if book.state != 'available':
                raise exceptions.UserError("Book is not available for borrowing")
            book.state = 'borrowed'

    # Compute Method
    @api.depends('state')
    def _compute_is_available(self):
        for book in self:
            book.is_available = book.state == 'available'

    # Onchange Method
    @api.onchange('state')
    def _onchange_state(self):
        if self.state == 'lost':
            self.active = False

    # Constraint Method
    @api.constrains('num_pages')
    def _check_num_pages(self):
        for book in self:
            if book.num_pages <= 0:
                raise exceptions.ValidationError("Number of pages cannot be less than or equal to zero")
```
### Explanation
- borrow_book: This is a custom action method that changes the state of a book to 'borrowed'. It includes logic to check the current state of the book.
- _compute_is_available: A compute method to determine if the book is available. It's decorated with @api.depends to specify its dependency on the state field.
- _onchange_state: An onchange method that is triggered when the state field changes. Here, it is used to deactivate the book if it's lost.
- _check_num_pages: A constraint method that ensures the number of pages in a book is greater than zero.
### Best Practices
- Use the correct decorator based on the method's purpose.
- Write clear, concise methods focused on a single task or operation.
- Ensure methods that change data are safe and include necessary validations.
- Remember that methods can be overridden or extended in inheriting models, so design them with extensibility in mind.
-  Methods in Odoo models are instrumental in implementing the logic and rules of your business processes, and they form the interactive backbone of Odoo applications.
