# Odoo ORM
The Odoo ORM (Object-Relational Mapping) is a crucial part of the Odoo framework. It acts as an interface between the Odoo models (written in Python) and the database (PostgreSQL), allowing developers to interact with the database in an object-oriented manner without needing to write raw SQL queries. Understanding the ORM is key to effective development in Odoo. Here's an overview of its main aspects:

### 1. ORM Basics:
- Models: In Odoo, models are Python classes that represent business objects and are mapped to database tables. They are defined in modules and contain fields and methods.
- Fields: Fields are class attributes that define the properties of a model. They are mapped to columns in a database table. Odoo provides various field types to handle different data types.
- Methods: Methods in Odoo models can be used to define business logic. They can be standard Python methods or special ORM methods that interact with the database.
### 2. Field Types:
- Basic Fields: Char, Text, Integer, Float, Boolean, etc., are used for basic data types.
- Relational Fields: These define relationships between models. Types include:
- Many2one: A foreign key to another model, representing a many-to-one relationship.
- One2many: A virtual relationship representing a one-to-many relationship. It's the reverse of a Many2one field.
- Many2many: Represents a many-to-many relationship between models.
- Special Fields: Such as Binary for binary data, Date and Datetime for dates and timestamps, Selection for a list of choices, etc.
### 3. ORM Methods:
- CRUD Operations:
1. create: To create new records.
2. write: To update existing records.
3. unlink: To delete records.
4. read: To read record data.
- Recordsets:
  Odoo uses recordsets, which are sets of records of the same model, to manipulate and navigate through records efficiently.
- Domain:
  Used for filtering records. It's a list of criteria used to select records.
### 4. Record Rules and Access Rights:
These are used to manage access to model records. Record rules can be applied to limit what records are visible to a user, while access rights manage what operations (create, read, update, delete) a user can perform.
### 5. Computed Fields:
Fields whose value is calculated using a method. They can be stored in the database or calculated on the fly.
### 6. Onchange Methods:
These methods are triggered when a field's value is changed in the form view, allowing dynamic changes in the UI.
### 7. Inheritance Mechanisms:
Odoo supports various inheritance mechanisms to extend or modify the behavior of existing models.
### 8. API Decorators:
@api.model, @api.multi, @api.one, etc., are decorators used to define the behavior of model methods.
Understanding and effectively using the Odoo ORM is fundamental to developing robust, efficient, and maintainable applications in Odoo. It abstracts the lower-level database interactions, allowing developers to focus on the business logic.



## 4. Record Rules and Access Rights in Odoo
> Overview
Odoo's security framework relies heavily on Record Rules and Access Rights to manage data access and maintain data security. These mechanisms ensure users access only the data they are authorized to, aligning with their roles and responsibilities.

### Record Rules 
Record Rules control which records of a model are visible or accessible to a user. They are part of Odoo's security layer.

###  Key Points
- Purpose: Restrict access to records based on specific conditions.
- Scope: Applicable to read, create, write, and delete operations.
### Types:
- Global Rules: Apply to all users.
- Group Rules: Apply only to specific user groups.
**Example**
Scenario: Managers should see only employee records of those who report to them in the hr.employee model.

Domain: [('parent_id', '=', user.id)]
Groups: Applied to a specific group like "Employee / Officer".
Operations: Limited to Read and Write.
## Access Rights
Access Rights are more straightforward and define the basic operations a user can perform on a model.

### Key Points
- Operations: Cover Create, Read, Write, Delete (CRWD).
Levels:
- Model Level: General access to any record of a model.
- Field Level: Access to specific fields in a model.
- Groups: Rights are assigned to user groups.
Example
For HR Managers in hr.employee model:

### Full access (CRWD).
- Group: Human Resources / Manager.
For Regular Employees:

### Limited to Read access.
- Group: Employee.
### Implementing Record Rules and Access Rights
Via Odoo Interface
- Access Groups: Edit group settings under 'Users & Companies' to set Access Rights.
- Create Record Rules: Under 'Technical' settings in 'Security', define new Record Rules with specific domains and applicable operations.
Via Data Files (XML/CSV)
- Define Access Rights and Record Rules in XML/CSV files: Place these under the 'security' directory of a custom module.
Example for Access Rights in XML:
```
<record id="access_hr_manager" model="ir.model.access">
    <!-- Access rights details here -->
</record>
```
Example for Record Rules in XML:
```
<record id="rule_hr_employee" model="ir.rule">
    <!-- Record rule details here -->
</record>
```
Update the Module: Apply the changes by updating the module in Odoo.


## 6.Onchange Methods:
> Onchange methods in Odoo are a crucial feature for dynamic user interfaces in form views. They are used to automatically update or modify the form's fields based on changes made by the user. These methods provide immediate feedback and interaction in the UI without requiring a server round trip or saving the record.

### Basic Concept of Onchange Methods:
- Triggered by UI Changes: Onchange methods are triggered when the value of a field in a form view is modified.
- Immediate Update: They allow for other fields in the form to be updated immediately based on the change.
- Not Stored: Changes made by the onchange methods are temporary and exist only until the form is saved. If the form is closed without saving, changes made by onchange methods are lost.
Usage and Functionalities:
- Dynamic Adjustments: Onchange methods can be used to auto-fill or adjust values of other fields based on the input of one field. For example, selecting a customer in a sales order might automatically update the delivery address field.

- Validation and Warnings: They can provide instant feedback, such as warnings or field value validation, before the user submits the form.

- Enhanced User Experience: By providing immediate feedback, onchange methods greatly enhance the user experience, making the interface more intuitive and responsive.

### Implementation:
Onchange methods are defined in Odoo models (Python classes) and are decorated with @api.onchange.

Example:
Consider a scenario in a sales module where selecting a customer should automatically update the customer's delivery address and loyalty points.

```
from odoo import api, fields, models

class SaleOrder(models.Model):
    _inherit = 'sale.order'

    loyalty_points = fields.Integer("Loyalty Points", readonly=True)

    @api.onchange('partner_id')
    def _onchange_partner_id(self):
        if self.partner_id:
            self.partner_shipping_id = self.partner_id.address_get(['delivery'])['delivery']
            self.loyalty_points = self.partner_id.loyalty_points
        else:
            self.partner_shipping_id = False
            self.loyalty_points = 0
```
In this example, the _onchange_partner_id method is triggered when the partner_id field changes. It updates the partner_shipping_id (delivery address) and loyalty_points fields based on the selected customer.

### Key Points to Remember:
- Not for Business Logic: Onchange methods are intended for UI interactions, not for enforcing business logic. Business rules should be implemented in other methods like constraints or compute methods.

- Temporary Changes: Remember that changes made in onchange methods are not saved until the user saves the form.

- Performance Consideration: Overusing onchange methods or writing heavy logic inside them can lead to performance issues. Keep the methods light and efficient.

> Understanding and correctly implementing onchange methods is vital for creating dynamic and user-friendly forms in Odoo applications. They play a key role in making the UI interactive and responsive to user input.

#### Best Practices
- Thorough Testing: Ensure new rules do not overly restrict necessary access.
- Documentation: Maintain clear documentation for future reference.
- Manage via Groups: Utilize user groups for easier access management.
#### Summary
> Record Rules provide detailed control at the record level, filtering accessible records based on conditions.
Access Rights offer basic CRUD permissions at the model level.
> Combining Both: Access Rights define basic model access, while Record Rules refine this to specific records.
> Implementation: Can be done via the Odoo interface or through XML/CSV in custom modules.
> This guide encapsulates the essentials of setting up and managing Record Rules and Access Rights in Odoo, ensuring both security and operational efficiency.
