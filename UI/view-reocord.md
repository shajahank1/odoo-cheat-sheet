# View Records
> In Odoo, "view records" are XML definitions that specify how data models are visually represented and interacted with in the user interface. Views are essential components of Odoo's architecture, dictating how users see and manipulate data across the system. They are defined in XML format and can be customized extensively to meet specific business requirements.

### Types of Views in Odoo
> Odoo supports several types of views, each designed for different purposes and data interactions:

- Form View: Used for displaying and editing a single record. It's the most detailed view type, allowing for in-depth interaction with all fields of a model.
- Tree View (List View): Displays multiple records in a list format. It is useful for overviewing many items at once and supports basic interactions like editing, deletion, and creation.
- Kanban View: Visualizes records in a card-based layout, commonly used for managing tasks or stages in a process (like a sales pipeline or project tasks).
- Graph View: Provides graphical representations of data, such as pie charts, bar charts, and line charts, which are helpful for reporting and analytics.
- Pivot View: Allows for complex data analysis and aggregation, similar to pivot tables in spreadsheet software.
- Calendar View: Shows records on a calendar based on date fields within the model, ideal for scheduling and planning.
- Gantt View: Used for displaying records on a timeline, suitable for project management and tracking.
- Search View: Configures the search interface for other views, determining how users can filter and search for records.
### Defining a View Record
To define a view, you create a record in the ir.ui.view model. This record specifies the type of view, the model it applies to, and the XML architecture that describes the view.

#### Example of a Form View Definition
Here’s how you might define a simple form view for a custom model called model.example:

```
<odoo>
    <data>
        <record id="model_example_form_view" model="ir.ui.view">
            <field name="name">model.example.form</field>
            <field name="model">model.example</field>
            <field name="arch" type="xml">
                <form>
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="description"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>
    </data>
</odoo>
```
### Attributes Commonly Used in View Records
- id: The XML ID used to reference the view elsewhere in the code or other views.
- model: Specifies the model (database table) that the view interacts with.
- arch: Contains the XML structure that defines the layout and elements of the view.
- inherit_id: Used when modifying an existing view. This specifies the view that is being extended or modified.
- priority: Determines the sequence of applying views if multiple views target the same model.
### How to Use and Manage View Records
View records are typically managed through Odoo's developer mode, where you can directly create, edit, and debug views. They are stored in the database but are initially defined in module XML files, which makes them part of a module's data files. When modules are updated or installed, these definitions are processed to reflect in the UI.

### Best Practices
- Keep It Simple: Avoid overly complex views that might confuse users or slow down the system.
- Use Inheritance: Instead of creating a new view from scratch, extend an existing one to maintain consistency and reduce redundancy.
- Test Views Thoroughly: Especially in dynamic environments where views pull data from various models, ensure your views behave as expected under all conditions.
- Understanding and utilizing view records effectively is crucial for customizing and enhancing the user experience in Odoo, ensuring that the interface meets the functional needs of the business.

# Attributes in View Record
Odoo views are defined with various attributes that dictate their behavior, appearance, and how they interact with data models. Here’s a detailed explanation of the most common attributes used in view records, along with examples to illustrate their use.

#### 1. id
The id attribute provides a unique identifier for the view within the Odoo system. This identifier is used to reference the view in actions, menus, other views, or anywhere else where the view needs to be specified.

```
<record id="view_model_form" model="ir.ui.view">
    ...
</record>
```
#### 2. model
This specifies the model (database table) that the view should operate upon. It links the view directly to a model so that the UI knows which data to display or manipulate.

```
<record id="view_model_form" model="ir.ui.view">
    <field name="model">res.partner</field>
    ...
</record>
```
#### 3. arch
The arch field holds the XML architecture of the view. This is where you define the structure, layout, and UI elements like fields, buttons, and groups.

```
<field name="arch" type="xml">
    <form>
        <sheet>
            <field name="name"/>
        </sheet>
    </form>
</field>
```
#### 4. inherit_id
Used for extending or modifying an existing view. This attribute references the external ID of the view that you are inheriting from.

```
<record id="view_model_form_custom" model="ir.ui.view">
    <field name="inherit_id" ref="base.view_partner_form"/>
    ...
</record>
```
#### 5. priority
Determines the order in which views are applied, especially when multiple views target the same model. Lower numbers have higher priority.



```
<record id="view_model_form_high_priority" model="ir.ui.view">
    <field name="priority" eval="5"/>
    ...
</record>
```
#### 6. groups_id
Specifies which security groups can see this view. This can be used to restrict access to the view based on user roles.


```
<record id="view_model_form" model="ir.ui.view">
    <field name="groups_id" eval="[(4, ref('base.group_user'))]"/>
    ...
</record>
```
#### 7. active
Determines whether the view is active. Inactive views are not used in the UI but remain in the database. Useful for temporarily disabling views without deleting them.

```
<record id="view_model_inactive_form" model="ir.ui.view">
    <field name="active" eval="False"/>
    ...
</record>
```
#### 8. name
A human-readable name for the view, used mainly for identification in the backend and in development tools, not shown to end users.

```
<record id="view_model_form" model="ir.ui.view">
    <field name="name">Partner Form View</field>
    ...
</record>
```
Combining Attributes in Practice
In practical applications, these attributes are combined to define complex views that are tailored to specific business needs. Here’s how a typical form view might look combining multiple attributes:

```
<odoo>
    <data>
        <record id="view_res_partner_form_inherit" model="ir.ui.view">
            <field name="name">res.partner.form.inherit</field>
            <field name="model">res.partner</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="priority" eval="20"/>
            <field name="arch" type="xml">
                <xpath expr="//field[@name='email']" position="after">
                    <field name="custom_field"/>
                </xpath>
            </field>
        </record>
    </data>
</odoo>
```
This example shows a form view extending the base partner form, adding a custom field right after the email field. It uses inherit_id to extend an existing view, priority to define its application order, and xpath within arch to specify where the new field should be placed.

Understanding and using these attributes effectively allows developers to create robust, customizable, and secure applications within the Odoo ecosystem.







