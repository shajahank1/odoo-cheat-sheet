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

