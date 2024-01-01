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




