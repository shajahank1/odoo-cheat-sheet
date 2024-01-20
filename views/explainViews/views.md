# Views
In Odoo, views are one of the core components of the user interface, defining how data is presented and interacted with in the system. A view in Odoo is an XML configuration that specifies the layout and behavior of a user interface element associated with a model. Different types of views cater to different interaction paradigms, such as forms for editing, lists for browsing, and graphs for reporting.

## Types of Views
### Form View (form):

Used for displaying and editing individual records.
Can contain various widgets, fields, buttons, and even embedded views.
Example:
```
<form>
    <field name="name"/>
    <field name="description"/>
</form>
```
### List View (tree):

Displays multiple records in a tabular format (rows and columns).
Useful for browsing and quick editing of records.
Example:
```
<tree>
    <field name="name"/>
    <field name="date"/>
</tree>
```
### Kanban View (kanban):

Visual, card-based representation of records.
Often used for agile workflows, CRM pipelines, etc.
Example:
```
<kanban>
    <field name="name"/>
    <!-- Kanban card definition -->
</kanban>
```
### Search View (search):

Defines filters and group-by options for searching and categorizing records.
Example:
```
<search>
    <field name="name"/>
    <filter string="Active" name="active" domain="[('active','=',True)]"/>
</search>
```
### Calendar View (calendar):

Displays records in a daily, weekly, or monthly calendar.
Example:
```
<calendar string="Meetings" date_start="start_datetime">
    <field name="name"/>
</calendar>
```
### Graph View (graph):

Provides graphical representation (bar, line, pie charts) of data.
Example:
```
<graph>
    <field name="amount" type="measure"/>
</graph>
```
### Pivot View (pivot):

For complex reporting, similar to pivot tables in spreadsheet software.
Example:
```
<pivot>
    <field name="name" type="row"/>
</pivot>
```
## Defining Views
- XML Structure: Views are defined in XML and are linked to Odoo models. They are typically created in module files under the /views directory.
- Inheritance: Odoo supports view inheritance, allowing you to extend and modify existing views.
- Arch Field: The view's XML is stored in the arch field of the ir.ui.view model.
### Usage
- Views are used to create a user-friendly interface tailored to the needs of each application within Odoo.
- They can be customized extensively to fit the specific requirements of your business processes.
> By effectively utilizing different types of views, you can ensure that users have an intuitive and efficient interface for interacting with the data and functions of your Odoo application.




