# widgets
> In Odoo, widgets are special UI components used within views to enhance the way fields are displayed and interacted with by users. They add specific functionalities to standard form fields, making them more interactive, visually appealing, or suitable for handling specific types of data.

### Key Concepts of Widgets in Odoo
Widgets are primarily used in form views but can also appear in tree or search views. They are specified directly in the field definition within the XML view through the widget attribute. The choice of widget can significantly alter how a field behaves or is displayed.

### Common Types of Widgets and Their Usage
Here’s a look at some commonly used widgets in Odoo and how to use them:

- monetary: Displays currency values with proper formatting and currency symbol. It requires a corresponding currency field on the model.Example:
```
<field name="price" widget="monetary"/>
<field name="currency_id" invisible="1"/>
```
- html: Renders HTML content stored in a field, allowing rich text formatting.Example:
```
<field name="description" widget="html"/>
```
- many2many_tags: Displays Many2many fields as tags, which can be clicked to remove or add items.Example:
```
<field name="tag_ids" widget="many2many_tags"/>
```
- date, datetime: Provides a picker for entering date and datetime values, respectively.Example:
```
<field name="start_date" widget="date"/>
<field name="appointment_time" widget="datetime"/>
```
- selection: Displays a dropdown list for fields defined with a selection of key-value pairs.Example:
```
<field name="state" widget="selection"/>
```
- handle: Adds a drag-and-drop handle to list views, used primarily with sequences for reordering items.Example:
```
<tree editable="bottom">
    <field name="sequence" widget="handle"/>
    <field name="name"/>
</tree>
```
- image: Used to display binary fields as images.Example:
```
<field name="image" widget="image"/>
```
- statusbar: Displays the status of an object in a horizontal bar, typically used with states.Example:
```
<field name="state" widget="statusbar" statusbar_visible="draft,confirmed,done"/>
```
- url: Turns a Char field into a clickable URL.Example:
```
<field name="homepage" widget="url"/>
```
- progressbar: Displays a field value as a progress bar, often used to show completion percentages.Example:
```
<field name="progress" widget="progressbar"/>
```
### How to Use Widgets
To use a widget in Odoo, you need to specify it in the field tag of your XML view as follows:

```
<field name="field_name" widget="widget_type"/>
```
The choice of widget depends on the type of data the field contains and how you want it to be presented or interacted with. It's important to ensure that the widget is compatible with the field type (e.g., using the image widget with a binary field, not with a Char field).

### Conclusion
Widgets are essential tools in the Odoo UI toolkit, providing enhanced user interaction and better data presentation. They allow developers to tailor the user experience according to business requirements, making the application both functional and user-friendly. By choosing the right widget for the right field, you can leverage Odoo's capabilities to create effective and intuitive interfaces.
