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
