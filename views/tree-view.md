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
