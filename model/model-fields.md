# Model Fields

> model fields represent the various types of data that can be stored in a model, akin to columns in a database table. Each field type in Odoo has specific characteristics and is used for different types of data.

## Common Field Types
### Char (Character/String)

- Usage: For storing short text or string values.
- Example: name = fields.Char("Name")
- Details: Often used for names, titles, or other short text values.

### Text

- Usage: For longer text, similar to Char but without a length limit.
- Example: description = fields.Text("Description")
- Details: Ideal for descriptions, comments, or any lengthy text.
  
### Boolean

- Usage: For true/false values.
- Example: is_active = fields.Boolean("Active", default=True)
- Details: Commonly used for flags, like indicating if a record is active or inactive.

### Integer

- Usage: For whole numbers.
- Example: quantity = fields.Integer("Quantity")
- Details: Used for numeric values without decimal points, like quantities or counts.

### Float

- Usage: For numbers with decimal points.
- Example: price = fields.Float("Price")
- Details: Suitable for prices, weights, and other measurements.

### Date and Datetime

- Usage: For storing dates and date-time values.
- Examples:
birth_date = fields.Date("Birth Date")
appointment_datetime = fields.Datetime("Appointment Datetime")
- Details: Used for any date or date-time related data.
  
### Selection

- Usage: For a fixed set of choices.
- Example: state = fields.Selection([('draft', 'Draft'), ('confirmed', 'Confirmed')], "State")
- Details: Ideal for status fields, types, or any field with a limited set of options.

### Many2one

- Usage: For creating a many-to-one relationship.
- Example: customer_id = fields.Many2one('res.partner', string="Customer")
- Details: Links this record to another record in a different model. Used for relational data.

### One2many and Many2many

- Usage: For one-to-many and many-to-many relationships.
- Examples:
order_line_ids = fields.One2many('order.line', 'order_id', string="Order Lines")
tag_ids = fields.Many2many('product.tag', string="Tags")
- Details: Used for linking to multiple records in another model. One2many is a virtual relationship, usually the inverse of a Many2one. Many2many is for associations where a record can belong to multiple records in another model.

### Binary

- Usage: For storing binary data like files or images.
- Example: image = fields.Binary("Image")
- Details: Commonly used for file uploads, images, and document storage.

#### Usage
> Fields are declared in Odoo models to define the data structure.
They are used in views (forms, lists, kanban, etc.) to display and edit data.
Fields can also include additional parameters like required, readonly, help (for tooltips), and default (default values).
Odoo's ORM (Object-Relational Mapping) automatically handles the conversion between these field types in Python and the corresponding types in the database, simplifying data manipulation and ensuring type safety. This makes it easy to develop business applications with complex data requirements.
