# widget

In Odoo, a widget is a component that defines how a field is displayed and interacted with in the user interface. Widgets can be applied to fields in views like forms, lists, or kanbans, altering the default behavior or representation of the field to provide a richer user experience.

## Key Points About Widgets
- Field-Specific: Different widgets are available for different types of fields (Char, Integer, Date, Many2one, etc.).

- UI Enhancement: Widgets enhance the standard input and display mechanisms. For example, a date field can be displayed with a calendar picker.

- Custom Widgets: Odoo provides a set of built-in widgets, but developers can also create custom widgets for specific needs.

- Contextual Use: The choice of widget depends on the context and the specific requirements of how a field should be presented or interacted with.

## Common Widgets in Odoo
### For Char and Text Fields:

widget='text': Multiline text box.
widget='html': Rich-text HTML editor, used for HTML fields.
### For Numeric Fields:

widget='monetary': Displays a currency symbol next to the number.
widget='float_time': Converts a float field into hours and minutes.
### For Date and Datetime Fields:

widget='datepicker': Provides a UI to pick dates from a calendar.
widget='datetimepicker': For selecting date and time.
### For Selection Fields:

widget='selection': Dropdown list for selection fields.
### For Binary (File) Fields:

widget='binary': Upload/download widget for files.
widget='image': Displays an image, used for binary fields storing image data.
### For Relational Fields:

widget='many2one': Dropdown list to select a record from a related model.
widget='many2many_tags': Displays Many2many fields as tags.
### For Boolean Fields:

widget='checkbox': A simple checkbox.
widget='toggle_button': Displays as a toggle button.
### Example Usage in Views
Here is an example of how widgets are used in a form view:

```
<form>
    <field name="date_field" widget="datepicker"/>
    <field name="amount" widget="monetary"/>
    <field name="description" widget="html"/>
    <field name="partner_id" widget="many2one"/>
</form>
```
### In this example:

The date_field is displayed with a datepicker.
The amount field shows a currency symbol.
The description field uses a rich-text editor.
The partner_id field is a dropdown to select a record from the related model.
### Considerations
- Compatibility: Ensure the widget is compatible with the field type.
- User Experience: Choose widgets that enhance usability and clarity.
- Performance: Some widgets, especially those involving complex interactions or large datasets, can impact performance.
- Widgets are an essential part of creating intuitive and effective user interfaces in Odoo, allowing for more interactive and user-friendly forms and views.


### For Text and Char Fields
widget='text': Multiline text area.
widget='html': HTML editor for rich text formatting.
widget='email': Widget for email fields, adds mailto: link.
widget='url': Turns the Char field into a clickable URL.
widget='char_domain': Used for fields representing a domain.
### For Numeric Fields
widget='float': Default widget for float fields.
widget='integer': Default widget for integer fields.
widget='monetary': Displays the currency symbol next to the number.
widget='float_time': Time duration in hours and minutes.
widget='percentage': Shows a percentage.
widget='progressbar': Shows a progress bar.
### For Date and Datetime Fields
widget='date': Date picker.
widget='datetime': Date and time picker.
### For Selection Fields
widget='selection': Default dropdown selection.
widget='radio': Displays options as radio buttons.
widget='priority': Displays as stars for priority selection.
widget='image': For binary fields, displays the binary data as an image.
widget='binary': File upload/download widget.
widget='document_viewer': Displays a document viewer for binary fields.
### For Boolean Fields
widget='checkbox': Default checkbox.
widget='toggle_button': Toggle switch.
widget='boolean_button': Displays a button that toggles a boolean field.
widget='statusbar': Displays the field as a status bar.
### For Relational Fields
widget='many2one': Default dropdown for Many2one fields.
widget='many2many_tags': Displays Many2many fields as tags.
widget='many2many': A list view for Many2many fields.
widget='many2many_binary': Used for Many2many fields to handle binary data like attachments.
widget='many2many_checkboxes': Displays Many2many fields as a list of checkboxes.
widget='many2many_kanban': Kanban view for Many2many fields.
widget='many2many_dropdown': Dropdown for Many2many field.
widget='handle': Used in tree views to make records sortable using drag-and-drop.
### Special Widgets
widget='kanban': Kanban view for One2many and Many2many fields.
widget='pivot': Pivot view for complex data analysis.
widget='graph': Graph view for data visualization.
widget='signature': Capture signatures.
widget='color': Color picker.
widget='ace': Code editor, useful for technical fields.
### Web Client Widgets
widget='mail_thread': Displays the mail thread and Chatter.
widget='web_editor': Full-featured WYSIWYG editor.
widget='image_url': Displays an image from a URL.
### Considerations
Not all widgets are suitable for every field type; ensure compatibility.  
The availability and behavior of widgets can vary between Odoo versions.  
Custom widgets can be created to extend or modify existing functionality.  
Always refer to the official Odoo documentation or source code for the most accurate and up-to-date information regarding widgets. 


# Custom Widgets
Custom widgets in Odoo are an advanced feature that allows developers to create their own widgets for specific use cases, extending or altering the default behavior of fields in the Odoo user interface. Custom widgets can be used to provide unique functionalities, visual presentations, or user interactions that are not covered by the standard set of Odoo widgets.

## Creating a Custom Widget
To create a custom widget in Odoo, you typically need to follow these steps:

- Define the Widget's JavaScript: Create a JavaScript file defining the behavior of the widget. This involves using Odoo's JavaScript framework.

- Add the Widget to the Asset Bundle: Include your JavaScript file in an asset bundle so it gets loaded into the Odoo web client.

- Use the Widget in Views: Specify your custom widget in the view definition by using the widget attribute on a field.

## Example of a Custom Widget
Let's consider a simple example where we create a custom widget to display ratings as stars.

### 1. Define the Widget's JavaScript
Create a file named static/src/js/star_widget.js with the following content:

```
odoo.define('your_module.StarRatingWidget', function (require) {
"use strict";

var AbstractField = require('web.AbstractField');
var fieldRegistry = require('web.field_registry');

var StarRatingWidget = AbstractField.extend({
    template: 'StarRatingWidgetTemplate',
    events: {
        'click .fa-star': '_onStarClicked',
    },

    // Render the stars
    _render: function () {
        var self = this;
        this.$el.empty();
        for (var i = 0; i < 5; i++) {
            var $star = $('<i>', {class: 'fa fa-star-o'});
            if (i < self.value) {
                $star.addClass('fa-star').removeClass('fa-star-o');
            }
            this.$el.append($star);
        }
    },

    // Handle click event on stars
    _onStarClicked: function (event) {
        var index = $(event.target).index();
        this._setValue(index + 1);
    },
});

fieldRegistry.add('star_rating', StarRatingWidget);

return StarRatingWidget;
});
```
In this JavaScript, we define a new widget StarRatingWidget that handles rendering and interaction of star ratings.

### 2. Add Widget to the Asset Bundle
Include the JavaScript file in your module's assets. Update your module's XML file (e.g., views/assets.xml) to include this JavaScript file:

```
<odoo>
    <data>
        <template id="assets_backend" name="your_module assets" inherit_id="web.assets_backend">
            <xpath expr="." position="inside">
                <script type="text/javascript" src="/your_module/static/src/js/star_widget.js"></script>
            </xpath>
        </template>
    </data>
</odoo>
```
### 3. Use the Widget in Views
Now, you can use your custom widget in Odoo views. For example, in a form view:

```
<field name="rating" widget="star_rating"/>
```
Here, rating is a field (e.g., an integer field) in your model, and you're applying the star_rating widget to it.

### Conclusion
> Creating custom widgets in Odoo involves defining their functionality and appearance using JavaScript and integrating them into the Odoo web client. This allows for a high degree of customization in how data is displayed and interacted with in the Odoo UI. Custom widgets are particularly useful when standard widgets do not meet the specific requirements of your application.
