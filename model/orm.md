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
