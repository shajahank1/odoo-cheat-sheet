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
