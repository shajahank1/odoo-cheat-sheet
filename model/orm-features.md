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
