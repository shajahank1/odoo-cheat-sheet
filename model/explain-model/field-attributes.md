# Field Attributes
### 1. string
- Default: Field name with underscores replaced by spaces and capitalized.
- Example: name = fields.Char(string="Full Name")
- Usage: Used as the label in views.
### 2. help
- Default: None
- Example: age = fields.Integer(help="Enter the person's age")
- Usage: Provides a tooltip for the field in views.
### 3. readonly
- Default: False
- Example: state = fields.Selection([...], readonly=True)
- Usage: Makes the field uneditable in views.
### 4. required
- Default: False
- Example: email = fields.Char(required=True)
- Usage: Ensures the field cannot be empty/null.
### 5. default
- Default: None
- Example: active = fields.Boolean(default=True)
- Usage: Sets a default value for the field.
### 6. index
- Default: False
- Example: reference = fields.Char(index=True)
- Usage: Creates an index for the field in the database.
### 7. unique
- Default: Not applicable directly on fields; use _sql_constraints.
- Example: _sql_constraints = [('uniq_reference', 'UNIQUE(reference)', 'Reference must be unique')]
- Usage: Ensures field value is unique across the table.
### 8. store
- Default: True for regular fields, False for computed fields.
- Example: total = fields.Float(compute='_compute_total', store=True)
- Usage: Determines if the field is stored in the database.
### 9. compute
Default: None
Example: total = fields.Float(compute='_compute_total')
Usage: Specifies the method that computes the field's value.
### 10. inverse
- Default: None
- Example: total = fields.Float(inverse='_set_total')
- Usage: Method called to perform the inverse operation when the field is modified.
### 11. search
- Default: None
- Example: name = fields.Char(search='_search_name')
- Usage: Method to implement custom search logic.
### 12. related
- Default: None
- Example: city = fields.Char(related='partner_id.city')
- Usage: Creates a field related to another field.
### 13. depends
- Default: Not applicable directly on fields; used with @api.depends decorator.
- Example: @api.depends('line_ids.price_total')
- Usage: Specifies field dependencies for computed fields.
### 14. size
- Default: None for Char fields.
- Example: name = fields.Char(size=64)
- Usage: Sets the maximum size for character fields.
### 15. translate
- Default: False
- Example: description = fields.Text(translate=True)
- Usage: Indicates the field value is translatable.
### 16. selection
- Default: None
- Example: state = fields.Selection([('draft', 'Draft'), ('confirmed', 'Confirmed')])
- Usage: Defines choices for Selection fields.
### 17. digits
- Default: None
- Example: price = fields.Float(digits=(6, 2))
- Usage: Sets precision for numeric fields.
### 18. groups
- Default: None
- Example: secret_info = fields.Char(groups='base.group_system')
- Usage: Restricts field visibility to certain user groups.
### 19. copy
- Default: True
- Example: unique_ref = fields.Char(copy=False)
- Usage: Controls whether field value should be copied when duplicating a record.
> Understanding and correctly setting these attributes is key to creating effective and user-friendly Odoo applications.
