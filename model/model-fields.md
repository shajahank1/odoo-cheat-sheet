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

# Model Attributes
> In Odoo, fields in models can have several attributes that define their behavior, appearance, and relationships. Here are some of the key attributes used with field definitions:

### string:
- Description: Human-readable label for the field, used in views.
- Example: name = fields.Char(string="Name")
- Usage: Determines how the field is labeled in the user interface.

### required:
- Description: If set to True, the field must be filled in by the user.
- Example: email = fields.Char(string="Email", required=True)
- Usage: Enforces data integrity by ensuring that important fields are not left blank.

### readonly:
- Description: If True, the field cannot be modified in the user interface.
- Example: create_date = fields.Datetime(string="Created On", readonly=True)
- Usage: Useful for displaying information that should not be edited, like system-generated timestamps.

### default:
- Description: Sets a default value for the field.
- Example: state = fields.Selection([...], default='draft')
- Usage: Initializes fields with a preset value when creating new records.

### help:
- Description: Provides a tooltip for the field in the user interface.
- Example: age = fields.Integer(string="Age", help="Enter your age in years")
- Usage: Useful for giving users hints or additional information about what the field represents.

### index:
- Description: If True, creates a database index for the field, which can improve search performance.
- Example: email = fields.Char(string="Email", index=True)
- Usage: Beneficial for fields that are frequently searched or used in filters.

### store:
- Description: Determines whether the field's value is stored in the database. Usually, computed fields are not stored unless specified.
- Example: age = fields.Integer(compute='_compute_age', store=True)
- Usage: Important for fields whose values need to be persistent and queryable.

### compute:
- Description: Specifies a method name that computes the field's value.
- Example: full_name = fields.Char(compute='_compute_full_name')
- Usage: Used for fields whose value is derived from other fields.

### inverse:
- Description: Defines a method that inversely sets the value of a computed field.
- Example: full_name = fields.Char(compute='_compute_full_name', inverse='_set_full_name')
- Usage: Allows computed fields to be writable, with the inverse method handling how changes are saved.

### track_visibility:
- Description: Tracks changes to the field value in the model's chatter.
- Example: status = fields.Selection([...], track_visibility='onchange')
- Usage: Useful for monitoring and logging changes in important fields.

> Each of these attributes provides specific functionality to the field, allowing for detailed control over how data is entered, displayed


# Automatic Fields
> Odoo automatically creates and manages certain fields in all models, known as automatic fields. These fields provide essential information about the record's creation, modification, and relational data. Here are the key automatic fields in Odoo:

### id:
- Description: A unique identifier for each record. It's the primary key in the database table.
- Usage: Automatically handled by Odoo; used to reference records uniquely.

### create_uid:
- Description: The user who created the record.
- Type: Many2one relationship linking to the res.users model.
- Usage: Useful for tracking which user created a specific record.

### create_date:
- Description: The date and time when the record was created.-
- Type: Datetime field.
- Usage: Automatically set by Odoo; helpful for auditing and historical data tracking.

### write_uid:
- Description: The user who last updated the record.
- Type: Many2one relationship linking to the res.users model.
- Usage: Used to identify the user responsible for the last modification.

### write_date:
- Description: The date and time when the record was last updated.
- Type: Datetime field.
- Usage: Set by Odoo whenever a record is written to; useful for tracking modifications.

### __last_update:
- Description: A special field that indicates the last modification time of the record.
- Type: Datetime field, essentially a mirror of write_date.
- Usage: Often used in views and client-side logic to determine if a record has been modified.
**Example**
When you define a new model in Odoo, these automatic fields are implicitly present and managed by the system. For instance:

```
from odoo import models, fields

class CustomModel(models.Model):
    _name = 'custom.model'
    name = fields.Char("Name")
```
    # No need to explicitly declare automatic fields like create_date, write_date, etc.
> In this example, CustomModel will have create_uid, create_date, write_uid, write_date, and id fields automatically, even though they are not explicitly declared.

### Usage
> These automatic fields are widely used in Odoo for auditing, security, data integrity, and synchronizing data with external systems. They provide a built-in mechanism to track the creation and modification of records, which is crucial for enterprise applications.
