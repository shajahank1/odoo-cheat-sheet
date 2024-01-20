#  ORM Features
Odoo's Object-Relational Mapping (ORM) offers a range of advanced features that extend beyond basic CRUD (Create, Read, Update, Delete) operations. These advanced features allow for more complex data manipulation and business logic implementation, making Odoo a powerful tool for enterprise resource planning and business management. Here are some of the advanced ORM features in Odoo:

### 1. Computed Fields
- Description: Fields whose values are calculated dynamically using Python methods.
- Usage: Used for fields whose value depends on other fields.
Example:
```
from odoo import fields, models, api

class SaleOrder(models.Model):
    _name = 'sale.order'
    total_amount = fields.Float(compute='_compute_total_amount')

    @api.depends('order_line.price_total')
    def _compute_total_amount(self):
        for record in self:
            record.total_amount = sum(line.price_total for line in record.order_line)
```
### 2. Onchange Methods
- Description: Methods that are triggered by changes in the UI, usually for setting field values dynamically.
- Usage: Typically used in form views to update other field values based on changes.
Example:
```
@api.onchange('product_id')
def _onchange_product_id(self):
    self.price = self.product_id.standard_price
```
### 3. Constraints (Python & SQL)
- Python Constraints: Methods decorated with @api.constrains used to check business constraints.
- SQL Constraints: Database-level constraints defined in the model's _sql_constraints attribute.
Example:
```
from odoo import models, fields, api, exceptions

class LibraryBook(models.Model):
    _name = 'library.book'
    _sql_constraints = [
        ('name_uniq', 'UNIQUE (name)', 'The book title must be unique.')
    ]

    @api.constrains('pages')
    def _check_pages(self):
        if self.pages <= 0:
            raise exceptions.ValidationError("Number of pages must be positive")
```
### 4. Inheritance and Extension
- Description: Odoo supports two types of model inheritance: classical inheritance (_inherit) and prototypical inheritance (_inherits).
- Usage: Used to extend or modify existing models or create new models based on existing ones.
Example:
```
class CustomPartner(models.Model):
    _inherit = 'res.partner'
    custom_field = fields.Char("Custom Field")
```
### 5. Recordsets and Recordset Operations
- Description: Odoo uses recordsets for batch processing of records.
- Usage: Operations like filtered, mapped, sorted on recordsets for advanced data manipulation.
Example:
```
partners = self.env['res.partner'].search([]).filtered(lambda p: p.city == 'San Francisco')
```
### 6. Workflow Management (Odoo 13 and earlier)
- Description: Provides a framework for defining business processes (Note: largely replaced by Automated Actions and Server Actions in newer versions).
- Usage: Used to define sequences of actions and transitions.
Example:
```
<record id="workflow_sale_order" model="workflow">
    <!-- Workflow definition here -->
</record>
```
### 7. Automated Actions and Server Actions
- Description: Tools to automate business processes based on record changes or time conditions.
- Usage: Used to trigger Python code or create/update records automatically.
Example:
```
<record id="action_auto_send_email" model="ir.actions.server">
    <!-- Server action definition here -->
</record>
```
> These advanced ORM features provide a high degree of flexibility and power, allowing developers to efficiently implement complex business logic and data interactions within the Odoo framework.

----
# adding constraints and override default ORM methods.
Adding constraints and overriding default ORM methods are common tasks in Odoo development, allowing you to enforce business rules and customize standard behaviors.

### Adding Constraints
- SQL Constraints
SQL constraints are used to enforce data integrity at the database level. You define them in your model class using the _sql_constraints attribute.

Example:

```
from odoo import models, fields

class LibraryBook(models.Model):
    _name = 'library.book'

    name = fields.Char('Title', required=True)
    isbn = fields.Char('ISBN')

    _sql_constraints = [
        ('isbn_unique', 'UNIQUE(isbn)', 'The ISBN must be unique.'),
    ]
```
> This example ensures that the isbn field is unique across all records in the library.book model.

### Python Constraints
Python constraints are methods decorated with @api.constrains, used to perform checks on field values before saving the record. If a constraint is violated, a ValidationError is raised.

Example:

```
from odoo import models, fields, api, exceptions

class LibraryBook(models.Model):
    _name = 'library.book'

    num_pages = fields.Integer('Number of Pages')

    @api.constrains('num_pages')
    def _check_num_pages(self):
        for record in self:
            if record.num_pages <= 0:
                raise exceptions.ValidationError("The number of pages must be greater than 0.")
```
This example checks that the num_pages field value is greater than 0.

### Overriding Default ORM Methods
You can override default ORM methods like create, write, read, or unlink to customize their behavior. Always remember to call the super method to maintain the base logic.

Example: Overriding the create Method:

```
from odoo import models, fields, api

class LibraryBook(models.Model):
    _name = 'library.book'

    @api.model
    def create(self, vals):
        # Custom logic before creating a record
        if 'special_condition' in vals:
            # Do something special
            pass
        
        # Call the super method and store its result
        record = super(LibraryBook, self).create(vals)
        
        # Custom logic after creating a record
        # ...

        return record
````
This example adds custom logic before and after creating a record in the library.book model.

### Best Practices
- Use SQL Constraints Judiciously: SQL constraints are powerful but can be restrictive. Use them for critical data integrity rules like uniqueness.

- Handle Exceptions in Python Constraints: When raising exceptions in Python constraints, provide meaningful error messages.

- Maintain Super Method Logic: When overriding ORM methods, ensure that you call the super() method appropriately to keep the original method’s behavior intact.

- Test Thoroughly: Overriding ORM methods and adding constraints can have significant effects on your module's behavior. Thorough testing is essential to ensure stability and correctness.





