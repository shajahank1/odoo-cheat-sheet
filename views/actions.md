# Actions
> Actions in Odoo are operations that are triggered by user interactions, like clicking a menu item or a button. They define what should happen when a specific event occurs. There are several types of actions in Odoo:

- #### Window Actions (ir.actions.act_window):
        These are the most common actions, used to open or display views (like forms, lists, kanbans, etc.).
        They define which model to access and how to display it (e.g., in a form view, a list view, etc.).
        Can be linked with menus to open specific records or views when the menu is clicked.
- #### URL Actions (ir.actions.act_url):
        Used to redirect the user to a specific URL.
        Often used for linking to external websites or documents.
- #### Server Actions (ir.actions.server):
        Trigger Python code execution.
        Useful for performing backend operations, like sending an email, updating records, or any other custom logic.
- #### Client Actions (ir.actions.client):
        Used to execute client-side (JavaScript) code.
        Allows for more dynamic and interactive user interfaces.
- #### Report Actions (ir.actions.report):
        Used to generate reports.
        Linked with QWeb report templates to render PDFs, HTML, or other formats.

  ## Triggering Actions
  Triggering actions in Odoo is a fundamental part of the system's interactivity and workflow management.
  Actions can be triggered in various ways, depending on their type and the specific business logic of the module.

  > Actions can be triggered mainly in three ways:
    - by clicking on menu items (linked to specific actions)
    - by clicking on buttons in views (if these are connected to actions)
    - as contextual actions on object

Here's an overview of the common methods used to trigger actions:
### 1. Through Menu Items:
The most straightforward way to trigger an action is by clicking on a menu item. When you define a menu item in XML, you link it to an action, which gets triggered when the menu item is selected.

**XML Definition Example:**
```
<menuitem id="menu_action_example"
          name="Example Action"
          action="action_example"/>
```**
**Action Definition:****
```
<record id="action_example" model="ir.actions.act_window">
    <field name="name">Example Action</field>
    <field name="res_model">your.model.name</field>
    <field name="view_mode">form</field>
</record>
```

### 2. On Button Clicks (in Forms):
Actions can be triggered by buttons in form views. You can define buttons in your form views' XML and link them to specific actions.

**Form View Button Example:**
```
<form>
    <header>
        <button string="Approve" type="object" name="action_approve"/>
        <button string="Cancel" type="object" name="action_cancel"/>
    </header>
    <!-- form fields -->
</form>
```
**Model Method:**
In your model, define the methods action_approve and action_cancel that are triggered when the buttons are clicked.

```
from odoo import models

class YourModel(models.Model):
    _name = 'your.model.name'

    def action_approve(self):
        # Logic for approval
        pass

    def action_cancel(self):
        # Logic for cancellation
        pass
```

### 3. Automated Actions and Server Actions:
Automated actions (ir.cron) and server actions (ir.actions.server) can be configured to trigger at specific times or based on certain events in the system.

**Automated Action (Cron Job):**
Automated actions are typically used for scheduled tasks (like nightly data updates).

```
<record id="ir_cron_your_cron_job" model="ir.cron">
    <field name="name">Your Cron Job</field>
    <field name="model_id" ref="model_your_model"/>
    <field name="state">code</field>
    <field name="code">model.your_cron_method()</field>
    <field name="interval_number">1</field>
    <field name="interval_type">days</field>
</record>
```
**Server Action:**
Triggered by various events, like record creation, update, or upon a button click.

```
<record id="action_server_example" model="ir.actions.server">
    <field name="name">Example Server Action</field>
    <field name="model_id" ref="your.model.name"/>
    <field name="state">code</field>
    <field name="code">
        # Python code to execute
    </field>
</record>
```
### 4. Workflow Actions (Odoo 14 and earlier):
In older versions of Odoo (14 and earlier), workflows were used to manage business processes. Workflows trigger actions based on state changes in documents.

### 5. URL Actions:
URL actions are triggered when a user clicks on a link that leads to an external URL or a different part of the Odoo application.

### 6. Client Actions (JavaScript):
Client actions are triggered in the web client, usually through custom JavaScript code. These are used for highly interactive UI elements.

#### **Conclusion:**
> Triggering actions in Odoo is a versatile mechanism, allowing for a wide range of interactions from simple menu clicks to complex, automated workflows. The method of triggering depends on the specific requirements of the application and the desired user experience.
  

  
