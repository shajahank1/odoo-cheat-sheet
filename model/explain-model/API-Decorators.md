#  API-Decorators
API decorators in Odoo are used to define the behavior of model methods, particularly how these methods interact with recordsets. They are essential in determining how a method processes records, and they simplify the development of business logic within Odoo's ORM framework.

## Commonly Used API Decorators
- @api.model:
> Indicates that the method is relevant to the model as a whole and not to a specific record.
Typically used for methods that operate independently of any record, like methods that return a default value or a report action.
Methods with this decorator do not expect a recordset (self is the model itself).
Example:

```
@api.model
def method_name(self, args):
    # logic that doesn't involve a specific record
```
-- @api.multi / @api.returns:
> The default behavior for most methods in Odoo.
Indicates that the method can handle multiple records (a recordset).
This is useful when you want to perform an operation on several records at once.
Example:
```
@api.multi
def method_name(self, args):
    for record in self:
        # logic for each record in the recordset
```
- @api.one:

> Intended for methods that are designed to work on a single record at a time.
If used on a recordset, Odoo will internally loop through each record and apply the method.
Less common in newer Odoo versions and generally not recommended due to potential performance issues.
Example:

```
@api.one
def method_name(self, args):
    # logic for a single record
```
- @api.depends:

> Used for compute methods (methods that calculate field values).
Specifies which fields will trigger the method when their values change.
Essential for computed fields to ensure they are recalculated at the right time.
Example:

```
@api.depends('field1', 'field2')
def _compute_field(self):
    for record in self:
        # compute logic based on field1 and field2
```
- @api.onchange:
> Used for methods that should be triggered by changes in the UI.
These methods respond to changes in specified fields in the form view, often used to update other fields based on those changes.
Example:
```
@api.onchange('field1')
def _onchange_field1(self):
    if self.field1:
        # logic to update other fields based on field1
```
- @api.constrains:
> Used to define methods that act as constraints.
These methods are triggered when saving a record and are used to validate field data.
If the constraint is violated, the method should raise a ValidationError.
Example:

```
@api.constrains('field1')
def _check_field1(self):
    for record in self:
        if not self._some_condition_on_field1():
            raise ValidationError("Field1 condition failed")
```
### Summary
> API decorators play a critical role in defining how methods in Odoo models behave with respect to recordsets and field changes. They are key in structuring the logic within Odoo applications, making code more readable and maintaining the standard behavior and conventions of Odoo's ORM.
