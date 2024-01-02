# Implementing business logics 
Implementing business logic in Odoo involves using its powerful ORM (Object-Relational Mapping) capabilities, server actions, automated actions, and other extension mechanisms. Let's go through the key methods for implementing business logic in Odoo:

## 1. Model Methods
> The most common way to implement business logic in Odoo is through methods in model classes. These methods can be triggered by user actions or server-side processes.

**CRUD Methods Override:** Override create, write, unlink, etc., to add custom behavior during these operations.
Custom Methods: Define your own methods in models for specific business logic. These can be triggered by buttons in views, server actions, etc.
**Example:**
```
from odoo import models, api

class MyModel(models.Model):
    _name = 'my.model'

    @api.model
    def create(self, vals):
        # Custom logic before creating a record
        record = super(MyModel, self).create(vals)
        # Custom logic after creating a record
        return record

    def my_custom_method(self):
        # Business logic here
```
_________________________________________________________________________________________________________________________
## 2. Server Actions
Server actions are used to execute Python code and are ideal for complex business logic. They can be triggered from UI elements like buttons.
**Example:**
- Define a server action in XML and link it to a method in a model.
- Execute the action from a button in a form view.

> Server Actions in Odoo are powerful tools that allow you to execute custom Python code in response to specific triggers. They are particularly useful for implementing complex business logic that goes beyond the capabilities of standard CRUD operations or field computations.

### Purpose of Server Actions:
- Execute Custom Python Code: They are used to run bespoke Python code, which can interact with the Odoo database, perform calculations, modify records, or integrate with external systems.
- Triggered by UI Elements: Server Actions can be triggered from various UI elements, like buttons in forms or tree views.
### How to Implement Server Actions:
- 1. Define the Server Action in XML:
First, you need to define the server action in an XML data file. This definition includes the action's name, the model it's related to, and the Python code to execute.
XML Definition Example:
```
<odoo>
    <data>
        <record id="action_example" model="ir.actions.server">
            <field name="name">Example Action</field>
            <field name="model_id" ref="model_res_partner"/>
            <field name="state">code</field>
            <field name="code">
                # Python code goes here
                # Example: Update a field on all records
                for record in model.browse(env.context.get('active_ids')):
                    record.write({'field_name': 'new value'})
            </field>
        </record>
    </data>
</odoo>
``` 
### 2. Trigger the Server Action:
Server Actions can be triggered in several ways, most commonly from buttons in form or tree views.

- Button in Form View:
```
<record id="view_res_partner_form" model="ir.ui.view">
    <field name="name">res.partner.form</field>
    <field name="model">res.partner</field>
    <field name="arch" type="xml">
        <form>
            <!-- ... other form elements ... -->
            <button string="Run Action" type="object" name="action_example" />
        </form>
    </field>
</record>
```
> In this example, a button is added to the res.partner form view. When clicked, it triggers the server action action_example.
### 3. Implement the Python Code:
The Python code in the server action can do various tasks, like creating, reading, updating, or deleting records, or even calling external APIs.

#### Python Code in Server Action:
- The code is written as a string in the <field name="code"> tag.
- Use model to refer to the model on which the action is executed.
- Use env to access the Odoo Environment, which provides access to all models.
> env.context gives you access to the current context, which can contain useful information like the current user or active record IDs (active_ids).
**Example Scenario:**
Suppose you want to create a server action for the res.partner model that updates a custom field is_verified to True for all selected partner records.

#### XML Definition:
```
<record id="action_verify_partners" model="ir.actions.server">
    <field name="name">Verify Partners</field>
    <field name="model_id" ref="base.model_res_partner"/>
    <field name="state">code</field>
    <field name="code">
        for partner in model.browse(env.context.get('active_ids')):
            partner.write({'is_verified': True})
    </field>
</record>
```
### Triggering the Action:
> You can link this action to a button in the res.partner views or trigger it from other server-side logic.
### Key Points to Remember:
- Context Awareness: Be aware of the context in which your server action is running, as it can significantly affect the logic (e.g., which records are active or selected).
- Security & Permissions: Ensure your server action respects Odoo's security rules. Consider the permissions of the user triggering the action.
- Error Handling: Implement robust error handling in your Python code to manage exceptions gracefully.
> Server actions are a versatile mechanism in Odoo for extending the system's functionality, automating processes, and integrating with external systems. They provide a high level of control and flexibility in implementing custom business logic.
____________________________________________________________________________________________________________________
## 3. Automated Actions and Scheduled Jobs
For business logic that needs to be executed periodically or based on certain triggers, use automated actions or define scheduled jobs (cron jobs).

**Automated Actions:** Set up actions that are triggered by data changes.
**Cron Jobs:** Schedule regular tasks like daily reports, data synchronization, etc.
**Example:**
- Define a cron job in XML to call a method at regular intervals.
- Automated action to check and update records when certain fields change.
## 4. Workflow Business Logic
> Although Odoo has deprecated the old workflow engine, you can still implement workflow-like logic using automated actions and server actions.
- State Management: Use fields to manage the state of a record.
- Actions on State Change: Trigger server actions when the state changes.

## 5. Onchange Methods
Use @api.onchange decorators for methods that should react to changes in the UI, providing dynamic feedback or auto-filling fields.

## 6. Computed Fields
Use @api.depends for fields whose value is computed based on other field values. This is crucial for keeping data consistent and up-to-date.

### Best Practices:
- Modularize Logic: Keep your business logic modular and reusable. Avoid duplicating code.
- Error Handling: Implement robust error handling and validation to ensure data integrity.
- Performance Consideration: Be mindful of the impact of your logic on performance, especially in methods that are called frequently or on large datasets.
- Security: Ensure that your methods respect Odoo's security and access control mechanisms.
> By utilizing these methods, you can implement a wide range of business logic in Odoo, from simple validations to complex, automated workflows. Remember to leverage Odoo's existing frameworks and tools, as they provide a solid foundation for most common business requirements.
