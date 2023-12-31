# Menus in Odoo
> Menus in Odoo are hierarchical, and there are several types that you can define to organize the navigation for your module effectively. Here's a detailed explanation:

### Types of Menus in Odoo
- Main Menu (Top-Level Menu):
The highest level in the menu hierarchy.
Often represents a major functional area or module.
Visible at the top of the Odoo interface.
- Submenu:
Nested under a main menu or another submenu.
Used to organize related items under a common category.
- Action Menu:
Typically the lowest level in the menu hierarchy.
Linked to specific actions like opening a form view, tree view, or triggering a wizard.

### How to Define Menus
Menus in Odoo are defined in XML files. These files should be included in your module, typically in the views directory, and referenced in the module's __manifest__.py file.

**Example Structure:**
> Main Menu: "Hospital Management"
Submenus: "Patients", "Appointments"
Action Menus: Under "Patients", menus like "Patient Records", "Admissions"
XML Definitions:
**1. Defining the Main Menu:**
```
<menuitem id="main_hospital_menu"
          name="Hospital Management"
          sequence="10"/>
```
id: Unique identifier for the menu item.
name: Name displayed on the menu.
sequence: Determines the order of the menu (optional).  

**2. Defining a Submenu:**
```
<menuitem id="menu_hospital_patients"
          name="Patients"
          parent="main_hospital_menu"
          sequence="1"/> ```
parent: The id of the parent menu. This submenu will be nested under "Hospital Management".
```
**3. Defining an Action Menu:**
```
<menuitem id="menu_patient_records"
          name="Patient Records"
          parent="menu_hospital_patients"
          action="patient_records_action"
          sequence="1"/>
```
action: The identifier of the action to be triggered when this menu item is clicked. This action is defined separately and typically links to a view or a wizard.

**4. Linking Actions to Views:**

You'll also need to define actions that are associated with these menus. For instance, the action patient_records_action might be defined as:

```
<record id="patient_records_action" model="ir.actions.act_window">
    <field name="name">Patient Records</field>
    <field name="res_model">hospital.patient</field>
    <field name="view_mode">tree,form</field>
</record>
 ```

This action links to the hospital.patient model and specifies that both tree and form views should be available. 

### Integrating Menus into Your Module
**Create the XML File:**

Example file name: hospital_menus.xml.
Place this file in the views directory of your module.
Reference in __manifest__.py:

``` 
{
    # Other manifest details...
    'data': [
        # ... other data files ...
        'views/hospital_menus.xml',
        # ... other data files ...
    ],
    # Other manifest details...
}
```
By following these steps, you can effectively create and organize menus in your Odoo module, providing a clear and accessible user interface for the functionality you develop.

### Steps to Enable/Disable Menus Based on Privileges
**Define Security Groups:**
> Security groups are defined in XML within your module. These groups determine the level of access a user has.
Example: Defining a group for managing patients in a hospital module.
```
<record id="group_hospital_manager" model="res.groups">
    <field name="name">Hospital Manager</field>
    <field name="category_id" ref="module_category_hospital"/>
</record>
```
In this example, group_hospital_manager is a group for hospital managers.

**Assign Groups to Menus:**
> When defining menus in XML, you can specify which security group has access to each menu.
Example: Allowing only hospital managers to access the "Patient Records" menu.
```
<menuitem id="menu_patient_records"
          name="Patient Records"
          parent="menu_hospital_patients"
          action="patient_records_action"
          groups="group_hospital_manager"/>
```
Here, the groups attribute restricts the menu to users who belong to the group_hospital_manager.

**Creating Record Rules (Optional):**
> Record rules can further refine access at the data level, defining what records users can see or modify.
Example: Restricting doctors to only see their patients' records.
```
<record id="rule_doctor_patients" model="ir.rule">
    <field name="name">Doctor's Patients</field>
    <field name="model_id" ref="model_hospital_patient"/>
    <field name="domain_force">[('doctor_id', '=', user.id)]</field>
    <field name="groups" eval="[(4, ref('hospital.group_doctor'))]"/>
</record>
```
Reference Security Files in __manifest__.py:

Include your security group and record rule definitions in your module's manifest file.
```
{
    # Other manifest details...
    'data': [
        'security/hospital_security.xml',
        'security/ir.model.access.csv',
        # ... other data files ...
    ],
    # Other manifest details...
}
```
hospital_security.xml would contain your group and rule definitions, and ir.model.access.csv would define the basic access rights for each model.

#### Important Considerations
**Groups and Access Rights:**
> Define access rights carefully. Make sure that each user role has the necessary permissions for their job without excessive privileges.
**Testing:**
> After setting up security groups and assigning them to menus, test your configurations with users assigned to different groups to ensure that the access controls work as expected.
By using security groups and record rules, you can effectively control access to menus in Odoo, ensuring that users only see and interact with menus relevant to their roles and permissions.

## Displaying menus dynamically
> Displaying menus dynamically in Odoo typically involves conditionally showing or hiding menu items based on certain criteria, such as user groups, record data, or other dynamic conditions. Odoo doesn't directly support dynamically showing/hiding menu items purely based on real-time conditions (like record values) without some level of customization. However, there are approaches you can take to achieve similar results:

### 1. Based on User Groups (Standard Approach):
The most common way to dynamically display menus is based on user groups. As mentioned earlier, you can define which groups can see which menus in your XML:

```
<menuitem id="menu_example"
          name="Example Menu"
          parent="parent_menu_id"
          groups="group_id_1,group_id_2"/>
```
This menu will only be visible to users who belong to group_id_1 and group_id_2.

### 2. Custom Modules for Dynamic Menus:
For more advanced dynamic behavior (like showing/hiding menus based on other business logic), you would need to create a custom module. This can involve:

Overriding Methods: You might need to override methods that generate the menu structure. This is complex and requires a deep understanding of Odoo's internals.

Server Actions: Use server actions to check conditions and then redirect to different actions or provide warnings.

### 3. Using Automated Actions or Scheduled Actions:
While not directly affecting the menu display, you can use automated actions or scheduled actions to change user group memberships or other settings that influence which menus are visible to them.

### 4. JavaScript Customization:
For a UI-based approach, you could use custom JavaScript to show/hide menus. This would involve:

Creating a JavaScript module in your custom addon.
Using the Odoo web client's JS API to manipulate the DOM, showing or hiding menu items based on your criteria.
Example Scenario:
Imagine you have a menu item that should only be visible when a certain condition is met (like a specific configuration option is enabled). You could:

Create a configuration model to store this setting.
Use a server action or scheduled action to check this setting and add/remove users from a specific group based on the value.
The menu item visibility is controlled by whether users are in that group.

#### Conclusion:
> Dynamic menu visibility based on real-time conditions is a complex task in Odoo and generally requires custom development. The approach you choose will depend on your specific requirements and the complexity of the conditions you need to evaluate. For many use cases, controlling access through user groups is sufficient and much simpler to implement. For more complex scenarios, custom development, potentially involving server actions, scheduled actions, or JavaScript, will be necessary.

# Eg. of Dynamic Menu
> Let's dive into a more detailed example to illustrate how you can dynamically display menus in Odoo based on user groups, which is the standard approach. For this example, imagine we're working with an Odoo module for a bookstore. We want to display certain menus only to users who are part of the 'Manager' group.

### Step 1: Define User Groups
In your custom module, define user groups in an XML file. Let's create a group for bookstore managers:

bookstore_security.xml:

```
<odoo>
    <data>
        <record id="group_bookstore_manager" model="res.groups">
            <field name="name">Bookstore Manager</field>
            <field name="users" eval="[(4, ref('base.user_root'))]"/> <!-- Assigning admin for demo purposes -->
        </record>
    </data>
</odoo>
```
This defines a group called "Bookstore Manager".

### Step 2: Create Menu Items
Now, let's define some menu items, with certain menus being visible only to users in the 'Bookstore Manager' group.

bookstore_menus.xml:
```
<odoo>
    <data>
        <!-- Main Menu for Bookstore Module -->
        <menuitem id="main_menu_bookstore" name="Bookstore" sequence="10"/>

        <!-- Sub Menu visible to all users -->
        <menuitem id="menu_bookstore_books" name="Books" parent="main_menu_bookstore" sequence="1"/>

        <!-- Sub Menu visible only to Managers -->
        <menuitem id="menu_bookstore_management" name="Management" parent="main_menu_bookstore"
                  groups="module.group_bookstore_manager" sequence="2"/>
    </data>
</odoo>
```
In this file, menu_bookstore_books is visible to all users, but menu_bookstore_management is only visible to users in the 'Bookstore Manager' group.

### Step 3: Reference Security and Menu Files in __manifest__.py
Make sure Odoo knows about these files by referencing them in your module's __manifest__.py file:

```
{
    # other manifest details...
    'data': [
        'security/bookstore_security.xml',
        'views/bookstore_menus.xml',
        # other XML files...
    ],
    # other manifest details...
}
```
**How It Works**
**User Group Assignment:** Users who are part of the 'Bookstore Manager' group will see the "Management" submenu under "Bookstore". Other users will not see this menu.
**Dynamic Aspect:** If a user is added to or removed from the 'Bookstore Manager' group, the visibility of the "Management" menu will change accordingly.
### Conclusion
> This example demonstrates how to use user groups in Odoo to control menu visibility. It’s a simple yet powerful way to ensure that users only see menus relevant to their roles. For more dynamic control based on real-time conditions or complex business logic, custom development involving server-side logic, scheduled actions, or JavaScript would be required, as mentioned earlier.


> Creating truly dynamic menus in Odoo based on real-time conditions or complex business logic often requires a blend of server-side and client-side customizations. Let's delve into examples for each of the three approaches you're interested in: Custom Modules for Dynamic Menus, Using Automated Actions or Scheduled Actions, and JavaScript Customization.

## 1. Custom Modules for Dynamic Menus:
Imagine a scenario where a menu should only be visible when there are pending approvals in a workflow system.

### Server-Side Module:

You could create a custom module that adds a Boolean field to the res.users model, indicating whether there are pending approvals for that user.
A server-side method checks for pending approvals and updates this field accordingly.
```
from odoo import models, fields

class ResUsers(models.Model):
    _inherit = 'res.users'

    has_pending_approvals = fields.Boolean(default=False)

    def check_pending_approvals(self):
        # Logic to check for pending approvals
        # This could be a scheduled action
        for user in self.search([]):
            # Set 'has_pending_approvals' based on actual conditions
            user.has_pending_approvals = True  # or False, based on actual check
```
**XML Menu Item:**
The menu item is still tied to a user group, but this group's membership is dynamically managed by your server-side logic.
## 2. Using Automated Actions or Scheduled Actions:
Consider a scenario where you want a menu to be visible to users on their birthday.

**Scheduled Action:**
Create a scheduled action (cron job) in your custom module.
This action runs daily, checks the birthdates of users, and adds them to a special group if it's their birthday.
```
from odoo import models, fields
import datetime

class ResUsers(models.Model):
    _inherit = 'res.users'

    birthday = fields.Date()

    def assign_birthday_group(self):
        birthday_group = self.env.ref('your_module.group_birthday')
        today = datetime.date.today()
        users_with_birthday = self.search([('birthday', '=', fields.Date.to_string(today))])
        for user in users_with_birthday:
            user.groups_id |= birthday_group
```
**XML Menu Item:**
The menu is accessible to users in the "Birthday" group, which is managed by the scheduled action.
## 3. JavaScript Customization:
For a real-time UI-based approach, imagine displaying a menu item based on the content in the user's current view.

JavaScript:
Extend Odoo's web client JS to add logic that shows/hides a specific menu item based on UI content.
```
odoo.define('your_module.CustomMenu', function (require) {
    "use strict";

    var core = require('web.core');
    var Widget = require('web.Widget');

    var CustomMenu = Widget.extend({
        start: function () {
            this._super.apply(this, arguments);
            var self = this;
            // Custom logic to determine if the menu should be shown
            var should_show_menu = true; // Replace with actual condition
            if (should_show_menu) {
                this.$el.show();
            } else {
                this.$el.hide();
            }
        }
    });

    core.action_registry.add('your_custom_menu_action', CustomMenu);
});
```
**XML Action:**
The custom action is linked to the menu item. When the menu is clicked, the JavaScript code is executed.
```
<record id="action_your_custom_menu" model="ir.actions.client">
    <field name="name">Your Custom Menu</field>
    <field name="tag">your_custom_menu_action</field>
</record>
```
**Conclusion:**
These examples illustrate three different methods to achieve dynamic menu visibility in Odoo:  

**Custom Modules for Dynamic Menus:** Involves server-side logic to dynamically manage user group memberships based on business logic.  
**Using Automated Actions or Scheduled Actions:** Utilizes scheduled actions to adjust user group memberships based on time-based or event-based conditions.  
**JavaScript Customization:** Employs client-side scripting to dynamically show or hide menus based on real-time UI conditions or user interactions.  

Each method has its use cases and complexities, and the choice depends on the specific requirements and desired level of dynamism in the Odoo application.  

