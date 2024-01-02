# Widget
https://www.odoo.com/documentation/17.0/developer/reference/frontend/javascript_reference.html#reference-js-widgets

> In Odoo, widgets are UI components used to enhance and control the way field values are displayed and interacted with in views (forms, kanban, list, etc.). They can greatly improve the user experience by providing more context-appropriate UI elements for different types of data.

## Types of Widgets and Their Functionalities:
### Char Widget:
- Functionality: Default widget for text fields.
- Usage: Used for basic text inputs.
Example Implementation:
```
<field name="name" widget="char"/>
```
### Text Widget:
- Functionality: Used for longer text fields, provides a textarea.
- Usage: Ideal for descriptions or any long-form text.
Example Implementation:
```
<field name="description" widget="text"/>
```
### Many2one Widget:
- Functionality: Used for fields that reference another model.
- Usage: Select a single record from another model.
Example Implementation:

```
<field name="partner_id" widget="many2one"/>
```
### One2many and Many2many Widgets:
- Functionality: Display and manage one-to-many and many-to-many relationships.
- Usage: Handle linked records, displaying them in a list or a kanban view.
Example Implementation:

```
<field name="order_line" widget="one2many_list"/>
```
### Selection Widget:
- Functionality: Provides a dropdown selection for fields with a selection option.
- Usage: Choose one option from a predefined list.
Example Implementation:
```
<field name="state" widget="selection"/>
```
### Date and DateTime Widgets:
-Functionality: For date and datetime fields, showing a date picker.
- Usage: Selecting dates and times.
Example Implementation:
```
<field name="date_start" widget="date"/>
<field name="datetime_end" widget="datetime"/>
```
### Float Widget:
- Functionality: Used for decimal fields.
- Usage: Displaying and inputting floating-point numbers.
Example Implementation:
```
<field name="price" widget="float"/>
```
### Monetary Widget:
- Functionality: Used for currency values, often accompanied by a currency field.
- Usage: Display monetary values with currency symbol.
Example Implementation:
```
<field name="amount_total" widget="monetary"/>
<field name="currency_id" invisible="1"/>
```
### Binary Widget:
- Functionality: For binary fields (like file uploads).
- Usage: Uploading and displaying files or images.
Example Implementation:
```
<field name="document" widget="binary"/>
<field name="image" widget="image"/>
```
### Statusbar Widget:
- Functionality: Visual representation of states (like stages or statuses).
- Usage: Shows the progress or current state of a record.
- Example Implementation:

```
<field name="state" widget="statusbar"/>
```

### Best Practices for Using Widgets:
- Context-Appropriate: Choose widgets that best represent the nature of the data and the user interaction required.
- User Experience: Use widgets to simplify user interactions and make data entry more intuitive.
- Performance: Be mindful of widgets that might load additional data (like relational fields) and their impact on view loading times.
- Custom Widgets: For more specialized needs, consider developing custom widgets using JavaScript.
### Where to Use Them:
> Widgets can be used in various view types - form, list, kanban, etc. The choice of widget often depends on the view type and the specific data interaction required.
- Form Views: For detailed record creation and editing.
- List Views: For displaying records in a table-like format.
- Kanban Views: For a card-based representation, often used in stages.
> By carefully selecting and implementing widgets, you can significantly enhance both the functionality and usability of your Odoo applications, leading to a better overall user experience.

## field decorations
> In Odoo, field decorations are used to visually enhance fields in tree views based on certain conditions. These decorations allow you to apply different styles to field values, like colors or font styles, depending on the data they contain. This feature is particularly useful for quickly conveying information about record status or values without requiring users to delve into details.

### Types of Field Decorations:
- decoration-bf (Bold and Faded):
Makes the text bold if the condition is true; otherwise, it appears faded.
Example: <field name="priority" decoration-bf="priority=='high'"/>
- decoration-danger (Red Text):
Changes the text color to red when the condition is met.
Example: <field name="status" decoration-danger="status=='overdue'"/>
- decoration-info (Light Blue Text):
Applies a light blue text color based on the condition.
Example: <field name="state" decoration-info="state=='draft'"/>
   decoration-muted (Grey Text):
Renders the text in grey if the condition is true.
Example: <field name="active" decoration-muted="not active"/>
- decoration-primary (Dark Blue Text):
Changes text color to dark blue when the condition is met.
Example: <field name="type" decoration-primary="type=='important'"/>
- decoration-success (Green Text):
Applies green text color based on the specified condition.
Example: <field name="is_paid" decoration-success="is_paid==True"/>
- decoration-warning (Orange Text):
Turns the text color orange when the condition is true.
Example: <field name="temperature" decoration-warning="temperature > 30"/>

### How to Implement Field Decorations:
> To implement field decorations, you need to specify them in the XML definition of a tree view.
Example:
Consider a scenario where you want to apply decorations in a tree view of tasks:

```
<tree>
    <field name="name"/>
    <field name="priority" decoration-bf="priority=='high'"/>
    <field name="deadline" decoration-danger="deadline &lt; today"/>
    <field name="state" decoration-info="state=='draft'"/>
</tree>
```
In this example:
- Tasks with high priority will be displayed in bold.
- Tasks with a deadline before today will have their deadline displayed in red.
- Tasks in the 'draft' state will have their state displayed in light blue.
### Best Practices:
- Use Sparingly: Decorations should be used sparingly to avoid overwhelming users with colors and styles.
- Enhance User Experience: Use them to highlight important information or statuses that require immediate attention.
- Test for Clarity: Ensure that the decorations do not make the tree view cluttered or hard to read.
> Field decorations in Odoo are a simple yet powerful tool to enhance data readability in tree views, enabling users to quickly grasp the status or significance of records at a glance.


### how to implement dynamic decoration based on values
> Implementing dynamic decorations in Odoo based on field values is a powerful feature for enhancing the readability and visual appeal of list views. You can apply different styles (like colors or bold text) to fields based on specific conditions. This dynamic decoration is usually defined within the tree view of a model in Odoo.

### Steps to Implement Dynamic Decorations:
- 1. Define the Tree View:
Start by defining the tree view for your model in an XML file.
Use the <tree> tag to start the tree view definition.
- 2. Add Fields to the View:
Include the fields you want to display in the tree view using <field> tags.
- 3. Apply Decorations:
Use the decoration attributes (decoration-bf, decoration-danger, decoration-muted, etc.) on the fields.
Define a condition for each decoration attribute. This condition should be a domain-like expression that evaluates to either True or False based on the field's value.
**Example:**
> Consider an example where you want to decorate the 'state' field in a list view of tasks based on their current status:

```

<record id="view_task_list_custom" model="ir.ui.view">
    <field name="name">task.list.custom</field>
    <field name="model">project.task</field>
    <field name="arch" type="xml">
        <tree string="Tasks">
            <field name="name"/>
            <field name="user_id"/>
            <field name="planned_date"/>
            <field name="state" 
                   decoration-info="state == 'draft'" 
                   decoration-muted="state == 'cancelled'" 
                   decoration-danger="state == 'overdue'" 
                   decoration-success="state == 'done'"/>
        </tree>
    </field>
</record>
```
**In this example:**
- Tasks in the 'draft' state will have their state displayed in light blue (decoration-info).
- Tasks in the 'cancelled' state will have their state displayed in grey (decoration-muted).
- Tasks in the 'overdue' state will have their state displayed in red (decoration-danger).
- Tasks in the 'done' state will have their state displayed in green (decoration-success).
### Best Practices:
- Clarity and Readability: Use decorations to enhance clarity and readability, not to overwhelm users with too much color or styling.
- Meaningful Use: Apply decorations in a way that provides meaningful insights at a glance, such as highlighting overdue tasks or differentiating between statuses.
- Testing: Test your tree view in different scenarios to ensure that the decorations work as expected and the list view remains user-friendly.
- Dynamic field decorations are a great way to make list views in Odoo more informative and visually appealing, enhancing the overall user experience.
