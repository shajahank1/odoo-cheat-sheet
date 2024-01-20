# Recordsets
In Odoo, a recordset is a fundamental concept representing a collection of records (rows in a database) from a model (table). It's an ordered collection, much like a list, but with a rich API specifically designed for handling database records in a convenient and efficient way. Understanding how to work with recordsets is key to effective development in Odoo.

## Key Characteristics of Recordsets
- Collection of Records: A recordset may contain one, many, or no records. It can be thought of as a list of records.

- Model-Specific: Each recordset is specific to a single model (e.g., a recordset of res.partner records).

- Immutable: Recordsets are immutable. Any operation that would modify the recordset (like filtered, sorted) actually returns a new recordset.

- Chaining Operations: You can chain methods and operations on recordsets. For instance, self.env['res.partner'].search([...]).filtered(...).sorted(...)

## Common Operations on Recordsets
- Searching: search(domain) is used to search for records matching the specified domain criteria.
- Browsing: browse(ids) retrieves the records corresponding to the provided ids.
- Creating: create(vals) creates a new record with the specified vals.
- Writing: write(vals) updates the records in the recordset with the provided vals.
- Deleting: unlink() deletes the records in the recordset.
### Working with Single Records and Multiple Records
Operations on a single record (like accessing a field value) and operations on multiple records (like looping over a recordset) are handled through the same API.
Example of Using Recordsets
Here's an example demonstrating how to work with recordsets in Odoo:

Python Code in an Odoo Model:

```
from odoo import models, fields

class LibraryMember(models.Model):
    _name = 'library.member'
    _description = 'Library Member'

    name = fields.Char('Name')
    book_ids = fields.Many2many('library.book', string='Borrowed Books')

    def return_all_books(self):
        for member in self:
            member.book_ids = [(5,)]  # Remove all book links

    def borrow_book(self, book):
        for member in self:
            member.book_ids |= book  # Add the book to the member's borrowed books
```
In this example:

return_all_books method demonstrates modifying a Many2many field on each member in the recordset.
borrow_book method shows adding a book to the book_ids field using the |= operator.
### Summary
> Recordsets are a versatile and powerful way to interact with database records in Odoo. They provide a high-level API for CRUD operations and set operations, making data manipulation in Odoo both efficient and intuitive. Understanding and effectively using recordsets are crucial for any Odoo developer.
