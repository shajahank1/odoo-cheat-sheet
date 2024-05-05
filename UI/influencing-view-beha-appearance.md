## influencing behaviour or apperance of the view
Let's explain each variable mentioned in the context of an Odoo view with examples:

### parent:
parent refers to the record that contains the current view. This is particularly useful when you are dealing with sub-views of relational fields.
For example, if you have a One2many field displaying multiple records related to a parent record, parent will refer to the parent record containing the sub-view.
Let's consider an example where you have a purchase order (purchase.order) with multiple order lines (purchase.order.line). When defining a sub-view for the order lines, parent would refer to the purchase order record.
### context:
context refers to the context of the current view, which contains additional information passed to the view during its rendering.
Context can include various parameters or flags that influence the behavior or appearance of the view.
For example, if you have a context variable named active_id that contains the ID of the currently selected record, you can access it in the view using context.get('active_id').
```
<field name="partner_id" domain="[('customer', '=', True)]" context="{'active_id': active_id}"/>
```
### uid:
uid stands for the user ID and represents the ID of the current user interacting with the view.
This variable allows you to implement user-specific behavior in views.
For example, you can use uid to filter records based on the current user's access rights or preferences.
Example:
```
<field name="user_id" domain="[('id', '=', uid)]"/>
```
Let's say we have a scenario where we want to filter a list of tasks based on the current user's assigned tasks. We can use the uid variable to achieve this.

```
<tree string="My Tasks" editable="bottom" decoration-danger="user_id != uid">
    <field name="name"/>
    <field name="deadline"/>
    <field name="user_id" invisible="1"/>
</tree>
```
In this example:

- We have a tree view displaying a list of tasks.
- We want to filter the tasks to only show those assigned to the current user.
- We use the uid variable to dynamically filter the tasks based on the current user's ID.
> The invisible attribute is used to hide the user_id field, which is not relevant for the user to see but is used for filtering.
This way, when the user accesses this view, they will only see the tasks assigned to them, as the list is filtered based on the current user's ID (uid).

### today:
today represents the current local date in YYYY-MM-DD format.
It allows you to perform date-related calculations or comparisons within the view.
For example, you can use today to filter records based on their creation or modification date.
```
<field name="deadline" domain="[('deadline', '>=', today)]"/>
```
### now:
now represents the current local datetime in YYYY-MM-DD hh:mm:ss format.
Similar to today, it allows you to perform datetime-related calculations or comparisons within the view.
For example, you can use now to display a timestamp indicating when a record was last updated.
```
<field name="last_update" string="Last Updated" widget="datetime" readonly="True" value="now"/>
```
These variables provide valuable context and information that can be leveraged in Odoo views to implement dynamic behavior, conditional visibility, or filtering based on user, date, or record attributes.


# Using form-view root attributes
> These are additional root attributes that can be added to the <form> view in Odoo to customize its behavior and appearance. Let's go through each attribute:

### nosearch:
This attribute is used to specify the view title, which is displayed only if you open an action that has no name and whose target is new (opening a dialog).
- Requirement: Optional
- Type: String
- Default: ''

```
  <form nosearch="Contact Information">
```

### create:
This attribute is used to disable or enable record creation on the view.
- Requirement: Optional
- Type: Boolean
- Default: True
```
<form create="False">
```
### edit:
This attribute is used to disable or enable record edition on the view.
- Requirement: Optional
- Type: Boolean
- Default: True
```
  <form edit="False">
```
### duplicate:
This attribute is used to disable or enable record duplication on the view through the Action dropdown.
- Requirement: Optional
- Type: Boolean
- Default: True
```
    <form duplicate="False">
```
### delete:
This attribute is used to disable or enable record deletion on the view through the Action dropdown.
- Requirement: Optional
- Type: Boolean
- Default: True
```
      <form delete="False">

```
### js_class:
This attribute specifies the name of the JavaScript component the webclient will instantiate instead of the form view.
- Requirement: Optional
- Type: String
- Default: ''
```
<form js_class="custom_form_view">
```
### disable_autofocus:
This attribute is used to disable automatic focusing on the first field in the view.
Requirement: Optional
Type: Boolean
Default: False
```
  <form disable_autofocus="True">

```
> These attributes provide additional customization options for <form> views in Odoo, allowing developers to control various aspects such as record creation, edition, duplication, deletion, autofocus behavior, and more.

## banner_route
> The banner_route attribute in Odoo is used to fetch HTML content from a specified route and prepend it to the view. Here's a breakdown of its functionality:

### Purpose:
The banner_route attribute allows you to dynamically inject HTML content into a view in Odoo.
This HTML content is fetched from a specified route on the server and is displayed above the main view of the page.
### Usage:
To use banner_route, you include it as an attribute in the XML definition of your view (e.g., <tree>, <form>, <kanban>).
The value of banner_route is set to the URL of the controller route from which HTML content will be fetched.
### Behavior:
When the view is rendered, Odoo fetches the HTML content from the specified route on the server.
The fetched HTML content is then prepended to the view, appearing above the main content of the page.
If the HTML content includes a <link> tag for a stylesheet, Odoo automatically moves it from its original location and appends it to the <head> section of the page.
### Interacting with Backend:
To interact with the backend (e.g., trigger actions), you can use <a type="action"> tags within the fetched HTML content.
These action tags allow users to trigger actions defined in the backend controller.
Limitations:
Only views extending AbstractView and AbstractController, such as Form, Kanban, and List views, can use the banner_route attribute.
### Example:
- In the example provided, the <tree> view has a banner_route attribute set to /module_name/hello.
- There's a corresponding Python controller route defined in the MyController class, which returns a JSON response containing HTML content when accessed.
The returned HTML content includes a <link> tag for a stylesheet and a <h1> tag with the text "hello, world".
> In summary, banner_route is a powerful attribute in Odoo for dynamically injecting HTML content into views, allowing for customization and dynamic rendering of pages based on server-side data or logic.

### Here's an example demonstrating the usage of the banner_route attribute in Odoo:

```
<form string="Example Form View" banner_route="/module_name/hello">
    <!-- Form fields go here -->
</form>
```
In this example, we have a <form> view with the banner_route attribute set to /module_name/hello. When this form view is rendered, Odoo will fetch HTML content from the specified route (/module_name/hello) and prepend it to the form view.
Python Controller Route:
```
from odoo import http
from odoo.http import request

class MyController(http.Controller):

    @http.route('/module_name/hello', auth='user', type='json')
    def hello(self):
        return {
            'html': """
                <div>
                    <link href="/module_name/static/src/css/banner.css" rel="stylesheet">
                    <h1>Hello, World!</h1>
                </div>
            """
        }
```
> In the Python controller, we define a route /module_name/hello that returns a JSON response containing HTML content. The HTML content includes a <link> tag for a stylesheet (banner.css) and an <h1> tag with the text "Hello, World!".When this route is accessed, Odoo will fetch the HTML content returned by this controller and prepend it to the form view.
- Result:When the form view is rendered, the HTML content fetched from the controller route will be displayed above the form fields. The stylesheet specified in the HTML content will be applied, and the text "Hello, World!" will be displayed in a heading (<h1>).This allows you to dynamically inject custom HTML content into your views based on server-side logic, providing flexibility and customization options in your Odoo applications.

