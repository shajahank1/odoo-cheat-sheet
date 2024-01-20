# ORM 
Odoo's Object-Relational Mapping (ORM) is a core component of the Odoo framework. It provides an abstraction layer that allows developers to interact with the database in an object-oriented way, without needing to write raw SQL queries. This ORM layer enables efficient management of database operations, ensuring data integrity and security.

## Key Features of Odoo ORM
- Model Definition: In Odoo, models are Python classes that define the structure (fields) and behavior (methods) of business objects. Each model typically corresponds to a table in the database.

- Field Types: Odoo models include various field types for different data types (such as Char, Text, Integer, Float, Boolean, Date, Datetime, Binary, Selection, Many2one, One2many, Many2many), which automatically handle data conversion and storage.

- CRUD Operations: The ORM provides methods for Create, Read, Update, and Delete (CRUD) operations, enabling easy data manipulation. For example, create, search, browse, write, and unlink.

- Recordsets: Operations on models are performed using recordsets, which are collections of records. Recordsets are a powerful abstraction that allows for manipulating multiple records with concise syntax.

- Computed Fields: Support for fields whose values are computed using specified methods.

- Relational Fields: Fields that define relationships between models, like Many2one, One2many, and Many2many.

- Data Validation: Automatic data validation based on field types, and support for additional validation through Python methods.

- Inheritance: Supports both prototypical (delegation) and classical (extension) inheritance to extend or modify existing models.

- Business Logic Integration: Allows embedding of business logic directly into models through Python methods.

- Security: Includes a robust security mechanism with record rules and access rights, ensuring data access is regulated.

- API Decorators: Provides decorators like @api.model, @api.multi, @api.depends, and more, to define method behavior.

**Example of Using Odoo ORM**
```
from odoo import models, fields

class LibraryBook(models.Model):
    _name = 'library.book'
    _description = 'Library Book'

    name = fields.Char("Title", required=True)
    author_id = fields.Many2one("library.author", string="Author")
    page_count = fields.Integer("Page Count")

    def borrow_book(self, borrower_id):
        # Custom logic to borrow a book
        self.ensure_one()
        if self.state == 'available':
            self.state = 'borrowed'
            self.borrower_id = borrower_id
```
#### In this example:

A new model library.book is defined with fields for title, author, and page count.
A method borrow_book is defined for custom business logic.
### Conclusion
> Odoo's ORM is a crucial aspect of the framework, providing a high-level, object-oriented interface to interact with the database. It simplifies data handling, enhances developer productivity, and contributes to the robustness and scalability of Odoo applications.

## Interact with the data using the common ORM methods
Interacting with data in Odoo using the common Object-Relational Mapping (ORM) methods is a fundamental aspect of Odoo development. These methods allow you to perform Create, Read, Update, and Delete (CRUD) operations, along with other interactions with the database, in a Pythonic way. Here's an overview of how to use these methods:

### 1. Create (create)
Purpose: To create new records.
Usage: Pass a dictionary with the field names and their values.
Example:
```
new_partner = self.env['res.partner'].create({'name': 'John Doe', 'email': 'john@example.com'})
```
### 2. Read (browse and search)
- Browse:

Purpose: To fetch records by their IDs.
Usage: Pass the ID or list of IDs.
Example:
```
partner = self.env['res.partner'].browse(1)  # Gets the record with ID 1
```
- Search:

Purpose: To retrieve records matching certain criteria.
Usage: Pass a domain (a list of tuples specifying criteria).
Example:
```
customers = self.env['res.partner'].search([('customer_rank', '>', 0)])
```
### 3. Update (write)
Purpose: To update existing records.
Usage: Call write on a recordset, passing a dictionary with the updated values.
Example:
```
partner.write({'email': 'new.email@example.com'})
```
### 4. Delete (unlink)
Purpose: To delete records.
Usage: Call unlink on the recordset you want to delete.
Example:
```
partner.unlink()  # Deletes the record
```
### 5. Compute Methods
Purpose: Used for fields whose values are calculated based on other fields.
Usage: Define a method and decorate it with @api.depends.
Example:
```
@api.depends('field1', 'field2')

def _compute_field3(self):
    for record in self:
        record.field3 = record.field1 + record.field2
```
### 6. Onchange Methods
Purpose: Dynamically change the form view based on field values.
Usage: Define a method and decorate it with @api.onchange.
Example:
```
@api.onchange('field1')
def _onchange_field1(self):
    if self.field1 == 'certain_value':
        self.field2 = 'new_value'
```
### 7. Action Methods
Purpose: To perform specific actions, often triggered by user interactions.
Usage: Regular Python methods in the model.
Example:
```
def action_confirm_order(self):
    self.state = 'confirmed'
```
### Best Practices
- Data Integrity: Always ensure that data changes are consistent and validate data before writing to the database.
- Error Handling: Properly handle potential errors, especially in CRUD operations.
- Performance: Be aware of the potential performance implications of operations, especially in loops or batch operations.
- Security: Consider access rights and record rules in multi-user environments.
- Using these ORM methods, you can efficiently perform a wide range of data operations in Odoo, leveraging the framework's power to manage and manipulate business data effectively.
