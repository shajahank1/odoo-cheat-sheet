# Actions
> In Odoo, actions are used to define what happens when a user interacts with different parts of the UI, such as menu items, buttons, or links. There are several types of actions, each serving specific purposes. Let's explore these types along with examples and implementation details.

## 1. Window Actions (ir.actions.act_window)
Window actions are the most common type, used to open a view, like a form or list view.

### Example and Implementation:
#### Purpose: To open a specific model's view.
#### XML Definition:
```
<record id="action_open_partners" model="ir.actions.act_window">
    <field name="name">Partners</field>
    <field name="res_model">res.partner</field>
    <field name="view_mode">tree,form</field>
</record>
```
#### Business Logic:
Window actions usually don't contain business logic themselves but are linked to views that can have buttons or fields that trigger server actions or methods for business logic.

> Implementing Window Actions (ir.actions.act_window) in Odoo is a fundamental part of customizing the user interface, particularly when you want to direct users to specific views or forms based on certain actions like clicking a menu item or a button. Window actions are used to open a specific model's view, such as a list, form, calendar, or pivot view.

### Steps to Implement Window Actions:
#### 1. Define the Action in XML:
First, you define the action in an XML file within your custom module. This XML will create a record in the ir.actions.act_window model.

XML Definition Example:
xml
Copy code
<record id="action_my_model_form" model="ir.actions.act_window">
    <field name="name">My Model</field>
    <field name="res_model">my.model</field>
    <field name="view_mode">form,tree</field>
    <!-- Optional: Specify individual view IDs -->
    <field name="view_id" ref="module_name.view_id_form"/>
</record>
In this example, the action opens the my.model model in both form and tree views. You can also specify specific views (form, tree, kanban, etc.) by their external IDs.

#### 2. Add the Action to a Menu (Optional):
To make the action accessible from the UI, you can link it to a menu item.

Menu Item Example:
xml
Copy code
<menuitem id="menu_my_model"
          name="My Model"
          action="module_name.action_my_model_form"
          parent="base.menu_custom"
          sequence="10"/>
This example creates a menu item named "My Model" which, when clicked, triggers the window action to open my.model. The parent attribute is used to specify where in the menu structure this item appears.

3. Link the Action to a Button (Optional):
Window actions can also be triggered by buttons in views.

**Button in View Example:**
```
<record id="view_my_model_form" model="ir.ui.view">
    <field name="name">my.model.form</field>
    <field name="model">my.model</field>
    <field name="arch" type="xml">
        <form>
            <!-- Form view content -->
            <button string="Open Action" type="object" name="%(module_name.action_my_model_form)d"/>
        </form>
    </field>
</record>
```
> In this form view for my.model, there's a button that, when clicked, executes the window action defined earlier.

### Key Points to Remember:
- Action Type: The type of action is ir.actions.act_window for window actions.
- View Modes: The view_mode field defines which types of views (form, tree, kanban, etc.) are available.
- Target (Optional): You can specify the target (e.g., new for opening in a dialog) in the action definition.
- Context (Optional): You can pass default values or other context information to the action using the <field name="context"> tag.
> Implementing window actions allows you to create a navigational structure and user workflows in your Odoo application, providing a seamless and integrated user experience.

______________________________________________________________
## 2. URL Actions (ir.actions.act_url)
> URL actions are used to redirect the user to a specific URL.
### Example and Implementation:
#### Purpose: To open a specific URL.
#### XML Definition:
```
<record id="action_open_odoo" model="ir.actions.act_url">
    <field name="name">Open Odoo Website</field>
    <field name="url">https://www.odoo.com</field>
    <field name="target">new</field>
</record>
```
### Business Logic:
They are primarily for navigation and don't involve direct business logic execution.
## 3. Server Actions (ir.actions.server)
Server actions allow you to execute Python code, making them versatile for implementing business logic.
### Example and Implementation:
#### Purpose: To execute custom Python code.
#### XML Definition:
```
<record id="action_server_example" model="ir.actions.server">
    <field name="name">Server Action Example</field>
    <field name="model_id" ref="base.model_res_partner"/>
    <field name="state">code</field>
    <field name="code">
        action = model.do_something(records)
    </field>
</record>
```
### Python Code: In the model (res.partner in this case), define do_something.
### Business Logic:

> Implement your business logic in the Python method (e.g., do_something method in res.partner model).
The server action can be triggered from menus, buttons, or other actions.

> URL Actions in Odoo (ir.actions.act_url) are used to redirect the user to a specific URL. This can be an external link or a link to another part of the Odoo application. URL Actions are particularly useful for integrating external web resources or directing users to specific web pages.

### Steps to Implement URL Actions:
#### 1. Define the URL Action in XML:
First, you need to define the action in an XML file within your custom module. This XML will create a record in the ir.actions.act_url model.

**XML Definition Example:**
```
<record id="action_open_website" model="ir.actions.act_url">
    <field name="name">Open Website</field>
    <field name="url">https://www.example.com</field>
    <field name="target">new</field>
</record>

```
> In this example, the action opens the URL https://www.example.com in a new window or tab (specified by the target field).

#### 2. Add the Action to a Menu (Optional):
To make the URL action accessible from the UI, you can link it to a menu item.

**Menu Item Example:**
```
<menuitem id="menu_open_website"
          name="Open Website"
          action="module_name.action_open_website"
          parent="base.menu_custom"
          sequence="10"/>
```
This example creates a menu item named "Open Website" which, when clicked, triggers the URL action to open the specified website.

#### 3. Link the Action to a Button in a Form View (Optional):
URL actions can also be triggered by buttons in form views.

**Button in Form View Example:**
```
<record id="view_form_example" model="ir.ui.view">
    <field name="name">example.model.form</field>
    <field name="model">example.model</field>
    <field name="arch" type="xml">
        <form>
            <!-- ... other form elements ... -->
            <footer>
                <button string="Go to Website" class="btn-primary" type="object" name="%(module_name.action_open_website)d"/>
            </footer>
        </form>
    </field>
</record>
```
> In this form view, a button is added that triggers the URL action when clicked.

### Key Points to Remember:
- Action Type: The type of action is ir.actions.act_url for URL actions.
- Target Field: The target field determines how the URL is opened (new for a new window/tab, self for the same window).
- Dynamic URL: You can dynamically generate the URL in a method and return an ir.actions.act_url action with the generated URL.
- Security Considerations: Be cautious about linking to external websites and consider the security implications.
> URL actions in Odoo provide a simple yet powerful way to integrate external resources and web-based functionality into your Odoo applications, enhancing the overall user experience and capabilities of your system.

________________________________

## 4. Client Actions (ir.actions.client)
Client actions are used to execute client-side (JavaScript) actions.

### Example and Implementation:
#### Purpose: To execute client-side logic.
#### XML Definition: Typically defined within the JavaScript code and registered in the Odoo web client.
#### JavaScript Code: Implement a JavaScript action, then register it with Odoo's action manager.
### Business Logic:
> Business logic is written in JavaScript and is executed on the client side.

> Client Actions in Odoo (ir.actions.client) are used to execute client-side (JavaScript) code. They are especially useful for extending or modifying the Odoo web client's behavior, like opening custom client-side views or executing JavaScript functions.

### Steps to Implement Client Actions:
#### 1. Define the Client Action in XML:
First, you define the client action in an XML file within your custom module. This XML will create a record in the ir.actions.client model.

**XML Definition Example:**

```
<record id="action_client_example" model="ir.actions.client">
    <field name="name">Client Action Example</field>
    <field name="tag">client_action_tag</field>
    <!-- Optional: Additional parameters -->
    <field name="params">{'key': 'value'}</field>
</record>
```
> In this example, the action is identified by a tag, which is used to link the action to JavaScript code.

#### 2. Implement JavaScript Code:
Next, you need to write the JavaScript code that will be executed when the action is triggered. This involves extending the appropriate JavaScript classes and registering your client action.

####  JavaScript Implementation:

```
odoo.define('module_name.ClientAction', function (require) {
    'use strict';

    var AbstractAction = require('web.AbstractAction');
    var core = require('web.core');

    var ClientAction = AbstractAction.extend({
        contentTemplate: 'ClientActionTemplate',

        start: function () {
            // Your client-side logic here
        },
    });

    core.action_registry.add('client_action_tag', ClientAction);

    return ClientAction;
});
```
> In this JavaScript code, you define a new client action by extending AbstractAction and add it to Odoo's action registry with the same tag defined in the XML.

#### 3. Link the Action to a Menu Item or Button (Optional):
Like other actions, client actions can be linked to menu items or buttons for triggering.

**Menu Item Example:**
```
<menuitem id="menu_client_action_example"
          name="Client Action Example"
          action="module_name.action_client_example"
          parent="base.menu_custom"
          sequence="10"/>
```
> This example creates a menu item that triggers the client action.

### Key Points to Remember:
- Tag: The tag field in the XML definition is crucial as it connects the action with the JavaScript implementation.
- JavaScript Extension: You typically extend AbstractAction or another relevant base class depending on the desired behavior.
- Templates: You can use QWeb templates (contentTemplate) for the action's HTML content.
- Action Registry: Register your client action in Odoo's action registry for it to be recognized and executable.
- Development Mode: JavaScript development in Odoo often requires running Odoo in development mode (--dev=assets) for the changes to be immediately visible.
> Client actions are a core part of Odoo's extensibility, allowing developers to create rich, interactive client-side applications and enhancements within the Odoo framework.

____________________________________________________________
## 5. Automated Actions (ir.actions.automated)
Automated actions (or scheduled actions) are used to execute actions automatically based on a schedule or a trigger.

### Example and Implementation:
#### Purpose: To automate tasks based on time or triggers (like record creation or update).
#### XML Definition: Define a scheduled action with a server action to be executed.
#### Cron Configuration: Set up a cron job that triggers the action periodically.
### Business Logic:
The business logic is typically contained in the server action or method that the automated action calls.
### Implementing Business Logic:
- Server Actions: The most common way to implement business logic. Write a Python method and trigger it via a server action.
- Automated Actions: Use for background tasks or regular maintenance jobs.
- Window Actions: For user-initiated actions, combine window actions with form views that have buttons linked to server actions or methods.
- Client Actions: Use for complex UI interactions that require custom JavaScript.

> Automated Actions in Odoo, commonly known as Automated Actions (ir.actions.server), are a powerful tool for automating various operations based on certain triggers like record creation, updates, or deletions. They are primarily used to execute server-side logic in response to changes in your Odoo database.

### Purpose of Automated Actions:
- Data-Driven Automation: Execute specific actions automatically when data in Odoo is created, updated, or deleted.
- Versatile Business Logic Implementation: Commonly used for sending notifications, creating or updating related records, data validation, etc.
## How to Implement Automated Actions:
### 1. Define the Server Action:
First, define a server action (Python code to be executed) in an XML file within your custom module.

**Server Action XML Definition Example:**
```
<record id="my_module.my_server_action" model="ir.actions.server">
    <field name="name">My Server Action</field>
    <field name="model_id" ref="model_my_model"/>
    <field name="state">code</field>
    <field name="code">
        # Python code to be executed
    </field>
</record>
```
### 2. Define the Automated Action:
Next, define the automated action specifying when it should be triggered.

**Automated Action XML Definition Example:**
```
<record id="my_module.my_automated_action" model="base.automation">
    <field name="name">My Automated Action</field>
    <field name="model_id" ref="model_my_model"/>
    <field name="trigger">on_create_or_write</field>
    <field name="action_server_id" ref="my_module.my_server_action"/>
</record>
```
### 3. Implement Business Logic in Python:
In the server action, implement the Python logic that should be executed automatically. This could include creating records, modifying data, sending emails, etc.

**Example:**
Let's say you want to automatically send an email to a customer when a new sale.order is confirmed.

**Server Action to Send Email:**

```
<record id="action_send_email" model="ir.actions.server">
    <field name="name">Send Email on Sale Order Confirmation</field>
    <field name="model_id" ref="sale.model_sale_order"/>
    <field name="state">code</field>
    <field name="code">
        if record.state == 'sale':
            # Code to send email
    </field>
</record>
```
> Automated Action Triggered on Sale Order Update:

```
<record id="automated_action_send_email" model="base.automation">
    <field name="name">Automated Email on Sale Order Confirmation</field>
    <field name="model_id" ref="sale.model_sale_order"/>
    <field name="trigger">on_update</field>
    <field name="filter_pre_domain">[('state', '=', 'sale')]</field>
    <field name="action_server_id" ref="action_send_email"/>
</record>
```

### Key Points to Remember:
- Trigger Types: Common triggers include on_create, on_write, on_create_or_write, and on_unlink.
- Filter Domain: You can specify a domain to further filter the records that trigger the action.
- Python Code Execution: The Python code is executed in the context of the affected records. You can access these records through the records variable in the server action's Python code.
> Automated Actions in Odoo provide a powerful way to implement complex workflows and business logic that react to changes in your data, enhancing the system's automation capabilities.


> In each case, the implementation of business logic depends on the action type and the intended interaction within the Odoo application. Server actions offer the most flexibility for running custom logic, while automated actions are ideal for periodic tasks. Window and client actions focus more on user interaction and UI behavior.
