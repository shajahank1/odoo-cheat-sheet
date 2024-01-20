# Fields
Fields in Odoo are a fundamental part of models, representing the various properties or attributes of a business object. Each field in an Odoo model corresponds to a column in a database table. Understanding the different types of fields and their usage is crucial for effective Odoo development.

## Types of Fields
### Basic Fields:
- Char: A string field, used for short text.
- Text: A longer text field without a length limit.
- Boolean: A true/false field.
- Integer: Stores whole numbers.
- Float: Stores decimal numbers, with optional precision.
### Date and Time Fields:

- Date: Stores a date.
- Datetime: Stores date and time.
### Relational Fields:

- Many2one: A many-to-one relationship. Links to another model (e.g., a customer to a sales order).
- One2many: A one-to-many relationship. Inverse of a Many2one field (e.g., all sales orders of a customer).
- Many2many: A many-to-many relationship. Links multiple records from one model to multiple records of another model.
### Special Fields:

- Selection: A dropdown list with a set of options.
- Binary: Used to store binary data like files or images.
- Reference: A reference to any record of any model.
### Computed Fields:

Computed fields have their value calculated on the fly, usually based on other field values. They can be stored in the database or calculated each time they are accessed.
### Example of Defining Fields
Here's an example of an Odoo model with various types of fields:

```
from odoo import models, fields

class LibraryBook(models.Model):
    _name = 'library.book'
    _description = 'Library Book'

    # Basic Fields
    name = fields.Char('Title', required=True)
    description = fields.Text('Description')
    active = fields.Boolean('Active?', default=True)
    num_pages = fields.Integer('Number of Pages')
    price = fields.Float('Price')

    # Date and Time Fields
    date_published = fields.Date('Published Date')
    last_borrowed_on = fields.Datetime('Last Borrowed On')

    # Relational Fields
    author_id = fields.Many2one('library.author', string='Author')
    borrower_ids = fields.Many2many('res.partner', string='Borrowers')
    order_ids = fields.One2many('library.order', 'book_id', string='Orders')

    # Special Fields
    state = fields.Selection([
        ('available', 'Available'),
        ('borrowed', 'Borrowed'),
        ('lost', 'Lost')
    ], default='available')

    # Computed Field
    is_available = fields.Boolean(compute='_compute_is_available')

    def _compute_is_available(self):
        for record in self:
            record.is_available = record.state == 'available'
```
In this example, LibraryBook has various fields including basic types like Char and Float, relational types like Many2one, and a computed field is_available.

### Usage and Best Practices
- Field Attributes: Fields can have various attributes like string (field label), required (mandatory field), readonly (non-editable), help (tooltip text), etc.
- Computed Fields: For computed fields, always define a compute method. Optionally, you can store their values in the database.
- Relational Fields: When defining relational fields, you reference other models. It's important to ensure that these references are correctly set up.
> Fields are essential in defining the structure and behavior of business objects in Odoo, and they play a crucial role in the interaction between the user interface and the database.


# Basic Fields
> Basic fields in Odoo are the fundamental building blocks used to define the structure of data models. These fields represent common data types and are used to store most forms of standard information in an Odoo application.

## Types of Basic Fields
Char (Character/String Field)

- Description: Used for storing short text strings.
- Example Usage: Names, titles, labels, etc.
- Code Example:
```
name = fields.Char("Name")
```
### Text (Long Text Field)

- Description: Used for longer text, without a length limit.
- Example Usage: Descriptions, comments, notes.
#### Code Example:
```
description = fields.Text("Description")
```
### Boolean (True/False Field)

- Description: Stores a boolean value (True or False).
- Example Usage: Flags, status indicators, checkboxes.
- Code Example:
```
is_active = fields.Boolean("Is Active", default=True)
```
### Integer (Whole Number Field)

- Description: Stores whole numbers (integers).
- Example Usage: Quantities, counts, year values.
#### Code Example:
```
quantity = fields.Integer("Quantity")
```
### Float (Decimal Number Field)

- Description: Stores numbers with decimals. Precision can be defined.
- Example Usage: Prices, weights, distances, ratings.
#### Code Example:
```
price = fields.Float("Price", digits=(6, 2))  # 6 digits with 2 decimals
```
### Html (HTML Content Field)

- Description: Stores HTML content. It is rendered as HTML in the view.
- Example Usage: Product descriptions, formatted text.
- Code Example:
```
html_description = fields.Html("HTML Description")
```
### Selection (Choice Field)

- Description: Provides a set of selectable options. Defined as a list of tuples.
- Example Usage: Statuses, types, categories.
- Code Example:
```
state = fields.Selection([
    ('draft', 'Draft'),
    ('confirmed', 'Confirmed'),
    ('done', 'Done')
], default='draft', string="Status")
```
### Key Aspects of Basic Fields
- Data Validation: Odoo automatically handles basic data validation based on the field type (e.g., ensuring that an Integer field only contains whole numbers).
- UI Integration: Each field type is associated with a default widget that determines how it is displayed and edited in the Odoo UI.
- Default Values: You can specify default values for fields.
- Required Fields: Fields can be marked as required, ensuring data integrity.
> Basic fields are essential for representing and manipulating data in an Odoo application. They allow developers to define the structure of models in a way that aligns with business needs, ensuring that data is stored, displayed, and processed appropriately.

----
# explain Date and Time Fields
In Odoo, Date and Time fields are used to handle temporal data, allowing for the storage and manipulation of dates and timestamps. Understanding these field types is crucial for managing time-sensitive data in Odoo applications.

## 1. Date Field
- Purpose: Used to store date information (year, month, day) without time.
- Field Type: fields.Date
- Common Usage: Birthdates, deadlines, start dates, end dates.
#### Example Code:
```
from odoo import fields, models

class YourModel(models.Model):
    _name = 'your.model'

    start_date = fields.Date("Start Date")
```
### Features:
Provides date selection widgets in the UI.
Can be used with default functions like fields.Date.today() for the current date.
Supports operations like adding or subtracting days.
## 2. Datetime Field
- Purpose: Used to store both date and time information.
- Field Type: fields.Datetime
- Common Usage: Timestamps for events, transaction times, deadlines with specific times.
#### Example Code:
```
end_datetime = fields.Datetime("End Datetime")
```
### Features:
Includes both date and time components.
Accommodates time zone conversions, which is crucial for consistent time data across different geographic locations.
Offers datetime widgets in the UI for user interaction.
Supports datetime operations, such as computing differences between two datetimes.
#### Considerations
- Time Zones: Handling time zones is important when dealing with Datetime fields. Odoo stores Datetime fields in UTC in the database and converts them to the user's time zone in the UI.
Default Values and Functions: For both Date and Datetime fields, you can use default functions to automatically set values based on the current date/time (e.g., fields.Date.context_today or fields.Datetime.now).
- Formatting: Odoo automatically handles the formatting of these fields in the UI, displaying them according to the user's locale and preferences.
> Date and Time fields are essential in Odoo for any application that requires date-based functionality, such as scheduling, record tracking, and time-sensitive transactions. They provide the necessary tools for handling and manipulating temporal data in a user-friendly and efficient manner.
-----
# Relational Fields
Relational fields in Odoo are used to establish relationships between different models (akin to tables in a database). They are crucial for representing complex data structures and associations in business applications. Odoo supports three main types of relational fields: Many2one, One2many, and Many2many.

## 1. Many2one
- Purpose: Creates a many-to-one relationship. It represents a situation where many records from one model can be related to one record in another model.
- Usage: Commonly used for fields like customer in a sales order, where each order is linked to a single customer.
- Field Definition:
```
customer_id = fields.Many2one('res.partner', string='Customer')
```
### Characteristics:
In the database, a Many2one field is stored as an integer, representing the ID of the related record.
In the user interface, it typically appears as a dropdown list.
## 2. One2many
- Purpose: Represents a one-to-many relationship, essentially the inverse of Many2one. It's a virtual field and not stored in the database.
- Usage: For example, order lines in a sales order, where each order can have multiple lines.
- Field Definition:
```
order_line_ids = fields.One2many('sale.order.line', 'order_id', string='Order Lines')
```
### Characteristics:
Defined in the model containing the "many" side, but requires a corresponding Many2one field in the related model.
It's a virtual relationship, meaning there's no actual field stored in the database for a One2many.
## 3. Many2many
- Purpose: Represents a many-to-many relationship, where records from one model can be related to multiple records in another model and vice versa.
- Usage: For instance, tags on a product, where each product can have multiple tags, and each tag can be associated with multiple products.
- Field Definition:
```
tag_ids = fields.Many2many('product.tag', 'product_tag_rel', 'product_id', 'tag_id', string='Tags')
```
### Characteristics:
In the database, a Many2many relationship is typically represented through a join table (in the example, product_tag_rel).
In the UI, it often appears as a list of selectable items.
### Considerations and Best Practices
- Indexing and Performance: Many2one fields are indexed in PostgreSQL, which improves performance for read operations. However, large Many2many relationships can impact performance and should be used judiciously.
- Bidirectional Access: Many2one and One2many are two sides of the same relationship and provide bidirectional access between the linked records.
- Cascade Operations: Be mindful of operations like delete or unlink, as they can have cascading effects in relational fields, especially in One2many and Many2many relationships.
- Use in Views: These fields can be used to create powerful and interactive views in Odoo, such as form views with inline editing for One2many fields.
> Relational fields are fundamental in Odoo for modeling complex real-world relationships in business applications. They allow developers to structure data in a way that reflects the interconnected nature of business processes and entities.

#  Special Fields
In Odoo, special fields refer to a set of field types that provide functionalities beyond standard data storage. These fields are used for specific purposes and offer unique behaviors or interfaces in the Odoo environment.

## 1. Selection Field
- Purpose: Used to provide a set of selectable options.
- Usage: Commonly used for statuses, types, categories, etc., where the value needs to be selected from a predefined list.
### Field Definition:
```
state = fields.Selection([
    ('option1', 'Label 1'),
    ('option2', 'Label 2'),
    ...
], string='State', default='option1')
```
### Characteristics:
> The selection can be a list of tuples or a callable returning such a list.
In the UI, it typically appears as a dropdown list or a set of radio buttons.
## 2. Binary Field
- Purpose: Stores binary data, such as files or images.
- Usage: Used for file uploads, storing images, attachments, etc.
### Field Definition:
```
image = fields.Binary("Image")
```
### Characteristics:
> Can be used with file upload widgets in the UI.
Suitable for storing any form of binary data.
## 3. Reference Field
- Purpose: Holds a reference to any record of any model.
- Usage: Useful when a field needs to refer to different models dynamically.
- Field Definition:
```
ref = fields.Reference(selection=[('model1', 'Model 1'), ('model2', 'Model 2')], string='Reference')
```
### Characteristics:
> Stores a reference in the format 'model_name,record_id'.
In the UI, it typically appears as a dropdown list to select the model and a field to choose the record.
## 4. Monetary Field
- Purpose: Used specifically for monetary values.
- Usage: Prices, costs, and any other financial amounts.
### Field Definition:
```
price = fields.Monetary("Price", currency_field='currency_id')
```
### Characteristics:
> Works in conjunction with a currency field (usually a Many2one relation to a currency model).
Helps in correctly formatting and converting monetary values.
## 5. Html Field
- Purpose: Stores HTML content.
- Usage: Product descriptions, formatted text in blogs or emails, etc.
- Field Definition:
```
description = fields.Html("Description")
```
### Characteristics:
> The content is rendered as HTML in the Odoo UI.
Suitable for rich text content.
### Best Practices and Considerations
- Selection Field: Be cautious about changing the keys of the selection options as it may affect existing data.
- Binary Field: Be mindful of the database size, as binary fields can significantly increase it.
- Reference Field: Use sparingly, as it can complicate database relations and logic.
- Monetary Field: Always define alongside a currency field for proper currency handling.
- Html Field: Ensure proper sanitization of HTML content to prevent XSS vulnerabilities.
> Special fields in Odoo provide enhanced functionalities tailored to specific use cases, contributing to the flexibility and robustness of Odoo's ORM in handling diverse data requirements.

----

# Computed Fields
Computed fields in Odoo are fields whose values are calculated dynamically through Python methods. These fields are particularly useful for values that need to be derived from other fields' data and are not meant to be manually entered by the user.

### Key Characteristics of Computed Fields
- Dynamic Calculation: Computed fields' values are calculated on the fly, often based on other fields in the model.

- Not Stored by Default: By default, computed fields are not stored in the database. Their values are calculated each time they are accessed, though you have the option to store them.

- Dependencies: Computed fields must declare dependencies using the @api.depends decorator. This ensures that the computed field is recalculated whenever the dependent fields change.

### Creating Computed Fields
- Define the Field: Declare the field in your model and specify the compute attribute with the name of the computing method.
- Create the Compute Method: Define the method responsible for calculating the field's value. Use the @api.depends decorator to set up dependencies.
### Example of Computed Fields
Consider a scenario where we have a sale.order model, and we want to compute the total amount of all the order lines.

Python Code in an Odoo Model:
```
from odoo import api, fields, models

class SaleOrder(models.Model):
    _name = 'sale.order'

    order_line_ids = fields.One2many('sale.order.line', 'order_id', string='Order Lines')
    total_amount = fields.Float(compute='_compute_total_amount', string='Total Amount')

    @api.depends('order_line_ids.price_subtotal')
    def _compute_total_amount(self):
        for order in self:
            order.total_amount = sum(line.price_subtotal for line in order.order_line_ids)
            ```
In this example:

- total_amount is a computed field that depends on the price_subtotal of each line in order_line_ids.
- The _compute_total_amount method calculates the sum of price_subtotal from all order lines and assigns it to total_amount.
### Usage and Best Practices
- : Consider whether the computed field should be stored. Storing the computed field can improve performance but may consume more space in the database.
- Correct Dependencies: Make sure to specify all fields that your computation depends on using the @api.depends decorator.
- Readability: Keep the logic in compute methods readable and maintainable.
- Recordset Operations: Remember that you're typically working with recordsets in compute methods. Iterate over them properly to calculate values.
> Computed fields are essential for dynamic data representation in Odoo applications. They offer great flexibility for calculations and derived data, ensuring that your application can respond to changes in related data efficiently and accurately
