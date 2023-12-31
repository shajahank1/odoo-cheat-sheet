# Tree View
> A tree view in Odoo is a type of user interface that displays records in a list format. It is commonly used for showing multiple items where each item can be a record from a database. The tree view is especially useful for providing an overview of data, where users can see multiple records at once, often with the ability to sort and navigate through them.

### Key Characteristics of Tree View
- List-Like Format: Displays records in rows, similar to a table.
- Columns: Each column in a tree view corresponds to a field in the model.
- Sorting and Navigation: Users can often sort the list by clicking on column headers and navigate through records.
- Selectable Records: Rows in the tree view are typically selectable, allowing users to open a detailed form view of the record or perform actions on it.

### Defining a Tree View in Odoo
Tree views are defined in XML within Odoo modules. Here's an example:

```
<record id="view_model_tree" model="ir.ui.view">
    <field name="name">model.tree</field>
    <field name="model">your.model</field>
    <field name="arch" type="xml">
        <tree string="Your Model Tree View">
            <field name="field1"/>
            <field name="field2"/>
            <!-- Additional field tags -->
        </tree>
    </field>
</record>
```
In this example:

view_model_tree is the unique ID for this view.
model.tree is a descriptive name.
your.model should be replaced with the actual model name.
<tree> tag defines the tree view.
<field> tags inside the <tree> tag specify which fields to display as columns.
### Usage
- List Records: Often used for listing records like products, orders, employees, etc.
- Navigation: Allows users to quickly browse through records and select one to view more details or perform actions.
- Data Overview: Provides a high-level view of data, useful in various operational contexts like inventory management, sales order processing, or employee directory.  

> Tree views are fundamental in Odoo for presenting data in a clear, concise, and navigable format. They are widely used across various modules for their efficiency in displaying and interacting with large datasets.

## sorting the tree view
Sorting in a tree view in Odoo is managed through the model's Python code and the XML view definition. Here's how you can set up sorting for your tree view:

### 1. Default Sorting in the Model
In your model definition (Python file), you can specify the default sort order using the _order attribute. This attribute determines the default sorting of records when they are displayed in a tree view.

**Python Model Example**
```
class YourModel(models.Model):
    _name = 'your.model'
    _description = 'Your Model Description'
    _order = 'field_name ASC'  # Replace 'field_name' with your field's name

    field_name = fields.Char('Field Label')
    # other field definitions

```
In this example, records will be sorted by field_name in ascending order (ASC) by default.

### 2. Sorting in the Tree View
In the tree view XML definition, you can make columns clickable for sorting by setting the sortable="True" attribute in the <field> tag.

**XML View Example**
```
<record id="view_your_model_tree" model="ir.ui.view">
    <field name="name">your.model.tree</field>
    <field name="model">your.model</field>
    <field name="arch" type="xml">
        <tree string="Your Model Tree View">
            <field name="field_name" sortable="True"/>
            <!-- other fields -->
        </tree>
    </field>
</record>
```
In this tree view, the column for field_name will be sortable. Users can click on the column header in the tree view to toggle between ascending and descending sort order.

### 3. Combining Default Sorting with View Sorting
By combining the _order attribute in your model and sortable="True" in your tree view, you can define both the default sorting behavior and allow users to sort by different columns in the UI.

#### Usage
>Efficient Data Navigation: Sorting is essential for users to quickly find records, especially in views with large data sets.
Adaptability: Different users might need different sorting orders based on their tasks, so enabling sorting in the tree view adds flexibility to data navigation.

>By setting up sorting in both the model and tree view, you enhance the usability and efficiency of the tree view in Odoo, providing a better user experience for data navigation and management.

### Filtering the Tree/List
> To filter a list view by default in Odoo, you can define a default filter directly in the action that opens the view. This involves specifying a domain filter in the action's XML definition, which determines what records should be displayed when the view is initially opened. Here's how to do it:

#### Step 1: Define the Action
Actions in Odoo are used to define what happens when a user interacts with the system, such as opening a menu item. In this case, you want to define an action for opening your list view with a specific filter.

**XML Action Example**
```
<record id="action_your_model" model="ir.actions.act_window">
    <field name="name">Your Model</field>
    <field name="res_model">your.model</field>
    <field name="view_mode">tree,form</field>
    <field name="domain">[('your_field', '=', 'your_value')]</field>
</record>
```
In this example:  
your.model is the name of the model you're working with.  
The domain field is where you set your default filter. It uses Odoo's domain notation. Replace 'your_field' with the name of the field you want to filter by, and 'your_value' with the value you want to filter for.
#### Step 2: Link the Action to a Menu Item (Optional)
If you want this filtered view to be accessible from a menu, you need to link the action to a menu item.

**XML Menu Item Example**
```
<menuitem id="menu_your_model"
          name="Your Model"
          action="action_your_model"
          parent="base.menu_custom"/>
```
**In this example:**

action_your_model is the ID of the action you defined earlier.
base.menu_custom should be replaced with the ID of the parent menu under which you want this item to appear.
#### Usage
- **Efficient Data Presentation**: Default filters are particularly useful when there is a common subset of data that users frequently need to access, such as open tasks or pending orders.
- **Improved User Experience:** Providing a default filtered view saves time for users by pre-selecting the most relevant or commonly used data set.
> This method of setting default filters helps streamline user workflows in Odoo by presenting them with the most pertinent data as soon as they open a view.
