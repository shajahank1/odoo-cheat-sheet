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
