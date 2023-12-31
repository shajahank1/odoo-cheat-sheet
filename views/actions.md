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
  

 # Examples
 ## 1. Window Actions (ir.actions.act_window):
 Window Actions in Odoo (ir.actions.act_window) are one of the most common and important types of actions. They are used to open or display views like forms, lists, kanbans, graphs, pivots, etc. These actions define how a specific model's data should be presented in the Odoo interface.

**Example: Creating a Window Action for a Model**
> Let's say you have a model named your.model.name and you want to create a window action to open a list (tree) view and a form view of this model.

#### Step 1: Define the Window Action
You define the window action in an XML file within your custom module. Let's name this file window_actions.xml:

```
<odoo>
    <data>
        <!-- Window Action to Open Your Model -->
        <record id="action_open_your_model" model="ir.actions.act_window">
            <field name="name">Your Model</field>
            <field name="res_model">your.model.name</field>
            <field name="view_mode">tree,form</field>
            <!-- Optionally specify individual views -->
            <field name="view_id" ref="your_module.your_model_tree_view"/>
            <field name="help" type="html">
                <p class="oe_view_nocontent_create">
                    Click to create a new record.
                </p>
            </field>
        </record>
    </data>
</odoo>
```
In this definition:

name is the action's display name.
res_model is the model to which the action applies.
view_mode specifies the view types to open with this action (e.g., tree,form for list and form views).
#### Step 2: Define the Views (Optional)
If you want to specify custom views (tree, form, etc.) for your model, define them separately:
```
<record id="your_model_tree_view" model="ir.ui.view">
    <field name="name">your.model.name.tree</field>
    <field name="model">your.model.name</field>
    <field name="arch" type="xml">
        <tree>
            <field name="field1"/>
            <field name="field2"/>
            <!-- other fields -->
        </tree>
    </field>
</record>

<record id="your_model_form_view" model="ir.ui.view">
    <field name="name">your.model.name.form</field>
    <field name="model">your.model.name</field>
    <field name="arch" type="xml">
        <form>
            <sheet>
                <group>
                    <field name="field1"/>
                    <field name="field2"/>
                    <!-- other fields -->
                </group>
            </sheet>
        </form>
    </field>
</record>
```
### Step 3: Link the Action to a Menu Item
To make the action accessible, link it to a menu item:

```
<menuitem id="menu_your_model"
          name="Your Model"
          action="action_open_your_model"/>
```
### Step 4: Add the Files to __manifest__.py
Make sure these files are included in your module's __manifest__.py file:

```
{
    'name': 'Your Module Name',
    'data': [
        'views/window_actions.xml',
        'views/model_views.xml',  // If you have separate view definitions
        // ... other data files ...
    ],
    // ... other manifest details ...
}
```
#### Conclusion
> With these steps, you have created a window action in Odoo that opens a list and form view for your model your.model.name. This action can be accessed via the specified menu item, providing a user-friendly way to interact with the data of your model. Window actions are integral to navigating and manipulating data in Odoo and are commonly used across various modules for effective data management and display.

## 2. Server Actions (ir.actions.server):
> Server Actions in Odoo (ir.actions.server) are powerful tools that allow you to execute custom Python code in response to certain triggers, such as a button click, a record creation, or a modification. These actions can perform a variety of tasks, like updating records, sending emails, or any complex logic that can be encoded in Python.

**Example: Creating a Server Action to Update a Field**
Let's say you have a model your.model.name with a field status. You want to create a server action that updates the status field to 'Processed' when triggered.

### Step 1: Define the Server Action
In an XML file within your custom module (e.g., actions.xml), define the server action:

```
<odoo>
    <data>
        <!-- Server Action to Update Status Field -->
        <record id="action_update_status" model="ir.actions.server">
            <field name="name">Update Status to Processed</field>
            <field name="model_id" ref="model_your_model_name"/>
            <field name="state">code</field>
            <field name="code">
                # Python code to execute
                if records:
                    records.write({'status': 'Processed'})
            </field>
        </record>
    </data>
</odoo>
```
In this XML:

id is a unique identifier for the action.
model_id refers to the model on which this action will operate.
state is set to code, indicating that the action will execute Python code.
code contains the Python code to be executed. Here, it updates the status field of the record(s) that triggered the action.
Step 2: Optionally Link the Action to a Button or a Menu Item
Server actions can be triggered in various ways, such as from a button in a form view or a menu item. For instance, to link it to a button:

```
<form>
    <!-- ... other form fields ... -->
    <footer>
        <button string="Mark as Processed" type="object" name="action_update_status"/>
    </footer>
</form>
```
This button, when clicked, will trigger the server action.

### Step 3: Add the Files to __manifest__.py
Make sure these files are included in your module's __manifest__.py:

```
{
    'name': 'Your Module Name',
    'data': [
        'views/actions.xml',
        # ... other data files ...
    ],
    # ... other manifest details ...
}
```
#### Conclusion
> With these steps, you have created a server action in Odoo that updates the status field of records to 'Processed'. Server actions are versatile and can be tailored to perform a wide range of tasks, depending on your business logic and requirements. Remember that the Python code in server actions should be written carefully, as it has the ability to perform significant changes in your Odoo database.

## 3. Client Actions (ir.actions.client):
> Client Actions in Odoo (ir.actions.client) are used to execute client-side (JavaScript) code. They are primarily employed for adding rich, interactive client-side features to your Odoo application. Unlike server actions, client actions are executed in the user's browser, allowing for dynamic updates to the UI without a full page refresh.

**Example: Creating a Client Action for a Custom Dashboard**
Suppose you want to create a client action that opens a custom dashboard in Odoo. This dashboard could display various statistics or custom content relevant to the user.

### Step 1: Define the Client Action
First, you'll define the client action in an XML file within your custom module. Let's call this file client_actions.xml:

```
<odoo>
    <data>
        <!-- Client Action for Custom Dashboard -->
        <record id="action_custom_dashboard" model="ir.actions.client">
            <field name="name">Custom Dashboard</field>
            <field name="tag">custom_dashboard_action</field>
            <field name="target">new</field>
        </record>
    </data>
</odoo>
```
In this definition:

name is the action's display name.
tag is a unique identifier for the client action, which will correspond to the JavaScript code that will be executed.
target specifies where the action should be displayed (new opens in a new window or tab).
### Step 2: Create the JavaScript File
Next, you need to write the JavaScript that will be executed by this client action. Create a JS file, say custom_dashboard.js, and include it in your module.

```
odoo.define('your_module.CustomDashboard', function (require) {
    "use strict";

    var AbstractAction = require('web.AbstractAction');
    var core = require('web.core');

    var CustomDashboard = AbstractAction.extend({
        contentTemplate: 'CustomDashboardTemplate',

        start: function() {
            // Your custom initialization or rendering code here
            console.log("Custom dashboard started!");
        },
    });

    core.action_registry.add('custom_dashboard_action', CustomDashboard);

    return CustomDashboard;
});
```
In this script, CustomDashboard extends AbstractAction. The start function is where you initialize or render your dashboard.

###  3: Add a QWeb Template (Optional)
If your client action involves rendering custom HTML, define a QWeb template. For instance, create a file templates.xml:

```
<odoo>
    <data>
        <template id="CustomDashboardTemplate">
            <!-- Your custom dashboard HTML here -->
            <div>
                <h1>My Custom Dashboard</h1>
                <!-- More content -->
            </div>
        </template>
    </data>
</odoo>
```
### Step 4: Add the Files to __manifest__.py
Include these new files in your module's __manifest__.py file:

```
{
    'name': 'Your Module Name',
    'data': [
        'views/client_actions.xml',
        'views/templates.xml',  // Include this line only if you have a QWeb template
    ],
    'qweb': [
        'static/src/xml/templates.xml',
    ],
    'web.assets_backend': [
        'your_module/static/src/js/custom_dashboard.js',
    ],
    // ... other manifest details ...
}
```
#### Conclusion
> With these steps, you've created a client action that triggers a custom JavaScript operation in Odoo. This example sets up a basic framework for a custom dashboard, but you can expand it with more complex logic and UI elements as needed for your application. Client actions are a powerful way to enhance the interactivity and user experience of your Odoo modules.

## 4. Report Actions (ir.actions.report):
> Report Actions in Odoo (ir.actions.report) are used for generating and presenting reports. These reports can be in various formats like PDF, HTML, Excel, etc. They are usually linked to QWeb templates, which define the layout and content of the report.

**Example: Creating a Report Action for a PDF Report**
Let's say you want to create a PDF report for the model your.model.name which lists some details of the records in a table format.

### Step 1: Define the Report Action
First, define the report action in an XML file within your custom module. Let's call this file report_actions.xml:

```
<odoo>
    <data>
        <!-- Report Action for Your Model -->
        <record id="action_report_your_model" model="ir.actions.report">
            <field name="name">Your Model Report</field>
            <field name="model">your.model.name</field>
            <field name="report_type">qweb-pdf</field>
            <field name="report_name">your_module.report_your_model_template</field>
        </record>
    </data>
</odoo>
```
In this definition:

name is the name of the report action.
model is the model for which the report is generated.
report_type specifies the type of report (in this case, qweb-pdf for a PDF report).
report_name is the technical name of the QWeb template used for the report.
Step 2: Create a QWeb Report Template
Next, create the QWeb template that defines the layout of your report. Create a file named report_template.xml:

```
<odoo>
    <template id="report_your_model_template">
        <t t-call="web.html_container">
            <t t-foreach="docs" t-as="doc">
                <t t-call="web.external_layout">
                    <div class="page">
                        <h2>Report for <t t-esc="doc.name"/></h2>
                        <!-- Your report content -->
                        <table class="table table-condensed">
                            <thead>
                                <tr>
                                    <th>Field 1</th>
                                    <th>Field 2</th>
                                    <!-- More fields -->
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <td><t t-esc="doc.field1"/></td>
                                    <td><t t-esc="doc.field2"/></td>
                                    <!-- More fields -->
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </t>
            </t>
        </t>
    </template>
</odoo>
```
### Step 3: Add the Files to __manifest__.py
Include these files in your module's __manifest__.py file:

```
{
    'name': 'Your Module Name',
    'data': [
        'views/report_actions.xml',
        'views/report_template.xml',
    ],
    // ... other manifest details ...
}
```
#### Conclusion
> With these steps, you've created a report action in Odoo that generates a PDF report for a specific model. The QWeb template defines the structure and layout of the report, and the report action ties this template to your model. You can customize the template further to match your reporting needs, including adding styles, images, and dynamic data elements based on the doc records.



