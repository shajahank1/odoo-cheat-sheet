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

> Automated Actions and Scheduled Jobs (Cron Jobs) in Odoo are powerful tools for automating tasks based on specific triggers or schedules.

### Automated Actions
Automated actions in Odoo are triggered by changes in the database, such as the creation, modification, or deletion of records.

**Purpose:**
To automate responses to changes in data.
Commonly used for things like sending notifications, updating related records, or enforcing business rules automatically when records are created or modified.
### Implementation:
-    Define Automated Action:
    1. Create a new record in the ir.actions.server model.
    2. Specify the trigger conditions (e.g., on creation, update).
- Specify Trigger Conditions:
    Define when the action should be triggered using Odoo's domain syntax.
- Link to a Server Action or Python Code:
    The automated action can execute a server action, which contains the Python code to be executed when the conditions are met.
**Example:**
Automatically setting a customer to a VIP status based on certain criteria:

```
<record id="action_mark_vip" model="ir.actions.server">
    <field name="name">Mark as VIP</field>
    <field name="model_id" ref="base.model_res_partner"/>
    <field name="state">code</field>
    <field name="code">
        if record.total_purchases > 10000:
            record.is_vip = True
    </field>
</record>
```
> This action would be linked to a condition, such as an update on the res.partner model where the total_purchases field is modified.

### Scheduled Jobs (Cron Jobs)
Scheduled Jobs or Cron Jobs in Odoo are used to perform tasks at regular intervals.

### Purpose:
To automate tasks that need to run periodically.
Common uses include generating reports, data synchronization, sending periodic notifications, or database maintenance tasks.
### Implementation:
- Define a Scheduled Action:
  Create a new record in the ir.cron model.
- Set Interval and Timing:
  Define how often the job should run (e.g., daily, weekly, monthly) and at what time.
- Link to Python Code:
  The scheduled action should be linked to a method in a model that contains the logic to be executed.
**Example:**
Creating a scheduled job to clear old log entries every night:

```
<record id="ir_cron_clear_logs" model="ir.cron">
    <field name="name">Clear Old Logs</field>
    <field name="model_id" ref="model_log_cleanup"/>
    <field name="state">code</field>
    <field name="code">model.clear_old_logs()</field>
    <field name="interval_number">1</field>
    <field name="interval_type">days</field>
    <field name="numbercall">-1</field>
</record>
```
> In this example, clear_old_logs is a method in the log_cleanup model that would contain the logic to identify and delete log entries older than a certain date.

### Key Points to Remember:
- Automated Actions for Data-Driven Tasks: Use these when you need to react to changes in your database.
- Scheduled Jobs for Time-Based Tasks: Use these for tasks that need to run at specific times or intervals.
- Testing and Monitoring: Both automated actions and scheduled jobs should be thoroughly tested and monitored, especially in a production environment, to ensure they perform as expected without unintended side effects.
- Performance Considerations: Be mindful of the potential impact on system performance, particularly for jobs that might consume significant resources.
> By leveraging these features, Odoo provides a flexible framework to automate a wide range of tasks, enhancing efficiency and enabling advanced functionalities in business workflows.
  _______________________________________________________________________________________________________________________0
## 4. Workflow Business Logic
> Although Odoo has deprecated the old workflow engine, you can still implement workflow-like logic using automated actions and server actions.
- State Management: Use fields to manage the state of a record.
- Actions on State Change: Trigger server actions when the state changes.

  > Workflow Business Logic in Odoo refers to the processes and rules that govern the flow of operations within an Odoo application. While Odoo removed the traditional workflow engine after version 10, the concept of workflows is still very much applicable through the use of automated actions, server actions, and status (state) fields in models.

### Implementing Workflow Business Logic:
- Use of Status (State) Fields:
    Define a selection field in your model to represent the different stages or statuses of the record's lifecycle. This field usually named state, acts as a tracker for the     record's current position in the workflow.
- Automated Actions for State Transitions:
    Use automated actions (ir.actions.server) to automatically perform actions when records enter certain states. For example, sending a notification, updating related records, or even triggering additional server actions.
- Server Actions for Business Rules:
    Implement server actions to encapsulate complex business logic. These actions can be triggered manually from the UI or automatically as part of state transitions.
- Custom Methods for Transition Logic:
    Define custom methods in your model for state transition logic. These methods can validate conditions, update fields, or even call other methods or server actions.
- UI Elements to Trigger Transitions:
    Use buttons in form views to allow users to manually move a record to the next stage in the workflow. These buttons can call your custom methods or server actions.
####  Example Scenario:
Consider a simplified approval workflow for a purchase order:

- Model Definition:

```
from odoo import models, fields, api

class PurchaseOrder(models.Model):
    _name = 'purchase.order'

    state = fields.Selection([
        ('draft', 'Draft'),
        ('submitted', 'Submitted'),
        ('approved', 'Approved'),
        ('rejected', 'Rejected')
    ], default='draft')

State Transition Methods:

def action_submit(self):
    self.state = 'submitted'

def action_approve(self):
    # Business logic for approval
    self.state = 'approved'

def action_reject(self):
    # Business logic for rejection
    self.state = 'rejected'
Form View Buttons:

```
<form>
  <!-- ... other form elements ... -->
  <button string="Submit" states="draft" type="object" name="action_submit"/>
  <button string="Approve" states="submitted" type="object" name="action_approve"/>
  <button string="Reject" states="submitted" type="object" name="action_reject"/>
</form>
```
? In this example, the purchase.order model has a state field representing its current stage in the approval workflow. Custom methods (action_submit, action_approve, action_reject) handle the transition between states, and buttons in the form view allow users to trigger these transitions.

### Key Points to Remember:
- Flexibility: While Odoo doesn't have a built-in workflow engine like in earlier versions, the current framework offers greater flexibility.
- Modularity: Keep your business logic modular and reusable across different models and workflows.
- UI Integration: Make sure your workflow is well-integrated with the user interface for a seamless user experience.
- Security and Access Rights: Ensure that the workflow respects user roles and permissions, allowing actions only to authorized users.
> Implementing workflow business logic in Odoo requires a combination of model design, server actions, UI elements, and potentially automated actions, providing a robust system to handle various business processes.

________________________________________________________________________________________
## 5. Onchange Methods
Use @api.onchange decorators for methods that should react to changes in the UI, providing dynamic feedback or auto-filling fields.
Onchange methods in Odoo are used within form views to dynamically update the content based on user interactions. They are particularly useful for providing immediate feedback or recalculating values when a user changes a field in a form.

### Purpose of Onchange Methods:
- Dynamic UI Updates: Automatically update or modify other fields in the form when a user changes a value.
- Real-time Feedback: Provide real-time feedback or validations without needing to save the record first.

### How to Implement Onchange Methods:
- 1. Define an Onchange Method in a Model:
In your Odoo model, create a method to handle the logic that should be executed when a specific field's value changes.  
The method should not return anything but can update other fields in the same record (self).
- 2. Use @api.onchange Decorator:
Decorate this method with @api.onchange, specifying the field names that trigger this method.  
When one of these fields is modified in the UI, Odoo will automatically call this method.
- 3. Implement the Logic:
Inside the method, update other fields based on the changed field. These changes are reflected immediately in the UI but are not saved to the database until the user saves the form.

**Example:**
Suppose you have a sale.order.line model and want to update the total price when the quantity or unit price changes:

```
from odoo import api, models

class SaleOrderLine(models.Model):
    _inherit = 'sale.order.line'

    @api.onchange('product_uom_qty', 'price_unit')
    def _onchange_price_calculation(self):
        self.total_price = self.product_uom_qty * self.price_unit
```
> In this example, _onchange_price_calculation is automatically triggered when product_uom_qty or price_unit is modified in the UI. It recalculates total_price, which is updated in the form immediately.

### Key Points to Remember:
- Temporary Changes: Changes made by onchange methods are not permanent and are only applied to the UI. They are saved to the database only when the user saves the form.
- Single Record: Onchange methods are designed to work on a single record (self). They are not meant for operations involving multiple records.
- No Side Effects: It's good practice to ensure onchange methods don't have side effects outside of the record they are meant to work on.
- Performance: Keep onchange methods efficient as they are triggered frequently during form interactions. Avoid heavy computations or database operations.
> Onchange methods are crucial for creating an interactive and dynamic user interface in Odoo, enhancing the overall user experience by providing immediate feedback and data consistency.
________________________________________________________________________________________
## 6. Computed Fields
Use @api.depends for fields whose value is computed based on other field values. This is crucial for keeping data consistent and up-to-date.

Computed fields in Odoo are fields whose values are calculated dynamically using specified methods. They are essential for displaying data that is derived from other fields without physically storing them in the database.

### Purpose of Computed Fields:
- Dynamic Data Calculation: Computed fields allow you to display data that is dynamically calculated from other fields.
- No Physical Storage: The values of computed fields are not usually stored in the database (unless specified).
- Real-Time Updates: They are updated in real-time when any of the dependent fields change.

### How to Implement Computed Fields:
- 1. Define a Computed Field in a Model:
Add a field to your Odoo model and specify its compute parameter, pointing to the name of the method that will calculate its value.
- 2. Use @api.depends Decorator:
Decorate the computation method with @api.depends, specifying the fields that it depends on. This ensures that the computed field is recalculated whenever any of these fields change.
- 3. Implement the Computation Logic:
Inside the computation method, write the logic to calculate the value of the field. This method typically sets the computed field's value for each record in self.

**Example:**
Consider a simple example where you have an invoice model with a computed field total_amount that sums up the amounts from all invoice lines:

```
from odoo import api, fields, models

class Invoice(models.Model):
    _name = 'invoice'

    invoice_line_ids = fields.One2many('invoice.line', 'invoice_id', string="Invoice Lines")
    total_amount = fields.Float(compute='_compute_total_amount', string="Total Amount")

    @api.depends('invoice_line_ids.amount')
    def _compute_total_amount(self):
        for record in self:
            record.total_amount = sum(line.amount for line in record.invoice_line_ids)
```
> In this example, the total_amount field is a computed field. Its value is the sum of the amounts of all related invoice.line records. The method _compute_total_amount is triggered whenever an amount field in any invoice.line record (related to this invoice) changes.

### Key Points to Remember:
- Store Computed Fields (Optional): By default, computed fields are not stored in the database. If you want to store them (for performance reasons or to make them searchable), you can add store=True in the field definition.
- Dependencies Are Crucial: The @api.depends decorator is crucial to ensure that the computed field is updated correctly and automatically.
- Read-Only: Computed fields are read-only by default. To allow users to manually change computed fields, you can use inverse methods.
- Performance Considerations: Be cautious with the computation logic, especially for non-stored computed fields, as complex calculations can impact the system's performance.
> Computed fields are an effective way to present dynamic and calculated data in Odoo applications, enhancing data presentation and ensuring real-time data consistency.

_____________________________________________________________________________________________
### Best Practices:
- Modularize Logic: Keep your business logic modular and reusable. Avoid duplicating code.
- Error Handling: Implement robust error handling and validation to ensure data integrity.
- Performance Consideration: Be mindful of the impact of your logic on performance, especially in methods that are called frequently or on large datasets.
- Security: Ensure that your methods respect Odoo's security and access control mechanisms.
> By utilizing these methods, you can implement a wide range of business logic in Odoo, from simple validations to complex, automated workflows. Remember to leverage Odoo's existing frameworks and tools, as they provide a solid foundation for most common business requirements.
