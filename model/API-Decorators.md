# Odoo @api Decorators Guide
## 1. @api.model
**Trigger**: When the method is called without a recordset.
**Functionality**: Used for class-level methods that don't require a specific record, often for default values or setup operations.
Example:
```
@api.model
def _default_field(self):
    return "Default Value"
```
### Detailed Explanation
> @api.model is one of the decorators provided by Odoo's ORM (Object-Relational Mapping) system, used for defining model methods. Understanding and implementing @api.model correctly is important for effective Odoo development.

### Purpose of @api.model:
- Class-Level Method: @api.model is used for defining methods that are relevant to the model as a whole, rather than any specific record. It's typically used for methods that operate on the class level.
- No Recordset: Methods decorated with @api.model don't work on a recordset (self does not represent a record or recordset). Instead, self refers to the model class itself.
- Common Use Cases: Commonly used for creating new records, returning default values, or performing actions that are not tied to a specific record.
### How to Implement @api.model:
#### Basic Implementation:
##### Define a Method in a Model:

> Within an Odoo model (a Python class that inherits from models.Model), you define a method.
Decorate this method with @api.model.
##### Method Logic:

> Since self refers to the model class and not a recordset, the method logic should not assume any specific record.
It can perform operations applicable to the model as a whole, like returning general data, defaults, or creating new records.
**Example:**
Suppose you have a model my.model and want to define a method that calculates and returns some default value for a field when a new record is being created.

```
from odoo import api, fields, models

class MyModel(models.Model):
    _name = 'my.model'

    name = fields.Char('Name')
    description = fields.Text('Description')

    @api.model
    def get_default_description(self):
        # Logic to compute a default description
        return "This is a default description."

    @api.model
    def create_new_record(self, vals):
        # Logic to create a new record
        vals.update({'description': self.get_default_description()})
        return super(MyModel, self).create(vals)
```
> In this example, get_default_description is a method that computes and returns a default value for the description field. It is decorated with @api.model, signifying that it is a class method and does not operate on any specific record. The create_new_record method is another example that uses @api.model to create a new record with some default values.

### Key Points to Remember:
> Use @api.model for methods that do not need to operate on a specific record but instead work on a model level.
These methods can be called without a recordset, and self in the method represents the model class, not any particular record.
Ideal for utility functions, defaults, or creating records with specific criteria.
Understanding @api.model and its correct implementation is vital for developing modular, scalable, and efficient Odoo applications.

## 2. @api.multi
**Trigger**: Automatically, when called on a recordset. The default behavior in Odoo 10+.
**Functionality**: Used for operating on multiple records. The self in the method is a recordset containing multiple records.  
**Example**:
```
@api.multi
def process_records(self):
    for record in self:
        # Process each record
```
> @api.multi is an important decorator in Odoo's ORM (Object-Relational Mapping) system, particularly relevant in the context of methods that are designed to operate on recordsets.

### Purpose of @api.multi:
- Multi-Record Operation: @api.multi indicates that the method is intended to operate on multiple records of a model. It is the default behavior for model methods in Odoo.
Recordset Handling: When you decorate a method with @api.multi, self in the method context represents a recordset, which could be a single record or multiple records.
-Common Use Cases: It's commonly used for actions where the same operation needs to be applied to several records, like updating fields in bulk or performing calculations on multiple records.
### Implementation of @api.multi:
#### Basic Steps:
##### Define a Method in a Model:

> Create a method within your Odoo model (a class extending models.Model).
> Decorate this method with @api.multi. However, from Odoo 10 onwards, this is often implicit and can be omitted.
#### Method Logic:

> The method should be designed to handle a recordset (multiple records).
> Common practice is to iterate over self to apply the operation to each record in the recordset.
**Example:**
Suppose you have a model product.product and want to define a method that updates the stock level for multiple products.

```
from odoo import api, fields, models

class ProductProduct(models.Model):
    _inherit = 'product.product'

    stock_level = fields.Integer('Stock Level')

    @api.multi
    def update_stock_level(self, quantity):
        for product in self:
            product.stock_level += quantity
```
> In this example, update_stock_level is a method that increases the stock level of products. It iterates over self, which could be one or more product records. Each product's stock_level is updated by the specified quantity.

### Key Points to Remember:
> @api.multi is the default behavior for model methods from Odoo 10 onwards, so it's often not necessary to explicitly decorate methods with it.
> Use @api.multi when your method is intended to operate on recordsets (multiple records).
Always design the method's logic to handle multiple records gracefully, typically using a loop over self.
> Understanding and correctly implementing @api.multi is essential for Odoo developers, especially when dealing with operations that affect multiple records. This approach aligns with the recordset concept in Odoo, enabling efficient and scalable data manipulation.

## 3. @api.depends
**Trigger**: When any of the specified fields are modified.
**Functionality**: For computed fields. It automatically recalculates the field value when dependencies change.
Example:
```
@api.depends('field1', 'field2')
def _compute_total(self):
    self.total = self.field1 + self.field2
```
@api.depends is a crucial decorator in Odoo's ORM (Object-Relational Mapping) used primarily for computed fields. This decorator specifies the fields that a computed field depends on, ensuring that the computed field is recalculated whenever any of the dependent fields are modified.

### Purpose of @api.depends:
Automatic Recalculation: It is used to automatically update the value of a computed field when any of the fields it depends on change.
Dependency Tracking: Helps in maintaining data consistency and integrity by ensuring that changes in certain fields are reflected in related computed fields.
### How to Implement @api.depends:
#### Basic Steps:
- Define a Computed Field:
In your Odoo model, define a field with compute= parameter pointing to the method that computes its value.
- Use @api.depends Decorator:
Decorate the computation method with @api.depends, listing all the field names it should depend on.
- Write the Computation Logic:
Implement the logic to compute the field's value based on the dependent fields.
**Example**:
> Consider a sales order model (sale.order) where you need a computed field total_weight that calculates the total weight of the order based on the weights of individual order lines.

```
from odoo import api, fields, models

class SaleOrder(models.Model):
    _inherit = 'sale.order'

    total_weight = fields.Float(compute='_compute_total_weight', string='Total Weight')

    @api.depends('order_line', 'order_line.product_uom_qty', 'order_line.product_id.weight')
    def _compute_total_weight(self):
        for order in self:
            order.total_weight = sum(line.product_uom_qty * line.product_id.weight for line in order.order_line)
```
> In this example, the method _compute_total_weight is decorated with @api.depends. It specifies that the computation depends on the order_line, as well as the product_uom_qty and weight fields of the products in each order line. Whenever these fields are modified, total_weight is automatically recalculated.

### Key Points to Remember:
- Use for Computed Fields: @api.depends is specifically used for fields that are computed, i.e., their value is calculated based on other fields.
- Performance: Be mindful of the number of dependencies and complexity of computations to avoid performance issues.
- Data Consistency: It ensures that the computed field always reflects the current state of its dependent fields, enhancing data integrity.
Understanding @api.depends and its correct usage is essential for creating dynamic and responsive applications in Odoo, ensuring that computed fields are always up to date with their underlying data.

## 4. @api.onchange
**Trigger**: In response to UI changes in form views, when the specified field(s) are modified.
**Functionality**: Used to dynamically update other fields in the form based on changes in the UI, without saving the record.
Example:
```
@api.onchange('field1')
def _onchange_field1(self):
    self.field2 = self.field1 * 2
```
## 5. @api.constrains
**Trigger**: After creating or updating a record.
**Functionality**: To enforce business rules and data validation. Raises exceptions if the conditions are not met.
Example:
```
@api.constrains('age')
def _check_age(self):
    if self.age < 18:
        raise ValidationError("Age must be over 18.")
```
## 6. @api.returns
**Trigger**: When the decorated method returns a value.
**Functionality**: Defines the return type of a method, especially important for ensuring consistent return types.
Example:
```
@api.returns('self')
def some_method(self):
    return self.search([('id', '>', 5)])
```
## 7. @api.model_create_multi
**Trigger**: When creating multiple records.
**Functionality**: Optimizes the creation of multiple records, reducing the number of database round-trips.
Example:
```
@api.model_create_multi
def create(self, vals_list):
    # Logic for batch creation
```
## 8. @api.ensures_one
**Trigger**: When the method is called.
**Functionality**: Checks if the method is being called on a single-record recordset, raising an exception if it's called on multiple records.
Example:
```
@api.ensures_one
def method_name(self):
    # Logic for a single record
```
## 9. @api.depends_context
**Trigger**: When the context or specified keys in the context change.
**Functionality**: For computed fields whose value changes based on the user context, like language or company.
Example:
```
@api.depends_context('lang')
def _compute_name(self):
    # Logic depending on the language
```
## 10. @api.ondelete
**Trigger**: Before the deletion of records.
**Functionality**: Used to define actions or checks before records are deleted, typically to prevent deletion under certain conditions.
Example:
```
@api.ondelete(at_uninstall=False)
def _check_linked_records(self):
    if self.linked_records.exists():
        raise UserError("Cannot delete record as it has linked records.")
```
## 11. @api.autovacuum
**Trigger**: Periodically, as part of the database cleanup job.
**Functionality**: For scheduling automatic cleanup operations in models, like deleting old records or logs.
Example:
```
@api.autovacuum
def _autovacuum_expired_records(self):
    # Logic to clean up old or expired records
```
This guide provides a comprehensive overview of the @api decorators in Odoo, highlighting when


# @api.onchange vs @api.depends
> In Odoo, @api.onchange and @api.depends are two decorators used for different purposes in model development. Understanding their differences is crucial for effective use in Odoo applications.

## @api.onchange
**Purpose**:
> @api.onchange is used in form views to provide real-time interactivity. It triggers Python methods in response to changes made by the user in the UI.
## How it Works:
When a field specified in the @api.onchange decorator is modified in the UI, the decorated method is called.
The method can make changes to other fields in the view, which are reflected immediately in the UI.
Changes are not saved to the database until the user explicitly saves the form.
## Use Cases:
Dynamically updating or setting values of other fields based on the user's input.
Showing warnings or validation messages before the user submits the form.
Enhancing user experience with immediate feedback based on their actions.
Example:
```
@api.onchange('country_id')
def _onchange_country_id(self):
    self.city = 'Default City' if self.country_id.name == 'Country Name' else False
```
In this example, changing the country_id field updates the city field immediately in the UI.

## @api.depends
**Purpose**:
> @api.depends is used to define dependencies for computed fields. It ensures that the computed field is automatically recalculated when any of the specified fields change.
### How it Works:
The method decorated with @api.depends is automatically called when any of the fields it depends on are altered.
This is primarily used for fields that store their values in the database (when store=True).
### Use Cases:
Automatically recalculating a field when any of its dependencies change.
Ensuring data consistency and integrity for computed fields.
Example:
```
@api.depends('line_ids.price_total')
def _compute_total(self):
    self.total = sum(line.price_total for line in self.line_ids)
```
Here, _compute_total recalculates total whenever any price_total in line_ids changes, and total is stored in the database.

### Key Differences:
####Trigger Point:

> @api.onchange is triggered by user actions in the UI.
> @api.depends is triggered by changes to specified fields, regardless of how the change occurs (UI, backend, etc.).

### Persistence:
Changes made by @api.onchange are temporary and only applied when the user saves the form.
@api.depends affects fields that are usually stored in the database.
Use Case:

> @api.onchange is used for enhancing user experience and UI interactivity.
> @api.depends is used for maintaining data integrity and automatic updates of computed fields.
> Understanding these distinctions is essential for developing efficient, user-friendly, and data-consistent applications in Odoo.



