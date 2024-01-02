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
Implement your business logic in the Python method (e.g., do_something method in res.partner model).
The server action can be triggered from menus, buttons, or other actions.

## 4. Client Actions (ir.actions.client)
Client actions are used to execute client-side (JavaScript) actions.

### Example and Implementation:
#### Purpose: To execute client-side logic.
#### XML Definition: Typically defined within the JavaScript code and registered in the Odoo web client.
#### JavaScript Code: Implement a JavaScript action, then register it with Odoo's action manager.
### Business Logic:
Business logic is written in JavaScript and is executed on the client side.

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
> In each case, the implementation of business logic depends on the action type and the intended interaction within the Odoo application. Server actions offer the most flexibility for running custom logic, while automated actions are ideal for periodic tasks. Window and client actions focus more on user interaction and UI behavior.
