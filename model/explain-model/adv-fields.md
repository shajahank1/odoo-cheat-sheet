# Advanced Fields
Odoo provides several advanced field types that allow for more complex behavior and interactions than basic field types. These advanced fields enable developers to implement sophisticated features in their Odoo applications. Here are some of the key advanced field types:

### 1. Many2one
Description: Creates a many-to-one relationship between two models.
Usage: Used to link a record to another record in a different model.
Example:
```
partner_id = fields.Many2one('res.partner', string='Customer')
```
### 2. One2many
Description: Represents a one-to-many relationship. It's a virtual field, meaning it doesn't correspond to a column in the database.
Usage: Used to link a record to multiple records in another model.
Example:
```
order_line_ids = fields.One2many('sale.order.line', 'order_id', string='Order Lines')
```
### 3. Many2many
Description: Creates a many-to-many relationship between two models.
Usage: Used to link multiple records in a model to multiple records in another model.
Example:
```
tag_ids = fields.Many2many('blog.tag', string='Tags')
```
### 4. Binary
Description: Stores binary data, such as files or images.
Usage: Commonly used for storing documents, pictures, etc.
Example:
```
document = fields.Binary('Document')
```
### 5. Reference
Description: A reference field can hold a reference to any record of any model, similar to a pointer in programming languages.
Usage: Useful for dynamic references where the exact model may vary.
Example:
```
ref = fields.Reference([('res.partner', 'Partner'), ('product.product', 'Product')], string='Reference')
```
### 6. Monetary
Description: Used for monetary values and always associated with a currency.
Usage: Ideal for financial and accounting data.
Example:
```
price = fields.Monetary('Price', 'currency_id')
```
### 7. Html
Description: Stores HTML code and renders it in the view.
Usage: Used for rich text content that requires HTML formatting.
Example:
```
description = fields.Html('Description')
```
### 8. Selection
Description: Provides a set of selectable options (stored as string or integer in the database).
Usage: Used when the value should be selected from a predefined list.
Example:
```
state = fields.Selection([
    ('draft', 'Draft'), 
    ('confirmed', 'Confirmed'), 
    ('done', 'Done')
], default='draft', string='Status')
```
These advanced fields enable developers to build rich and interactive applications in Odoo, catering to complex business requirements and data relationships. Proper understanding and usage of these fields are crucial for effective Odoo development.
