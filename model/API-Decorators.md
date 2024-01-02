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
## 2. @api.multi
**Trigger**: Automatically, when called on a recordset. The default behavior in Odoo 10+.
**Functionality**: Used for operating on multiple records. The self in the method is a recordset containing multiple records.
Example:
```
@api.multi
def process_records(self):
    for record in self:
        # Process each record
```
## 3. @api.depends
**Trigger**: When any of the specified fields are modified.
**Functionality**: For computed fields. It automatically recalculates the field value when dependencies change.
Example:
```
@api.depends('field1', 'field2')
def _compute_total(self):
    self.total = self.field1 + self.field2
```
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



