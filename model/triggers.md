# Triggers in Model
> In Odoo, triggers in models typically refer to ORM (Object-Relational Mapping) methods that are automatically executed in response to certain events like creating, updating, or deleting records. These methods are crucial for implementing business logic and data manipulation in Odoo applications. The primary ORM methods acting as triggers are create, write, and unlink.

## 1. Create Method
- Purpose: Used to create new records in the database.
- Signature: create(self, vals).
- Usage: Called with a dictionary vals containing the initial values for the new record.
Custom Logic: Allows for executing custom actions before or after a record is created.
Example:
```
@api.model
def create(self, vals):
    # Custom logic before creating a record
    new_record = super(YourModel, self).create(vals)
    # Custom logic after creating a record
    return new_record
```
## 2. Write Method
- Purpose: Used to update existing records.
- Signature: write(self, vals).
- Usage: Called with a dictionary vals containing the fields and their updated values.
- Custom Logic: Useful for executing actions before or after updating a record.
Example:
```
def write(self, vals):
    # Custom logic before updating a record
    result = super(YourModel, self).write(vals)
    # Custom logic after updating a record
    return result
```
### 3. Unlink Method
- Purpose: Used to delete records.
- Signature: unlink(self).
- Custom Logic: Allows for handling tasks before or after a record is deleted, such as cleanup or related data updates.
Example:
```
def unlink(self):
    # Custom logic before deleting a record
    result = super(YourModel, self).unlink()
    # Custom logic after deleting a record
    return result
```

### 4. Compute Methods for Computed Fields
- Purpose: Used to define fields whose value is calculated on the fly from other fields.
- Functionality: A computed field executes its compute method whenever the system needs to determine its value.
- @api.depends Decorator: This is used to specify which fields the computed field depends on. Odoo tracks changes to these fields and updates the computed field accordingly.
**Example:**
```
from odoo import models, fields, api

class SaleOrder(models.Model):
    _name = 'sale.order'
    
    order_line_ids = fields.One2many('sale.order.line', 'order_id', string="Order Lines")
    total_amount = fields.Float(compute='_compute_total_amount')

    @api.depends('order_line_ids.price_total')
    def _compute_total_amount(self):
        for record in self:
            record.total_amount = sum(line.price_total for line in record.order_line_ids)
```
In this example, total_amount is computed as the sum of price_total of all related order_line_ids.
### 2. Onchange Methods
- Purpose: Used to dynamically change the form view based on changes in certain fields.
- Functionality: When the value of a field in the form view is modified, methods decorated with @api.onchange are triggered. This can be used to update other fields in the view.
**Example:**
```
@api.onchange('partner_id')
def _onchange_partner_id(self):
    if self.partner_id:
        self.address = self.partner_id.address
```
Here, changing partner_id in a form view will automatically update the address field.
### 3. Constraints (Python)
- Purpose: Used for data validation.
- Functionality: Constraints are methods that are called before creating or writing to a record. They are used to ensure that the data meets certain criteria.
- @api.constrains Decorator: This is used to define constraint methods. If the constraint is violated, Odoo will raise an error.
**Example:**
```
@api.constrains('age')
def _check_age(self):
    for record in self:
        if record.age < 18:
            raise ValidationError("Age must be greater than 18")
```
This example ensures that the age field value is always greater than 18.

> These functionalities allow for a high degree of interactivity and data integrity within Odoo applications, ensuring that the user interface is dynamic and that the data stored in the database is consistent and valid.

#### Use Cases and Best Practices
- Data Validation: Implement checks and validations before creating or updating records.
- Automated Actions: Trigger related actions or updates in other models or logs.
- Cleanup Operations: Handle related data when deleting records to maintain database integrity.
> Remember, it's crucial to call the super() method within these overrides to ensure the base class's functionality is executed. This approach maintains the standard behavior of the ORM while allowing custom business logic to be integrated seamlessly. These triggers are fundamental in Odoo's framework, allowing developers to create applications with robust data handling and business processes.
