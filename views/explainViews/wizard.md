# Wizards
Wizards in Odoo are transient models (TransientModel) that provide an interactive interface for complex user interactions. They are typically used for operations that are not straightforward to accomplish with standard forms or views.

### Key Features of Wizards:
- User Interaction: Great for scenarios where user input or choices determine the next steps or actions.
- Data Processing: Useful for processing data in a multi-step fashion, like importing/exporting data or configuring settings.
- Transient Nature: Data in wizards is temporary and not stored long-term in the database.
- Creating a Wizard:
### Define a TransientModel:

```
from odoo import models, fields

class MyWizard(models.TransientModel):
    _name = 'my.wizard'
    name = fields.Char("Name")
```
### Create Wizard Views:
You'll typically need a form view for the wizard. Optionally, you can include other view types like tree or kanban.

### Wizard Methods:
Implement methods to handle wizard logic, such as button click actions or record processing.

### Example of a Wizard Action:
In a custom module, you might define an action to open the wizard:

```
<record id="action_open_my_wizard" model="ir.actions.act_window">
    <field name="name">My Wizard</field>
    <field name="res_model">my.wizard</field>
    <field name="view_mode">form</field>
    <field name="target">new</field>
</record>
```
### Best Practices
- Use Advanced Views Appropriately: Choose the right type of view based on the data and user interaction required.
- Keep Wizards Simple: While wizards can handle complex scenarios, try to keep the user interface and steps as simple and intuitive as possible.
- TransientModel Cleanup: Remember that even though TransientModel data is temporary, cleaning up after a wizard's operation is good practice.
