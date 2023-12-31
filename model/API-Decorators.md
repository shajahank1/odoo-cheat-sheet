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
> @api.onchange is an essential decorator in Odoo's ORM (Object-Relational Mapping) used to create dynamic and interactive forms in Odoo's user interface. It's specifically designed for real-time interactivity within form views.

### Purpose of @api.onchange:
- Real-time UI Interaction: It's used to trigger Python methods in response to changes made by the user in form fields. The method can modify the values of other fields in the form based on this change.
- Instant Feedback: Provides immediate feedback or interaction in the UI without needing to save the record or perform a server round trip.
### How to Implement @api.onchange:
#### Basic Steps:
- Define a Method in a Model:
In your Odoo model, create a method to be triggered when a specific field's value changes in the UI.
This method should handle the logic of how other fields should be updated or validated based on the change.
- Use @api.onchange Decorator:
Decorate this method with @api.onchange, specifying the field names that, when changed, should trigger this method.
- Implement UI Logic:
Inside the method, implement the logic for updating other fields, showing warnings, or dynamically changing options based on the user's input.
**Example**:
Consider a scenario in a sales order where selecting a product should automatically update the unit price based on the product's current price.

```
from odoo import api, fields, models

class SaleOrderLine(models.Model):
    _inherit = 'sale.order.line'

    @api.onchange('product_id')
    def _onchange_product_id(self):
        if self.product_id:
            self.price_unit = self.product_id.list_price
        else:
            self.price_unit = 0.0
```
> In this example, when the product_id field in a sale order line changes, the _onchange_product_id method is triggered. It updates the price_unit field with the product's list price. If no product is selected, it sets the price to 0.0.

###  Points to Remember:
- Temporary Changes: Changes made in an @api.onchange method are not immediately saved to the database. They only persist temporarily until the user saves the form.
- User Experience: Use @api.onchange to enhance user experience by providing immediate feedback or dynamic content based on user actions.
- Avoid Heavy Logic: Since these methods are triggered by UI interactions, it's advisable to avoid heavy computations to ensure a responsive user interface.
> Implementing @api.onchange methods is essential for creating a dynamic and responsive user interface in Odoo applications, making them more intuitive and user-friendly.

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
> @api.constrains is a critical decorator in Odoo's ORM (Object-Relational Mapping) used to enforce business rules and data validation constraints at the model level. It's primarily used to ensure data integrity by validating field values whenever records are created or updated.

### Purpose of @api.constrains:
- Data Validation: Used to define methods that are automatically triggered to check certain conditions or constraints when records are created or updated.
- Enforce Business Rules: Ensures that business rules are adhered to by raising exceptions if the defined constraints are not met.
### How to Implement @api.constrains:
#### Basic Steps:
- Define a Method in a Model:
In your Odoo model, create a method that encapsulates the logic for the constraint.
This method should check whether the record meets specific conditions and raise exceptions if it doesn't.
- Use @api.constrains Decorator:
Decorate this method with @api.constrains, specifying the field names that, when changed, should trigger this validation.
The method is automatically called after a record is created or these fields are updated.
- Implement Validation Logic:
Inside the method, implement the validation logic. Use ValidationError from odoo.exceptions to raise an error if the validation fails.
**Example**:
Suppose you have an Employee model, and you want to ensure that the email of each employee is unique across the company.

```
from odoo import api, models, exceptions

class Employee(models.Model):
    _name = 'my.employee'

    email = fields.Char('Email')

    @api.constrains('email')
    def _check_email(self):
        for record in self:
            if self.search_count([('email', '=', record.email), ('id', '!=', record.id)]) > 0:
                raise exceptions.ValidationError("Email must be unique per employee.")
                
```
> In this example, _check_email ensures that the email field is unique across all my.employee records. If a duplicate email is found, a ValidationError is raised, preventing the record from being saved.

### Key Points to Remember:
- Triggered Automatically: The method is called automatically after a record is created or the specified fields are updated.
- Exception Handling: Use ValidationError to provide a user-friendly error message.
- Performance Consideration: Be mindful of the performance impact of the constraints, especially in scenarios involving large datasets or complex searches.
> Implementing @api.constrains correctly is crucial for maintaining data integrity and enforcing business rules within Odoo applications, ensuring that all data conforms to specified requirements and conditions.

## 6. @api.returns
**Trigger**: When the decorated method returns a value.
**Functionality**: Defines the return type of a method, especially important for ensuring consistent return types.
Example:
```
@api.returns('self')
def some_method(self):
    return self.search([('id', '>', 5)])
```
> @api.returns is a decorator in Odoo's ORM (Object-Relational Mapping) system, primarily used to specify the return type of a method, particularly when dealing with recordsets.

### Purpose of @api.returns:
- Define Return Type: It explicitly defines the type of data a method returns, ensuring consistency and clarity in the method's output, especially when dealing with recordsets.
- Type Conversion: It automatically converts the method's return value to the specified type, which is particularly useful when the method's return type is not immediately obvious or differs from what's expected.
### How to Implement @api.returns:
##### Basic Steps:
- Define a Method in a Model:
Create a method within your Odoo model that performs an operation and returns a value.
- Use @api.returns Decorator:
Decorate this method with @api.returns, specifying the type of data it should return.
The first argument is the model name (as a string) if you're returning recordsets, or the Python type (like int, dict) for other data types.
Optionally, you can provide a lambda function as a second argument to transform the return value.
- Implement Method Logic:
Write the logic for your method. Ensure that the return value aligns with the type specified in the @api.returns decorator.
**Example**:
Suppose you have a method in a model that performs a search and you want it to return a recordset of res.partner.
```
from odoo import api, models

class ResPartner(models.Model):
    _inherit = 'res.partner'

    @api.model
    @api.returns('self', lambda value: value.id)
    def get_partner_by_city(self, city):
        return self.search([('city', '=', city)])
```
> In this example, get_partner_by_city returns a recordset of res.partner records. The @api.returns('self', lambda value: value.id) decorator indicates that the method returns a recordset of the current model ('self' refers to res.partner in this context). The lambda function transforms the recordset into a list of IDs, but this is optional and depends on your specific use case.

### Key Points to Remember:
- Use for Recordsets: Particularly useful when the method returns recordsets and you want to ensure a specific type of record is returned.
- Return Transformation: The optional lambda function allows for transforming the returned value, providing additional flexibility.
- Clarity and Consistency: Helps maintain clarity in method definitions, especially in complex models or when returning non-standard types.
> Implementing @api.returns correctly can greatly enhance the readability and maintainability of your Odoo code, especially in cases where the return type of a method is not immediately clear or standard. It's a powerful tool for ensuring type consistency in method returns.

## 7. @api.model_create_multi
**Trigger**: When creating multiple records.
**Functionality**: Optimizes the creation of multiple records, reducing the number of database round-trips.
Example:
```
@api.model_create_multi
def create(self, vals_list):
    # Logic for batch creation
```
> @api.model_create_multi is a decorator in Odoo's ORM (Object-Relational Mapping) used for optimizing the creation of records. It's particularly useful when you need to create multiple records at once, improving performance by reducing the number of database write operations.

### Purpose of @api.model_create_multi:
- Bulk Record Creation: Allows for the creation of multiple records in a single call, which is more efficient than creating records one by one, especially in terms of database interactions.
- Performance Optimization: Reduces the overhead of multiple create operations, making it a preferred method when dealing with large datasets or batch processing.
### How to Implement @api.model_create_multi:
##### Basic Steps:
- Define a Create Method:
Override the standard create method in your Odoo model.
This method should be capable of handling a list of dictionaries, where each dictionary contains the data for one record.
- Use @api.model_create_multi Decorator:
Decorate the create method with @api.model_create_multi.
This indicates that the method is designed to handle the creation of multiple records.
- Implement Bulk Creation Logic:
Write the logic to iterate over the provided list of data dictionaries and create records accordingly.

**Example:**
Let's say you have a model my.model and you want to optimize the creation of records.

```
from odoo import api, models

class MyModel(models.Model):
    _name = 'my.model'

    name = fields.Char('Name')

    @api.model_create_multi
    def create(self, vals_list):
        # vals_list is a list of dictionaries
        for vals in vals_list:
            # You can add custom logic for each record's values here
            pass
        return super(MyModel, self).create(vals_list)
```
> In this example, the overridden create method is decorated with @api.model_create_multi. The method takes vals_list, which is a list of dictionaries where each dictionary contains the data for one record. The method processes each dictionary as needed and then calls the super method to actually create the records.

### Key Points to Remember:
- Use for Bulk Operations: Ideal when you have scenarios like importing data from external sources or creating multiple records based on complex computations.
- Enhances Performance: Significantly improves performance by reducing the number of database insert commands.
- Handling Data: Ensure that the method correctly handles each dictionary in vals_list, as each represents a set of data for a new record.
> Implementing @api.model_create_multi is essential for optimizing the creation of records in bulk in Odoo, leading to more efficient and performant applications, especially in data-intensive scenarios.

## 8. @api.ensures_one
**Trigger**: When the method is called.
**Functionality**: Checks if the method is being called on a single-record recordset, raising an exception if it's called on multiple records.
Example:
```
@api.ensures_one
def method_name(self):
    # Logic for a single record
```
> @api.ensures_one is a decorator in Odoo's ORM (Object-Relational Mapping) system, used to guarantee that a method is called on a singleton recordset, i.e., a recordset containing only one record. This is crucial for methods where operating on multiple records at once could lead to incorrect behavior or data inconsistency.

### Purpose of @api.ensures_one:
- Singleton Enforcement: Ensures that the method is called with exactly one record in the recordset. It's a safeguard to prevent methods from being inadvertently called on multiple records.
- Error Prevention: If the method is called with a recordset containing more than one record, Odoo will raise an exception, thus preventing potential errors or unwanted behavior in multi-record scenarios.
#### How to Implement @api.ensures_one:
##### Basic Steps:
- Define a Method in a Model:
Create a method within your Odoo model that is intended to operate on a single record at a time.
- Use @api.ensures_one Decorator:
Decorate this method with @api.ensures_one.
This decorator does not require any parameters.
- Implement Method Logic:
Write the logic for the method as usual. The logic should be designed with the assumption that it will only ever deal with one record.
**Example:**
Suppose you have a model res.partner and a method that sends a special notification to a partner. This operation only makes sense for one record at a time.

```
from odoo import api, models

class ResPartner(models.Model):
    _inherit = 'res.partner'

    @api.ensures_one
    def send_special_notification(self):
        # Method logic here
        print(f"Sending notification to {self.name}")

```
> In this example, the send_special_notification method is intended to work on a single res.partner record. The @api.ensures_one decorator ensures that if this method is accidentally called with a recordset containing more than one record, Odoo will raise an exception, thus preventing the method from executing in a potentially harmful way.

### Key Points to Remember:
- Use for Single Record Operations: Ideal for methods that are logically meant to operate on a single record.
- Exception Handling: Be prepared to handle the exceptions that are raised if the method is called with more than one record.
- Safety Mechanism: It acts as a safeguard against programming errors where a method intended for a single record is mistakenly called on multiple records.
> Implementing @api.ensures_one is essential for maintaining data integrity and preventing logical errors in Odoo applications, especially in scenarios where operations are meant to be strictly limited to individual records.

## 9. @api.depends_context
**Trigger**: When the context or specified keys in the context change.
**Functionality**: For computed fields whose value changes based on the user context, like language or company.
Example:
```
@api.depends_context('lang')
def _compute_name(self):
    # Logic depending on the language
```
> @api.depends_context is a decorator in Odoo's ORM (Object-Relational Mapping) system used for computed fields. It's specifically designed to indicate that the computation of a field depends not only on database content but also on the context in which the computation is performed.

### Purpose of @api.depends_context:
- Context-Sensitive Computation: Allows defining computed fields whose values depend on the current user's context. This is particularly useful when the computation depends on external factors like the user's timezone, language, or company.
- Dynamic Recalculation: Ensures that the computed field is recalculated whenever the specified elements in the context change.
#### How to Implement @api.depends_context:
##### Basic Steps:
- Define a Computed Field:
In your Odoo model, define a field with the compute= parameter pointing to the method that computes its value.
- Use @api.depends_context Decorator:  
   Decorate the computation method with @api.depends_context, specifying the context keys that it depends on.  
   This decorator is often used in conjunction with @api.depends.
- Implement Context-Aware Logic:  
-- Inside the method, implement the logic for the computed field, taking into account the specified elements of the context.  

**Example:**
Consider a scenario where you have a computed field in a sales order model (sale.order) that calculates the total amount in the user's preferred currency.

```
from odoo import api, fields, models

class SaleOrder(models.Model):
    _inherit = 'sale.order'

    total_amount_user_currency = fields.Float(compute='_compute_total_amount_user_currency')

    @api.depends('order_line.price_total', 'currency_id')
    @api.depends_context('uid')
    def _compute_total_amount_user_currency(self):
        user_currency = self.env.user.company_id.currency_id
        for order in self:
            total_in_order_currency = sum(line.price_total for line in order.order_line)
            order.total_amount_user_currency = total_in_order_currency * self.currency_id.rate / user_currency.rate
```
> In this example, the method _compute_total_amount_user_currency calculates total_amount_user_currency, a field that depends on the order_line.price_total and currency_id fields, as well as the user's context (uid). It computes the total order amount in the currency preferred by the current user.

### Key Points to Remember:
- Use for Contextual Calculations: Ideal for computations where the outcome depends on the user's environment or settings.
- Combination with @api.depends: Often used alongside @api.depends to cover both database field changes and context changes.
- Performance Consideration: Be mindful of the impact on performance, as context-dependent fields might need to be recalculated more frequently.
> Implementing @api.depends_context is crucial for developing dynamic and context-sensitive applications in Odoo, ensuring that computed fields reflect the most relevant and up-to-date information based on the user's current context.

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
> @api.ondelete is a specialized decorator in Odoo's ORM (Object-Relational Mapping) used for handling cleanup or validation operations before records are deleted. This decorator is particularly useful when you need to perform certain checks or actions right before a record is permanently removed from the database.

### Purpose of @api.ondelete:
- Pre-Deletion Handling: It allows developers to define methods that are executed just before records are deleted.
- Validation and Cleanup: Commonly used to check if it's safe to delete a record or to perform necessary cleanup tasks associated with the record being deleted.
### How to Implement @api.ondelete:
##### Basic Steps:
- Define a Method in a Model:
1. Create a method within your Odoo model that should be executed before record deletion.
2. This method can include logic to check conditions or perform cleanup tasks.
- Use @api.ondelete Decorator:
1. Decorate this method with @api.ondelete.
2. Optionally, you can specify at_uninstall=False to indicate that the method should not be executed when modules are uninstalled.
- Implement Pre-Deletion Logic:
1. Write the logic for actions to be taken before the deletion of the record. This often involves checks to prevent deletion under certain conditions, and if those conditions are met, raise an exception to stop the deletion.
**Example:**
Suppose you have a model project.task and you want to prevent the deletion of tasks if they are marked as 'Important'.

```
from odoo import api, models, exceptions

class ProjectTask(models.Model):
    _inherit = 'project.task'

    is_important = fields.Boolean("Important")

    @api.ondelete(at_uninstall=False)
    def _check_important_task(self):
        for task in self:
            if task.is_important:
                raise exceptions.UserError("Cannot delete a task marked as important.")
```
> In this example, the method _check_important_task is decorated with @api.ondelete. It checks if the project.task record is marked as 'Important'. If so, it raises a UserError, preventing the deletion of the task.

### Key Points to Remember:
- Use for Validation Before Deletion: It's ideal for enforcing business rules that determine whether a record can be safely deleted.
- Exception Handling: If the conditions for safe deletion are not met, you should raise an exception to halt the deletion process.
- Not for Cleanup After Deletion: This method is for pre-deletion checks, not for handling tasks after a record has been deleted.
> Implementing @api.ondelete methods correctly is vital for maintaining data integrity and enforcing business logic in Odoo applications, particularly in scenarios where the deletion of records has significant implications

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

> @api.autovacuum is a specific decorator in Odoo's ORM (Object-Relational Mapping) used for scheduling automated cleanup operations. It's particularly useful for managing tasks like clearing old records or performing regular maintenance activities that help in keeping the database optimized.

### Purpose of @api.autovacuum:
- Automated Cleanup: Allows for the definition of methods in models that are automatically executed by Odoo's scheduler for cleaning up old or irrelevant data.
- Database Maintenance: Helps in maintaining database health by removing obsolete data, which can improve performance and reduce storage usage.
### How to Implement @api.autovacuum:
##### Basic Steps:
- Define a Method in a Model:
1. Create a method within your Odoo model intended for regular cleanup or maintenance tasks.
2. This method should contain logic for identifying and removing/deactivating old or irrelevant records.
- Use @api.autovacuum Decorator:
1. Decorate this method with @api.autovacuum.
2. There are no parameters required for this decorator.
- Implement Cleanup Logic:
1. Write the logic for the cleanup operation. This could involve searching for records that meet certain criteria (like being older than a specific date) and then deleting or archiving them.
**Example:**
Imagine you have a model my.model and you want to delete records that are older than a year.

```
from odoo import api, fields, models
from datetime import timedelta

class MyModel(models.Model):
    _name = 'my.model'

    create_date = fields.Datetime("Creation Date")

    @api.autovacuum
    def _clean_old_records(self):
        one_year_ago = fields.Datetime.now() - timedelta(days=365)
        old_records = self.search([('create_date', '<', one_year_ago)])
        old_records.unlink()
```
> In this example, _clean_old_records is a method that searches for records in my.model that were created more than a year ago. These records are then deleted using the unlink() method. The @api.autovacuum decorator ensures that this method is called automatically as part of Odoo's regular cleanup job.

### Key Points to Remember:
- Scheduled Execution: The method is executed periodically based on Odoo's scheduler, not triggered by user actions.
- Use for Regular Maintenance: Ideal for tasks like cleaning logs, removing temporary data, or archiving old records.
- Careful Implementation: Ensure that the logic for identifying records is accurate and that you're only removing data that is safe to delete.
> Implementing @api.autovacuum methods is a powerful way to automate maintenance tasks within Odoo applications, contributing to the overall health and efficiency of the system


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



