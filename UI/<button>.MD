## <button>
In Odoo's XML views, the <button> tag is used to create interactive buttons on forms, tree views, and other view types. These buttons can be configured to perform a wide range of actions, from simple record modifications to triggering complex business logic defined in Python.

### Key Properties of the <button> Tag
- name: Specifies the name of the method in the model that the button should call when clicked. This is typically a method defined in the model's Python class.
- string: The label of the button, displayed as the button text in the UI.
- type: Defines the type of action the button will perform. Common types include:
- object: Calls a method on the current model.
- action: Executes an action, such as opening a new view or returning a report.
- url: Directs the user to a specified URL.
- class: Adds custom CSS classes to the button for styling purposes. Common classes include oe_highlight (makes the button more prominent, usually used for the primary action) and oe_link (styles the button as a hyperlink).
- icon: Specifies an icon to display on the button, enhancing its visual appeal and user understanding.
- confirm: Adds a confirmation step before executing the button's action. This property takes a string, which is displayed as the confirmation message.
- help: Provides a tooltip on hover, offering additional information about the button's function.
- states: Controls the button's visibility based on the record's state. This property takes a comma-separated list of states in which the button should be visible.
#### Example: Employee Record Approval Button
Here’s an example of using a <button> in an employee form view to approve an employee record. The button triggers a method in the Python model that changes the record’s state to "approved".

```
<odoo>
    <data>
        <record id="view_form_employee_approve" model="ir.ui.view">
            <field name="name">employee.form.approve</field>
            <field name="model">hr.employee</field>
            <field name="arch" type="xml">
                <form string="Employee">
                    <header>
                        <button name="action_approve" type="object"
                                string="Approve" class="oe_highlight"
                                confirm="Are you sure you want to approve this employee?"
                                help="Click to approve the employee" states="draft"/>
                    </header>
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="job_id"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>
    </data>
</odoo>
```
#### In this example:

- The name attribute of the button (action_approve) refers to a method in the hr.employee model.
- The type attribute is set to object, indicating that the button will call a Python method.
- The confirm attribute adds a confirmation step with the message "Are you sure you want to approve this employee?"
- The class attribute uses oe_highlight to make the button visually prominent.
- The states attribute ensures that the button is only visible when the employee's state is "draft".
#### Python Method Definition
Here is an example of how the action_approve method might be defined in the hr.employee model:

```
from odoo import api, fields, models

class Employee(models.Model):
    _name = 'hr.employee'

    state = fields.Selection([
        ('draft', 'Draft'),
        ('approved', 'Approved')
    ], default='draft')

    def action_approve(self):
        for record in self:
            record.state = 'approved'
```
#### Practical Usage
Buttons are extremely flexible and can be used for various purposes, from changing record states and opening modal dialogs to redirecting users or running reports. They are essential tools in creating interactive and dynamic Odoo applications.

### Conclusion
> The <button> tag in Odoo is a powerful element for adding interactivity to your applications. By leveraging its properties effectively, developers can enhance the user experience, streamline workflows, and enforce business rules directly through the UI.
