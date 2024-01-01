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

## Transient Models (models.TransientModel)
> Transient Models in Odoo, defined by inheriting from models.TransientModel, are used for temporary data that doesn't need to be stored permanently in the database. These models are ideal for wizard-like functionalities where you need to ask the user for information or provide a multi-step form, but you don't need to keep the data once the operation is completed.

### Key Characteristics of Transient Models
- Temporary Storage: Data stored in transient models is temporary and will be automatically cleaned up by Odoo.
- Similar Structure to Regular Models: They are defined similarly to regular models (models.Model) but inherit from models.TransientModel.
- Not Linked to Database Tables: While they have fields and behaviors like regular models, they don't correspond to a permanent table in the database.
**Example of a Transient Model**
Consider a scenario where you need a wizard to allow users to configure some settings or to input data that isn’t stored permanently.

```
from odoo import models, fields

class LibraryBookReturnWizard(models.TransientModel):
    _name = 'library.book.return.wizard'
    _description = 'Library Book Return Wizard'

    return_date = fields.Date("Return Date")
    condition = fields.Selection([
        ('good', 'Good'),
        ('damaged', 'Damaged')
    ], string="Book Condition")
    # Other temporary fields for the wizard
```

### Explanation
- _name and _description: Similar to regular models, these define the technical name and description.
- return_date and condition: These fields are used to temporarily store the data input by the user. For example, the date when a book is returned and its condition.
### Usage
- Wizards and Temporary Data Collection: Often used for wizards that guide users through a process or collect information temporarily.
- No Permanent Impact on Database: Ideal for scenarios where the data doesn't need to be stored long-term.

> Transient models are a powerful feature for creating interactive user experiences that require temporary data processing, without the overhead of permanent data storage and database table management.

## Abstract Models (models.AbstractModel)
> Abstract Models in Odoo, defined by inheriting from models.AbstractModel, are used as base models to provide shared functionalities across multiple models. They act as mixins, offering a way to define common fields and methods once and then reuse them in other models.

### Key Characteristics of Abstract Models
- No Database Table: Unlike regular models, abstract models do not create a database table. They are used purely for extending functionality.
- Reusable Components: Abstract models are ideal for defining common behavior or fields that can be shared across different models.
- Mixin Concept: They follow the mixin concept, allowing multiple inheritance and reuse of components.
### Example of an Abstract Model
Imagine you have a requirement where multiple models in your application need a set of common features, like audit logging. Instead of repeating the same code in each model, you can create an abstract model that encapsulates this common functionality.

```
from odoo import models, fields

class AuditMixin(models.AbstractModel):
    _name = 'audit.mixin'
    _description = 'Audit Mixin'

    created_on = fields.Datetime("Created On", readonly=True, default=lambda self: fields.Datetime.now())
    updated_on = fields.Datetime("Updated On", readonly=True)

    @api.model
    def create(self, vals):
        record = super(AuditMixin, self).create(vals)
        # Additional logic for 'create' operation
        return record

    def write(self, vals):
        vals['updated_on'] = fields.Datetime.now()
        return super(AuditMixin, self).write(vals)
```
### Explanation
- _name: The technical name of the mixin.
- created_on and updated_on: Fields to track the creation and update times of records.
- Overriding create and write: Methods to add custom logic during record creation and updates.
### Usage in Other Models
You can inherit this mixin in other models to provide them with automatic audit logging capabilities.

```
class YourModel(models.Model):
    _name = 'your.model'
    _inherit = ['audit.mixin']

    name = fields.Char("Name")
    # Other specific fields for your model
```
In this setup, YourModel will automatically have created_on and updated_on fields, along with the custom create and write behaviors defined in AuditMixin.

### Usage
- Code Reusability and Organization: Abstract models promote DRY (Don't Repeat Yourself) principles, leading to cleaner and more maintainable code.
- Flexibility: They offer a flexible way to extend the functionality of multiple models without database overhead.

> Abstract models are a powerful tool in the Odoo framework, enabling developers to create modular, reusable, and maintainable code by sharing common functionalities across different models.


## SQL View Models (models.Model with _auto = False):
> SQL View Models in Odoo, created by setting _auto = False in a models.Model class, are designed to map the model to an SQL view instead of a regular database table. These models are particularly useful for reporting and data analysis purposes, as they allow you to create complex queries and aggregations that would be difficult or inefficient to achieve with regular models.

### Key Characteristics of SQL View Models
- Read-Only: They are typically read-only and don’t support writing data back to the database.
- No Corresponding Table: No database table is created. Instead, they are linked to an SQL view.
- Complex Queries: Ideal for complex SQL queries that aggregate data from multiple tables.
### Defining an SQL View Model
#### Step 1: Create the SQL View
> You first need to create an SQL view in your database. This is usually done in the init method of the model.

#### Step 2: Defining the Model
> Once the SQL view is created, you define an Odoo model that maps to this view. This model is defined like a regular Odoo model, but with _auto = False to prevent Odoo from trying to create a corresponding table in the database. Instead, the model will use the SQL view you defined.

**Python Model Example**
```
from odoo import models, fields, api

class ReportModel(models.Model):
    _name = 'report.model'
    _description = 'Report Model'
    _auto = False  # No table will be created for this model

    # Define fields that match the columns of your SQL view
    field1 = fields.Char("Field 1")
    field2 = fields.Integer("Field 2")

    @api.model_cr
    def init(self):
        # SQL code to create the view
        tools.drop_view_if_exists(self.env.cr, 'report_model')
        self.env.cr.execute("""
            CREATE OR REPLACE VIEW report_model AS (
                SELECT
                    ...
                FROM
                    ...
            )
        """)
```
In this example:

_name: The technical name of the model.
_auto = False: Indicates that this is an SQL view model.
init: A method where the SQL view is created. The view should be defined with a CREATE OR REPLACE VIEW statement.
### Usage
- Reporting and Analysis: This model can now be used for reporting and analysis purposes. For example, in creating report views, pivot tables, or graphs in Odoo.
- Data Aggregation: Useful in scenarios where data needs to be aggregated or calculated from various sources or tables.
Considerations
- Read-Only Nature: Since SQL view models are typically read-only, they are not suitable for scenarios where you need to create, modify, or delete records.
- Database Dependency: The SQL code in the init method depends on your database structure, so it might need adjustments if the underlying database tables change.
0 Odoo ORM Limitations: Complex operations that are not supported by Odoo's ORM can be handled in SQL view models.
SQL view models provide a powerful way to handle complex data operations and reporting in Odoo, enabling efficient data processing and analysis.

### Further Usage and Integration
- Views and Actions: You can create form, tree, search, and other views for this model and define actions and menu items to access these views, just like you would with a regular Odoo model.
- Read-Only Operations: Since the model is typically read-only, it's used for reporting and data analysis purposes, where you don't need to modify the underlying data.
- By following these steps, you can leverage SQL View Models in Odoo to handle complex data queries that are not easily achievable through standard ORM methods, making them ideal for reporting and analysis tools within your Odoo applications.

## Mail Thread Models in Odoo
> Mail Thread Models in Odoo, created by inheriting from mail.thread in a models.Model class, are used to integrate models with Odoo's messaging and notification system. This feature allows records of the model to have functionalities like messaging, emails, followers, and logging activities.

### Key Characteristics of Mail Thread Models
- Messaging and Communication: These models can send and receive messages, and can be integrated with email.
- Threaded Discussions: They support threaded discussions, where each record can have its own conversation history.
- Automatic Logging: Actions like creating, editing, or deleting records can be automatically logged in the record's chatter.
- Followers: Users can follow records to receive notifications about new messages or changes.
### Example of a Mail Thread Model
Let's consider an example where you create a custom model for managing projects, and you want to integrate it with the mail system for communication and logging.

```
from odoo import models, fields

class CustomProject(models.Model):
    _name = 'custom.project'
    _description = 'Custom Project'
    _inherit = ['mail.thread']

    name = fields.Char("Project Name", required=True, track_visibility='onchange')
    description = fields.Text("Description")
    # Other project-specific fields
```
### Explanation
- _inherit = ['mail.thread']: This line integrates the model with Odoo's mail thread functionality.
- track_visibility='onchange': This attribute in the field definition enables tracking changes to the field, so modifications are logged in the chatter.
### Usage
- Project Management: In this example, each custom.project record can have messages and logs attached to it. Team members can communicate through the chatter attached to each project.
- Change Tracking: Changes to the project's name will be automatically logged, providing a history of modifications.
- Notifications: Users can follow a project to receive notifications about updates.
Integration in Views
To fully utilize the mail thread functionalities, you need to include the chatter in the form view:

```
<record id="view_form_custom_project" model="ir.ui.view">
    <field name="name">custom.project.form</field>
    <field name="model">custom.project</field>
    <field name="arch" type="xml">
        <form>
            <!-- Your form fields -->
            <div class="oe_chatter">
                <field name="message_follower_ids" widget="mail_followers"/>
                <field name="message_ids" widget="mail_thread"/>
            </div>
        </form>
    </field>
</record>
```
### Conclusion
> Mail Thread Models are an integral part of many Odoo applications, providing a robust framework for communication, tracking, and notifications directly linked to records. This functionality enhances collaboration and record-keeping, making it a valuable feature for a wide range of business applications.

## Delegated inheritance Model
> Delegated inheritance in Odoo, also known as prototype inheritance, is a type of model inheritance where a model inherits fields, methods, and behaviors from another model but stores its data in a separate database table. This is different from classical inheritance, where the inherited model extends the base model's database table.

### Key Characteristics of Delegated Inheritance
- Separate Database Tables: Both the base and inherited models have their own database tables.
- Shared Fields and Methods: The inherited model has access to all fields, methods, and attributes of the base model.
- Use of _inherits: This is achieved using the _inherits attribute in Odoo models.
### Example of Delegated Inheritance
Let's consider an example where you have a base model base.model and you want to create a specialized version of this model called specialized.model.

**Base Model (base.model)**
```
from odoo import models, fields

class BaseModel(models.Model):
    _name = 'base.model'
    _description = 'Base Model'

    name = fields.Char("Name")
    # Other fields and methods
```
**Inherited Model (specialized.model) using Delegated Inheritance**
```
class SpecializedModel(models.Model):
    _name = 'specialized.model'
    _description = 'Specialized Model'
    _inherits = {'base.model': 'base_id'}

    base_id = fields.Many2one('base.model', required=True, ondelete='cascade', auto_join=True)
    specialized_field = fields.Char("Specialized Field")
    # Additional fields and methods specific to the specialized model
```
### Explanation
_inherits = {'base.model': 'base_id'}: This line indicates that SpecializedModel inherits from base.model. The base_id field is a Many2one field linking to base.model.  
Data for SpecializedModel is stored in its own table, but it includes a reference to a record in the base.model table.
Fields and methods from base.model are accessible in specialized.model, along with any additional fields/methods defined in specialized.model.
### Usage
- Scenario: Delegated inheritance is useful when you want to extend a model without modifying the base model's structure, especially when dealing with different types of similar records (e.g., different types of contacts like customers, suppliers, employees).
- Data Integrity: The ondelete='cascade' in the Many2one field ensures that if the base record is deleted, the corresponding specialized record is also deleted.
> Delegated inheritance allows for flexible and modular development in Odoo, enabling the creation of specialized models that maintain a clear and organized database structure. It's particularly useful for creating models that are an extension of existing models but with additional specific requirements or behaviors.
