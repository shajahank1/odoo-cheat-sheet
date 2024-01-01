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
### 5. Onchange Methods
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
### 6. Constraints (Python)
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

# Diffrence Between compute, onchange, and constrains 
> In Odoo, compute, onchange, and constrains are all attributes used in model fields, but they serve different purposes and are used in different contexts.

## 1. Compute (Computed Fields)
- Purpose: Compute attributes are used to define fields whose values are calculated on the fly from other fields' values. These fields don’t store data in the database unless store=True is specified.
- Functionality: The computed method is triggered whenever the system needs to determine the field's value, which could be due to a change in any of the dependent fields.
- Decorator: @api.depends
Example:
```
total_price = fields.Float(compute='_compute_total_price')

@api.depends('line_ids.price')
def _compute_total_price(self):
    for record in self:
        record.total_price = sum(line.price for line in record.line_ids)
```
> Usage: Ideal for fields whose value depends on other fields, like calculating a total price from line items.
## 2. Onchange (Dynamic Form Behavior)
- Purpose: Onchange is used for fields that should trigger a function in the form view when their value changes. This is mainly for UI interactivity.
- Functionality: The function updates other fields in the form based on the field’s current value. It's only triggered in the UI and doesn’t affect the database directly.
- Decorator: @api.onchange
Example:
```
@api.onchange('country_id')
def _onchange_country_id(self):
    self.state_id = False  # Clear the state when country changes
```
> Usage: Used to dynamically update parts of the form based on user input, such as clearing a field when another field changes.
## 3. Constrains (Data Validation)
- Purpose: Constrains are used to validate data before it's written to the database. They ensure data integrity by enforcing certain conditions.
- Functionality: If the conditions of the constraint are not met, Odoo will raise a validation error, preventing the record from being saved.
Decorator: @api.constrains
Example:
```
@api.constrains('age')
def _check_age(self):
    if any(record.age < 18 for record in self):
        raise ValidationError("Age must be greater than 18")
```
> Usage: Essential for enforcing business rules and data validation, like ensuring an age field is above a certain value.
### Summary
- Compute: For fields whose value is calculated from other fields.
- Onchange: For dynamic UI changes in response to user input.
- Constrains: For validating data before saving to the database.
> Each of these attributes plays a distinct role in Odoo development, contributing to the creation of dynamic, interactive, and reliable business applications.

# depends vs compute
In Odoo, @api.depends and compute are closely related concepts used in conjunction with each other for computed fields, but they serve distinct roles:

## 1. @api.depends
- Purpose: The @api.depends decorator is used to specify which fields a computed field depends on. It determines when the computation method should be triggered.
- Functionality: When any of the fields specified in @api.depends are modified, Odoo knows to recompute the computed field.
- Usage: It is essential for ensuring that computed fields are recalculated at the right time, maintaining accuracy and data integrity.
Example:
```
@api.depends('line_ids.price')
def _compute_total_price(self):
    ...
```
Here, _compute_total_price will be called whenever the price field of any record in line_ids is changed.
## 2. compute
- Purpose: The compute attribute in a field definition is used to link that field to a specific method for its calculation.
- Functionality: This method is the actual computation logic and is executed to determine the field's value.
- Usage: It is crucial for defining how the value of a computed field is obtained.
Example:
```
total_price = fields.Float(compute='_compute_total_price')
```
This links the total_price field to the _compute_total_price method for its value.
### Relationship and Usage
> When defining a computed field, compute is used to specify the method that calculates the field's value, while @api.depends is used to declare the dependencies that trigger this computation.
> Both are used together to create dynamic and automatically updated fields based on other field values within the record or related records.
> The combination of compute and @api.depends ensures that computed fields are updated in real-time as dependent data changes, providing up-to-date and accurate information.
> In summary, compute specifies the method to calculate a field's value, while @api.depends defines when this calculation should occur by listing the field dependencies. Both are essential for the effective use of computed fields in Odoo.

## @api.depends

> In Odoo, the @api.depends decorator is used primarily with computed fields to specify the dependencies that determine when the computation should be triggered. However, there's a common misconception about different "types" of @api.depends. In reality, @api.depends is a single concept with flexible usage depending on the fields you specify as dependencies. Let's clarify its attributes and functionalities:

### Basic Usage of @api.depends
Single Field Dependency: You can specify a single field upon which the computation depends.

```
@api.depends('field_name')
def _compute_method(self):
    ...
```
Here, the compute method is triggered whenever field_name changes.

### Multiple Field Dependencies: You can specify multiple fields as dependencies.

```
@api.depends('field1', 'field2', 'field3')
def _compute_method(self):
    ...
```
The compute method will be triggered if any of field1, field2, or field3 change.

### Related Model Field Dependencies: It also supports dot notation for specifying related model fields.

```
@api.depends('related_model_id.field_name')
def _compute_method(self):
    ...
```
The method is triggered when field_name in the related record (related_model_id) changes.

### Advanced Usage
Deep Dependencies: Odoo supports deep dependencies that span across relational fields.
```
@api.depends('line_ids.product_id.price')
def _compute_total(self):
    ...
```
In this case, the compute method depends on the price field of the product_id in each line record.

### Chained Dependencies: Dependencies can be chained across several models.

```
@api.depends('partner_id.country_id.currency_id.rate')
def _compute_currency_rate(self):
    ...
```
Here, changes in the currency rate of a partner's country will trigger the compute method.

### Functionality and Considerations
- Recalculation: Computed fields are recalculated immediately and automatically when any of the fields specified in @api.depends are modified.
- Performance: Specifying unnecessary dependencies can lead to performance issues due to frequent recalculations. It's essential to include only those fields that directly affect the computed value.
- Data Integrity: Accurately specifying dependencies ensures that the computed fields always reflect the current state of their dependent data.
> In summary, @api.depends is a versatile tool in Odoo's ORM, allowing developers to define precise conditions under which a computed field should be recalculated. By properly utilizing this functionality, developers can ensure that their applications always display up-to-date information without unnecessary computations.
