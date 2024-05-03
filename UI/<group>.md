## <group>
> In Odoo's XML views, the <group> tag is a versatile and commonly used element for organizing fields into logical sections or groupings. It helps in structuring form views by visually separating different sets of information, making forms cleaner and easier to navigate.

### Key Properties of the <group> Tag
- string: Provides a label for the group, which can act as a section header. This label is displayed above the group if specified.
- colspan: Specifies the number of columns the group should span within the layout of its parent container. This is useful for layout control in complex forms.
- col: Determines how many columns are within the group. This can be used to arrange the fields inside the group into multiple columns, making efficient use of space.
- invisible: A boolean expression that, when evaluated to True, makes the group invisible. This can be dynamically controlled using domain expressions based on the state of the form or other fields.
- readonly: Similar to invisible, this attribute can make all fields within the group readonly based on a condition.
#### Example of <group> in Use
Let's consider an example form view for a model named example.employee with fields for personal information and job details. We can use groups to separate these areas clearly.

XML Definition
Suppose you are defining the form for example.employee:

```
<odoo>
    <data>
        <record id="view_form_example_employee" model="ir.ui.view">
            <field name="name">example.employee.form</field>
            <field name="model">example.employee</field>
            <field name="arch" type="xml">
                <form string="Employee Form">
                    <sheet>
                        <group string="Personal Information">
                            <field name="first_name"/>
                            <field name="last_name"/>
                            <field name="birthdate" widget='date'/>
                        </group>
                        <group string="Job Details" colspan="2">
                            <field name="department_id"/>
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

The form is divided into two groups: "Personal Information" and "Job Details".
Each group uses the string attribute to provide a clear label.
The "Job Details" group uses colspan="2", allowing it to expand across two columns in a grid layout if the parent container supports it.
#### Practical Usage
Groups are not just for visual grouping but also help in applying common properties to a collection of fields. For example, setting a group to be invisible or readonly based on user roles or other field values helps in managing complex form behaviors dynamically.

Here's how you might use dynamic attributes:

```
<group string="Admin Only Information" attrs="{'invisible': [('user_role', '!=', 'admin')]}">
    <field name="internal_notes"/>
</group>
```
In this configuration:

The group "Admin Only Information" and all its fields become invisible unless the user_role field equals 'admin'. This is useful for hiding sensitive information from non-admin users.
### Conclusion
> The <group> tag is a fundamental building block in Odoo's form view architecture, providing structure, organization, and conditional behavior to form layouts. By effectively using groups, you can create more organized, user-friendly, and conditionally interactive forms.
