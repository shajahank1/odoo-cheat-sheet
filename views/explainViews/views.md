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

-----
> Adding Form, List, and Search Views in Odoo involves defining these views in XML format for a specific model. Let's consider an example where we are adding views for a model named library.book.

## 1. Form View
The Form View is used for displaying and editing individual records.

views/library_book_form_view.xml:

```
<odoo>
    <record id="view_library_book_form" model="ir.ui.view">
        <field name="name">library.book.form</field>
        <field name="model">library.book</field>
        <field name="arch" type="xml">
            <form string="Book">
                <sheet>
                    <group>
                        <field name="name"/>
                        <field name="author"/>
                        <field name="isbn"/>
                    </group>
                </sheet>
            </form>
        </field>
    </record>
</odoo>
```
In this form view, fields name, author, and isbn of library.book are displayed for editing.

## 2. List View
The List View shows records in a table format, ideal for browsing.

views/library_book_list_view.xml:

```
<odoo>
    <record id="view_library_book_list" model="ir.ui.view">
        <field name="name">library.book.list</field>
        <field name="model">library.book</field>
        <field name="arch" type="xml">
            <tree string="Books">
                <field name="name"/>
                <field name="author"/>
                <field name="isbn"/>
            </tree>
        </field>
    </record>
</odoo>
```
This view lists books with their name, author, and isbn in a tabular format.

## 3. Search View
The Search View defines filters and group-by options for searching and categorizing records.

views/library_book_search_view.xml:

```
<odoo>
    <record id="view_library_book_search" model="ir.ui.view">
        <field name="name">library.book.search</field>
        <field name="model">library.book</field>
        <field name="arch" type="xml">
            <search string="Search Books">
                <field name="name"/>
                <field name="author"/>
                <filter string="Available" name="available" domain="[('available','=',True)]"/>
                <group string="Group By">
                    <filter string="Author" name="group_by_author" context="{'group_by': 'author'}"/>
                </group>
            </search>
        </field>
    </record>
</odoo>
```
In the search view, users can search books by name and author, and also filter them by availability and group them by author.

Including Views in the Module
Make sure to reference these view files in your module's __manifest__.py file under the 'data' key so that Odoo knows to load them when the module is installed.

library_management/__manifest__.py:

```
{
    'name': "Library Management",
    # ...
    'data': [
        # ...
        'views/library_book_form_view.xml',
        'views/library_book_list_view.xml',
        'views/library_book_search_view.xml',
    ],
    # ...
}
```
### Conclusion
> By defining these views, you create a user interface for the library.book model, allowing users to view, edit, search, and filter records in a user-friendly and efficient way. This setup is crucial for ensuring good user experience and effective data management in your Odoo application.



