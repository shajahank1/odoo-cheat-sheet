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
</record> ```
This action links to the hospital.patient model and specifies that both tree and form views should be available.
