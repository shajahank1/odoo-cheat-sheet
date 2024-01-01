# Models in Odoo
> In Odoo, models are fundamental components that represent the data structure of an application. They are essentially Python classes that define the schema for records, including fields, relationships, and behaviors. Models in Odoo are the equivalent of tables in a traditional database.

### Key Characteristics of Odoo Models
- Field Definition: Models define various types of fields (like char, text, integer, many2one, one2many, etc.) that represent the data structure.
- Record Rules and Access Rights: Models are linked with record rules and access rights to manage who can access and what they can do with the records.
- Methods and Business Logic: Models can contain methods (functions) to implement business logic.
- Inheritance: Odoo supports both classical and prototype inheritance for models, allowing for extension and customization of existing models.
### Creating a Model in Odoo
#### Step 1: Define the Model Class
First, you define a Python class inheriting from odoo.models.Model. This class represents your data model.

**Example**
```
from odoo import models, fields

class YourModel(models.Model):
    _name = 'your.model'  # Odoo database table name
    _description = 'Description of Your Model'

    name = fields.Char("Name")
    description = fields.Text("Description")
    # Define other fields as needed
```
**In this example:**

_name is the unique identifier for the model, used as the table name in the database.
Fields like name and description are defined to represent the data.
Step 2: Register the Model
The model class must be imported in your module's __init__.py file so that Odoo can recognize and register it.

```
# in your_module/__init__.py
from . import models
And in your models/__init__.py:
```

```
# in your_module/models/__init__.py
from . import your_model
```
#### Usage
Models are used for a variety of purposes:

- Data Storage: Representing and storing business data (e.g., customers, products, orders).
- Form Views: Models define the structure of data that is displayed in form views.
- Business Processes: Implementing business logic and workflows.
#### Extending Existing Models
You can also extend existing Odoo models to add new fields or modify business logic.

**Example of Model Inheritance**
```
class CustomPartner(models.Model):
    _inherit = 'res.partner'

    custom_field = fields.Char("Custom Field")
```
Here, res.partner is an existing model in Odoo, and CustomPartner extends it by adding a custom_field.

#### Conclusion
> Models are the backbone of any Odoo application, defining the structure and behavior of the data that drives business processes. They are highly customizable, allowing developers to tailor the system to meet specific business needs.

# Types of Model
> In Odoo, models are crucial for representing data structures and business logic. There are several types of models, each serving different purposes and offering various functionalities:

### Regular Models (models.Model):

- Description: These are the standard models used for most data storage needs in Odoo. They correspond to database tables.
- Usage: Used for storing persistent data like customers, products, orders, etc.
- Example: Defining a product model to store product information.

### Transient Models (models.TransientModel):

- Description: These models are temporary and are used mainly for wizards. Data stored in transient models are automatically deleted after a certain period or upon the user's session termination.
- Usage: Ideal for temporary data storage, like wizard forms that don't need permanent data persistence.
- Example: A setup wizard that guides a user through initial configuration steps.

### Abstract Models (models.AbstractModel):

- Description: These models are used as base models. They don't correspond to a database table and are used to share common functionalities between models.
- Usage: Useful for creating generic behaviors that can be reused in other models.
- Example: A mixin model providing a common set of fields and methods for different models.

### SQL View Models (models.Model with _auto = False):

- Description: These models are based on SQL views instead of regular tables. They're read-only and useful for reporting or complex queries.
- Usage: When you need to present data aggregated from multiple tables or with complex joins.
- Example: A reporting model that aggregates sales data from multiple tables for analysis.

### Mail Thread Models (models.Model with _inherit = ['mail.thread']):

- Description: These models inherit from mail.thread and are integrated into Odoo’s messaging system.
- Usage: For models where you want to attach mails, logs, and notifications.
- Example: Customer model with email integration for communication tracking.
  
### Delegated Inheritance Models:

- Description: This is a type of model inheritance in Odoo where a model inherits fields and behaviors from another but stores data in a separate table.
- Usage: When you want to extend an existing model without altering the original model's database structure.
- Example: Extending a user model to add additional fields specific to a module.

> Each type of model serves specific needs within an Odoo application, offering flexibility and robustness in designing the system's architecture and functionality. The choice of model type depends on the data persistence requirements, the relationship with other models, and the specific business logic that needs to be implemented.

##  Regular models in Odoo
> Regular models in Odoo, defined by inheriting from models.Model, are the most common type of models used for representing and managing business data. These models correspond to database tables, each record representing a row in the table.

### Key Characteristics of Regular Models
- Data Persistence: Regular models store data persistently in the database.
- Fields Definition: Fields in these models represent columns in the corresponding database table. You can define various field types like char, text, integer, float, boolean, date, binary, many2one (for relational fields), etc.
- ORM Methods: Odoo provides an Object-Relational Mapping (ORM) layer, allowing you to perform database operations without writing SQL queries.
- Business Logic: You can define methods to implement business logic, like actions to be taken when creating, updating, or deleting records.
- Inheritance: Regular models support both inheritance and extension, allowing you to modify or extend existing models.
- 
### Example of a Regular Model
Let's consider an example where we create a simple model for managing a library's book records.

```
from odoo import models, fields

class LibraryBook(models.Model):
    _name = 'library.book'
    _description = 'Library Book'

    name = fields.Char("Title", required=True)
    author = fields.Char("Author")
    isbn = fields.Char("ISBN")
    date_published = fields.Date("Date Published")
    # Other fields such as category, publisher, etc.
```
### Explanation
- _name: This is the technical name of the model, which is also the name of the database table.
- _description: Provides a human-readable description of the model.
name, author, isbn, date_published: These are fields in the model. Each field is mapped to a column in the database table.
Usage
- CRUD Operations: These models support Create, Read, Update, and Delete operations.
- Form and List Views: They are often linked with form and list views in Odoo's UI for user interaction.
- Business Workflows: Used to manage and automate business processes (like tracking books in a library in this example).

> Regular models are a fundamental part of Odoo's framework, enabling the development of complex business applications by providing a robust and flexible way to handle data and business logic.

