# Models
Odoo models are a central concept in the Odoo framework, acting as the backbone for data management and business logic. They play a crucial role in defining the structure and behavior of business data. Understanding Odoo models is key to effectively developing and customizing Odoo applications.

### What are Odoo Models?
- Data Structure and Business Logic: Models in Odoo define the structure of business data (like fields and their data types) and include methods that implement business logic.

- Abstraction over Database Tables: Each model typically corresponds to a table in the PostgreSQL database. However, when working with models, you interact with high-level Python objects rather than direct database queries.

- Inheritance and Extension: Models in Odoo can inherit from other models, allowing you to extend or modify existing models' behavior and structure.

### Key Features of Odoo Models
- Fields: Define the data each record of the model will hold. Fields can be of various types, such as Char, Text, Integer, Float, Boolean, Date, Datetime, Binary, Selection, Many2one, One2many, and Many2many.

- Methods: Define the operations that can be performed on the records. These can be standard CRUD (Create, Read, Update, Delete) operations or more complex business logic.

- Inheritance: Odoo supports different types of inheritance, allowing developers to extend or modify existing models.

- Recordsets: Operations on models are performed using recordsets, which are collections of records, enabling batch operations and a functional-style approach to manipulating data.

- API Decorators: Methods in models often use decorators like @api.model, @api.multi, and @api.one, which affect how the method interacts with recordsets.

- Constraints: Models can define constraints (like SQL constraints or Python constraints) to ensure data integrity.

- Computed Fields: Fields whose value is calculated on the fly using a function.

### Example of an Odoo Model
Here’s a simple example of an Odoo model:

```

from odoo import models, fields

class Book(models.Model):
    _name = 'library.book'
    _description = 'Library Book'

    name = fields.Char('Title', required=True)
    author = fields.Many2one('library.author', string='Author')
    page_count = fields.Integer('Page Count')
    date_published = fields.Date('Published Date')
    state = fields.Selection([
        ('available', 'Available'),
        ('borrowed', 'Borrowed'),
        ('lost', 'Lost')
    ], default='available')

    def borrow_book(self):
        for book in self:
            book.state = 'borrowed'
    
    def return_book(self):
        self.ensure_one()
        self.state = 'available'
```
In this example, library.book is an Odoo model representing a book in a library. It has fields like name, author, page_count, and state. It also includes methods borrow_book and return_book for changing the book's state.

> Conclusion
Odoo models are powerful tools for defining the structure of your data and encapsulating the business logic of your application. They allow for easy interactions with the database and provide a high level of customization and flexibility, which is essential for developing comprehensive business applications in Odoo.
