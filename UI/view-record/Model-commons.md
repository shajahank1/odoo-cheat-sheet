## Model Commons - ir.ui.view
This is the base model class for managing UI views in Odoo. It handles the storage, inheritance, and rendering of views such as forms, lists (trees), kanban boards, graphs, pivots, etc.

### Method: get_views(views, options=None)
This method is used to retrieve the structured data of multiple views simultaneously, along with associated model fields and potentially their filters.

#### Parameters:
- views: A list of tuples, each containing a view_id and a view_type (like 'form', 'tree', etc.). This specifies which views to fetch.
- options (dict): An optional dictionary that can include flags to modify the behavior of the method:
- toolbar: Includes contextual actions when loading fields_views.
- load_filters: Returns the model’s filters, which are the predefined search filters.
- action_id: Specifies an action to get filters for, otherwise loads global or model-specific filters.
#### Returns:
A dictionary containing fields_views (structured data for each requested view), fields (data structure of the model's fields), and optionally filters (if requested).
#### Behavior:
The method's output depends only on the requested view types, access rights, view access rules, and certain context values (like lang and TYPE_view_ref). Other context values are not considered.
### Method: get_view([view_id | view_type='form'])
This method retrieves the detailed composition of a specific view, taking into account all its inheritances and extensions.

#### Parameters:
- view_id (int): The ID of the view to fetch. If None, the method will return the view based on the view_type.
- view_type (str): Specifies the type of view if no view_id is provided. Defaults to 'form'.
options (dict): May contain boolean options like mobile, which indicates if the web client is using a responsive mobile view.
#### Returns:
The complete composition of the requested view, including any inherited views and extensions.
#### Return Type:
A dictionary representing the view.
#### Raises:
- AttributeError: This error is raised if the inherited view uses an unknown position attribute (anything other than 'before', 'after', 'inside', 'replace'), or if tags other than position are found in the parent view.
- InvalidArchitectureError: Raised if an unsupported view type is defined on the structure (other than the standard types like form, tree, calendar, search, etc.).
### Real-World Example Usage
Let's consider a scenario where you might use these methods. Suppose you are developing a custom module and need to adjust views based on user roles dynamically:

  1. Use get_views to fetch current form and list views of a model like res.partner with custom filters applied based on the user's role.
  2. Use get_view to fetch and possibly modify the form view of res.partner depending on whether the user accesses it from a mobile device or a desktop.
These methods are particularly useful for developers who need to dynamically adjust the UI based on complex business logic or user context, ensuring that the UI always meets the current operational requirements.



> Let's delve into more practical examples to demonstrate how the get_views and get_view methods from Odoo's ir.ui.view model can be utilized in real-world scenarios, emphasizing how developers might use these methods to dynamically adjust and fetch details about UI views based on certain conditions.

### Example 1: Using get_views to Fetch Multiple Views with Filters
Suppose you're building a dashboard in an Odoo module where you need to display multiple views of the res.partner model, tailored to the current user's permissions. You also want to include the predefined search filters for each view based on the user's specific actions.

Scenario
You want to dynamically fetch the form and tree views of the res.partner model, including toolbar actions and specific filters that the user can use.

Python Code
In your custom module, you might have a controller or model method that looks something like this:

```
def fetch_partner_views(self):
    # Assume self is an instance of an Odoo model with access to environment
    views = [(False, 'form'), (False, 'tree')]
    options = {
        'toolbar': True,
        'load_filters': True,
        'action_id': self.env.ref('contacts.action_contacts').id
    }
    partner_views = self.env['ir.ui.view'].get_views(views, options)
    return partner_views
```
In this code:

- views is a list where each tuple specifies the type of view to fetch (form and tree). False indicates no specific view_id is provided.
- options includes flags to include toolbar actions, load specific filters related to the action action_contacts, and use global filters.
The method fetches and returns detailed views and filter configurations, which can be used to render specific UI components or inform the user of available actions.
### Example 2: Using get_view to Fetch a Mobile-Specific Form View
Imagine you're enhancing the user experience by providing a mobile-specific form view of res.partner. This view needs to be simplified or adjusted when accessed from a mobile device.

#### Scenario
Adjust the partner form view dynamically when the user accesses it from a mobile device, incorporating specific fields or layout suitable for smaller screens.

#### Python Code
Here's how you might define a method in your model to fetch the mobile-specific form view:

```
def fetch_mobile_partner_form(self):
    # This could be triggered from a mobile frontend or specific routing
    view_type = 'form'
    options = {'mobile': True}  # Assuming there's frontend logic to set this based on device type
    mobile_form_view = self.env['ir.ui.view'].get_view(view_type=view_type, options=options)
    return mobile_form_view
```
In this example:

- view_type specifies that we want a form view.
- options is set to indicate the client is currently using a responsive mobile view, which might change the behavior or content of the view.
- The method fetches the composition of the form view considering it's accessed from a mobile device, potentially using different layouts or simpler structures optimized for mobile use.
### Conclusion
These methods (get_views and get_view) are particularly useful for developers who need to dynamically manipulate and fetch UI configurations based on user roles, device types, or specific operational contexts within Odoo. They allow for a flexible, dynamic UI setup that adapts to different user needs and enhances overall system usability.


## practical usage?

> The practical usage of the get_views and get_view methods in Odoo can significantly enhance the flexibility and responsiveness of an application by tailoring user interfaces to specific needs and contexts. Here are a few real-world scenarios that demonstrate how these methods can be effectively employed:

### Scenario 1: Role-Based UI Customization
- Objective: Customize the appearance and functionality of a view based on the user's role within the organization.

### Practical Application:
Imagine a scenario where a company wants different user roles (e.g., Sales Manager vs. Salesperson) to see different fields and actions on the same customer form view.

#### Using get_views:
```
def customize_views_for_user(self):
    user = self.env.user
    view_types = [('form', 'res.partner')]  # Assuming res.partner is the model for customers
    options = {}
    if user.has_group('sales_team.group_sale_manager'):
        options['toolbar'] = True  # Managers get additional toolbar options for reporting
    views = self.env['ir.ui.view'].get_views(view_types, options=options)
    return views
```
In this code, managers get additional options in their toolbar, like shortcuts to generate reports directly from the customer view.
#### Using get_view:
```
def get_custom_form_view(self):
    user = self.env.user
    view_type = 'form'
    if user.has_group('stock.group_stock_user'):
        view_id = self.env.ref('custom_module.view_partner_form_stock_user').id
    else:
        view_id = self.env.ref('base.view_partner_form').id
    view_details = self.env['ir.ui.view'].get_view(view_id=view_id)
    return view_details
```
Here, stock users see a customized form tailored to their needs, perhaps with additional inventory-related information, while others see the standard form.
### Scenario 2: Mobile-Specific Views
- Objective: Optimize forms and interfaces for mobile devices.

### Practical Application:
Provide simplified forms with fewer fields and a more mobile-friendly layout when users access the application via mobile devices.

#### Using get_view:
```
def get_mobile_specific_view(self, is_mobile):
    view_type = 'form'
    options = {'mobile': is_mobile}
    partner_view = self.env['ir.ui.view'].get_view(view_type=view_type, options=options)
    return partner_view
```
Depending on whether the is_mobile flag is set, this method fetches a version of the form that is optimized for mobile devices, enhancing usability and performance on smaller screens.
### Scenario 3: Dynamic Filters Based on User Actions
- Objective: Dynamically adjust the filters applied to a list view based on the user's recent activities or preferences.

### Practical Application:
A user recently working on high-priority projects might prefer to see only those projects when they go to the project list view next time.

#### Using get_views:
```
def get_project_views_with_custom_filters(self):
    user_preferences = self.get_user_preferences(self.env.user)
    project_views = [(False, 'tree'), (False, 'form')]
    options = {
        'load_filters': True,
        'action_id': self.env.ref('project.action_project_task_burndown_chart').id
    }
    if user_preferences.get('filter_high_priority'):
        options['context'] = {'default_priority': 'high'}
    views = self.env['ir.ui.view'].get_views(project_views, options=options)
    return views
``
- This fetches the project views with an option to pre-load filters that focus on high-priority tasks, as per the user's preferences, potentially adjusted after their last session.
### Conclusion
> These methods offer a high degree of customization and dynamic interaction within the Odoo framework, making applications more adaptive and user-friendly. They are particularly useful in environments where user roles, device types, and individual preferences significantly impact how data should be presented and interacted with.
