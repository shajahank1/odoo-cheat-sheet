# Constraint methods
Constraint methods in Odoo are used to validate data before it is written to the database. These methods enforce business rules and data integrity, ensuring that only valid data is saved. There are two types of constraints in Odoo: Python constraints and SQL constraints. Here, we'll focus on Python constraints.

## Python Constraints
- Purpose: Python constraints are used to enforce complex validation rules that can't be easily handled using SQL constraints.
- Behavior: They are triggered before data is written to or updated in the database. If the constraint condition is not met, an exception is raised, and the transaction is rolled back.
- Decorator: Python constraint methods are defined using the @api.constrains decorator.
- Exception: To indicate a validation failure, these methods should raise an odoo.exceptions.ValidationError.
### Creating Constraint Methods
- Define the Method: Create a method in your model and decorate it with @api.constrains, passing the field names that the method should react to.
- Implement Validation Logic: Inside the method, implement the logic to check whether the data meets the specified conditions.
### Example of a Constraint Method
Consider an example in a library.book model where we want to ensure that the number of pages in a book is positive.

Python Code in an Odoo Model:

```
from odoo import api, models, fields, exceptions

class LibraryBook(models.Model):
    _name = 'library.book'

    name = fields.Char('Title')
    num_pages = fields.Integer('Number of Pages')

    @api.constrains('num_pages')
    def _check_num_pages(self):
        for record in self:
            if record.num_pages <= 0:
                raise exceptions.ValidationError("A book must have a positive number of pages.")
```
### In this example:

- Constraint Trigger: The constraint is triggered whenever num_pages is created or modified.
- Validation Logic: The method _check_num_pages checks if the num_pages field is positive. If not, it raises a ValidationError, preventing the record from being saved.
### Usage and Best Practices
- Use for Business Logic Validations: Python constraints are best used for validations that involve business logic and require checking multiple fields or conditions that can't be expressed in SQL.
- rror Messages: Provide clear, user-friendly error messages in ValidationError.
- Performance Consideration: While Python constraints offer flexibility, they are executed on the application server, which can be less efficient than database-level SQL constraints for simple checks like field uniqueness.
Python constraints are a powerful feature in Odoo for enforcing data validation and integrity, ensuring that only data that meets specific business rules is saved to your database.
