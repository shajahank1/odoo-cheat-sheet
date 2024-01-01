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
