# Web Controllers
In Odoo, a controller is a Python class that handles HTTP requests and responses. It plays a crucial role in web development, particularly for creating custom web pages, handling form submissions, or providing endpoints for AJAX requests. Controllers are an essential part of Odoo's web framework, enabling developers to extend and customize the web interface.

###  Key Aspects of Odoo Controllers:

####   HTTP Routing: 
  Controllers in Odoo use decorators to define routes, which are URL patterns that the Odoo web server listens to. When a request for a        specific URL is received, the corresponding controller function is executed.

#### Response Handling: 
  Controllers process incoming requests, interact with the Odoo ORM (Object-Relational Mapping) or other services, and generate responses,     which can be HTML pages, JSON data, or other formats.

#### Customization and Extension: 
  Controllers are commonly used to add custom web pages to Odoo or to create RESTful APIs for integration with external systems or for AJAX    interactions in Odoo web client.

### Example of an Odoo Controller:
  Let’s create a simple example of an Odoo controller that returns a JSON response.

#### Step 1: Define the Controller
  First, you create a new Python file in your module, typically under the controllers directory. Let's name it main.py.

```
# my_module/controllers/main.py
from odoo import http
from odoo.http import request

class MyController(http.Controller):
    @http.route('/my_url', type='http', auth='public')
    def my_method(self):
        return "Hello, this is my custom page!"

```
n this example, MyController is a Python class that inherits from http.Controller. The method my_method is decorated with @http.route, indicating that it's a routable method. The route is /my_url, the type is http, and auth='public' means that the route doesn't require authentication.

#### Step 2: Register the Controller
In the module’s __init__.py file, import the controller so that Odoo knows about it.
```
# my_module/__init__.py
from . import controllers
```
And in controllers/__init__.py:
```
# my_module/controllers/__init__.py
from . import main
```
#### Step 3: Use the Controller
Now, when you start your Odoo server and navigate to http://yourdomain.com/my_url, you will see the text "Hello, this is my custom page!".

###  Advanced Usage:
- Controllers can be more complex, handling form submissions, interacting with Odoo models, returning dynamic HTML content (often using QWeb templates), or providing JSON APIs for AJAX calls.

### Security Considerations:
- When creating controllers, especially ones that expose data or accept data submissions, consider security aspects like authentication, data validation, and access rights.

### Conclusion:
- Controllers in Odoo are powerful tools for web development, allowing you to extend the capabilities of your Odoo application, create custom web pages, and build integrations with other systems or services. They are an essential part of Odoo's architecture for developers looking to leverage web technologies within the Odoo ecosystem.

<details>
   <summary>  HTTP Routing: </summary>
  
> Routing in Odoo is a way to map URLs to Python methods in controllers. It's an essential part of creating web applications in Odoo because it determines how HTTP requests are handled and responded to. The @http.route decorator is used to define routes.

**Basic Example of Routing**
Here's a simple example:
```
from odoo import http

class MyController(http.Controller):
    @http.route('/my_url', auth='public')
    def my_method(self):
        return "Hello, Odoo!"
```
In this example, when a user navigates to /my_url on the Odoo server, the my_method function is called, and it returns a simple string.

### Parameters of the @http.route Decorator
- **_Route**/**URLs**: The first argument(s) are the route(s) or URL(s) the method will handle. You can pass a single string or a list of strings for multiple routes._

- **_auth**: Defines the authentication type. Common values are:_

  1. 'user': The user must be authenticated; redirects to the login page if not.
  2. 'public': The route is accessible to everyone, even if not logged in.
  3. 'none': No authentication is performed.
  4. type: Specifies the response type. It can be:

- '**http**': _For regular HTTP responses._
- '**json**': _For JSON responses (used in JSON-RPC)._
- **methods**: _A list of HTTP methods this route should handle (e.g., ['GET', 'POST']). If not set, all methods are allowed._
  
- **website**: _If set to True, the route is only accessible through the website and uses the website layout._  
   ```  @http.route('/post_example', auth='public', methods=['POST']) ```  
   ```  @http.route('/post_example', auth='public', methods=['GET']) ```
   > If you don't specify the methods parameter, the route will accept all HTTP methods. 
- **csrf**: _Enables or disables Cross-Site Request Forgery protection. It's enabled by default for type='http' and methods=['POST']._
- **cors (str)** – _The Access-Control-Allow-Origin cors directive value._
- **handle_params_access_error** _(Callable[[Exception], Response]) – Implement a custom behavior if an error occurred when retrieving the record from the URL parameters (access error or missing error)._

#### Extended Example
Let's look at a more comprehensive example demonstrating these parameters:
```
from odoo import http
from odoo.http import request

class AdvancedController(http.Controller):
    @http.route(['/route1', '/route2'], type='http', auth='user', methods=['GET'])
    def handle_multiple_routes(self):
        return "<h1>Response for multiple routes</h1>"

    @http.route('/json_route', type='json', auth='public')
    def handle_json(self, **kw):
        data = {'message': 'JSON response'}
        return data

    @http.route('/post_only', methods=['POST'], auth='none', csrf=False)
    def handle_post(self):
        return "POST request accepted"
```
**In this example:**
- handle_multiple_routes handles GET requests for two URLs and requires user authentication.
- handle_json returns JSON data and is accessible to everyone.
- handle_post only accepts POST requests, has no authentication, and CSRF protection is disabled.

<details>
 <summary>Handling methods: </summary>

  HTTP methods like GET, POST, PUT, DELETE, etc., indicate the desired action to be performed on the resource identified by the URL. Odoo allows you to specify which methods your route should handle, and you can write your controller logic accordingly.

**Specifying HTTP Methods in Odoo**
When defining a route in Odoo, you can use the methods parameter in the @http.route decorator to specify which HTTP methods your route should respond to. If methods is not specified, the route will respond to all HTTP methods by default.

**Example of Handling Different Request Methods**
Here's an example of how to handle different HTTP methods in an Odoo controller:

```
from odoo import http
from odoo.http import request

class MyController(http.Controller):
    @http.route('/api/resource', type='json', auth='public', methods=['GET', 'POST'])
    def api_resource(self, **kw):
        if request.httprequest.method == 'GET':
            # Logic for handling GET requests
            return {'response': 'GET request processed'}
        
        elif request.httprequest.method == 'POST':
            # Logic for handling POST requests
            # Accessing JSON data from the request body
            data = request.jsonrequest
            return {'response': 'POST request processed', 'data': data}
```
In this example:

> The route /api/resource can handle both GET and POST requests.  
The method api_resource checks request.httprequest.method to determine the type of the HTTP request and processes it accordingly.  
For POST requests, the example assumes that the request body contains JSON data, which is accessed via request.jsonrequest.

</details>

### Usage of Routing
Routing is crucial for:

- Defining API endpoints for external integrations.
- Creating custom web pages or controllers in your Odoo application.
- Handling form submissions or any server interaction from the client side.
- By using routing effectively, you can extend Odoo's capabilities to meet various business needs, whether it's adding new pages to your website or creating a complete API for third-party integration.
</details>

<details>
 <summary>Response Handling: </summary>
> Response handling in Odoo involves sending back data or web content to the client after processing a request. This process is vital in web development as it determines how your application communicates with users or external systems.  

### Types of Responses in Odoo
**Odoo supports several types of responses, primarily:**

- HTTP Responses: Returning HTML content, redirects, or other HTTP-specific responses.  
- JSON Responses: For API endpoints, particularly useful in AJAX operations and JSON-RPC.  

**HTTP Responses**  
The most common type of response in web applications is an HTTP response. In Odoo, you can return plain text, HTML, or even perform redirections.  

Example: Returning HTML Content
``` from odoo import http

class MyController(http.Controller):
    @http.route('/hello', auth='public')
    def hello_world(self):
        return "<h1>Hello, World!</h1>"
``` 
Here, navigating to /hello will display "Hello, World!" in an H1 HTML tag.

Example: HTTP Redirect
```
from odoo.http import request

class MyController(http.Controller):
    @http.route('/redirect', auth='public')
    def redirect_example(self):
        return request.redirect('/hello')
```
This method redirects the user from /redirect to /hello.

**JSON Responses**
For API endpoints, especially those used by AJAX or external systems, JSON is a commonly used format.

**Example: JSON Response**
```
class MyAPIController(http.Controller):
    @http.route('/api/data', auth='public', type='json')
    def get_data(self):
        data = {'key': 'value', 'number': 123}
        return data
```
Here, a request to /api/data will receive a JSON response containing the specified data.

### Advanced Response Handling
> Odoo also allows for more complex response handling, such as returning response objects for greater control over headers, cookies, and status codes.

**Example: Response Object**
``` from werkzeug.wrappers import Response
from odoo import http

class MyController(http.Controller):
    @http.route('/custom_response', auth='public')
    def custom_response(self):
        response = Response("Custom Response", status=200, mimetype='text/plain')
        response.headers['Custom-Header'] = 'CustomValue'
        return response
In this example, a custom response is created with a specific status code, MIME type, and custom header.
```
### Usage in Odoo
- Web Pages: For rendering web pages or parts of a website.
- Data Exchange: For APIs, where you need to exchange data in a structured format like JSON.
- Handling Forms: When processing form submissions, you can return success messages, errors, or redirect the user.
- Ajax Requests: For dynamically updating parts of a webpage without reloading.
> Proper response handling is essential for creating interactive and user-friendly web applications in Odoo. It allows your Odoo modules to communicate effectively with users and other systems, enhancing the overall functionality of your application.

</details>

<details>
<summary> Types of URL Parameters in Odoo </summary>

  _In Odoo, URL parameter or argument handling is an essential part of creating dynamic routes in your web controllers. These parameters allow your application to respond differently based on the values passed through the URL. Odoo supports different types of URL parameters, each with its own use case._
  
- **Static Parameters**: Fixed parts of the URL.

      Example: /page/contact_us - Here, page and contact_us are static parameters.

- **Dynamic Route Parameters:** Variable parts of the URL that are captured and passed to the method.

  1. **Integer (<int:parameter>):** Matches and captures an integer value.

      Example: /user/<int:user_id> - Matches /user/123 and user_id will be 123.
  2. **String (<string:parameter>):** Matches and captures a string. It does not include slashes.

      Example: /category/<string:category_name> - Matches /category/electronics.

  3. **Path (<path:parameter>):** Similar to string, but it can include slashes.

      Example: /path/<path:subpath> - Matches /path/some/long/subpath.
  4. **Other Types:** Other types like float, uuid, etc., can also be used but are less common.

- **Query String Parameters:** These are not defined in the route but are appended to the URL after a ?.

    Example: /search?query=laptop - Here, query is a query string parameter.

### Static Parameters
> Handling static parameters in Odoo involves defining parts of the URL that remain constant. These static segments in the URL path are used to define specific routes that your controller methods will respond to. Unlike dynamic parameters, which can change and be captured as variables, static parameters are fixed and form the basic structure of your URL.

#### Defining Static Parameters in Routes
When you define a route in Odoo, you typically include static parameters to specify a particular path. For instance:
```
from odoo import http

class MyController(http.Controller):
    @http.route('/about', auth='public')
    def about_page(self):
        return "This is the About page."

    @http.route('/contact', auth='public')
    def contact_page(self):
        return "This is the Contact page."
```
In this example:

The route /about is a static parameter. When users navigate to yourdomain.com/about, the about_page method will handle the request.
Similarly, /contact is another static parameter, routing requests to yourdomain.com/contact to the contact_page method.
#### Usage of Static Parameters
Static parameters are used for:  
- Defining Clear and Structured URLs: They help in creating a well-structured and readable URL pattern. For instance, /products, /services, /blog etc., are common static parameters in URLs that clearly indicate the page content.   
- Creating SEO-Friendly URLs: Static URLs are easier to index and rank by search engines, making them better for SEO.  
- Navigation and Menu Items: They are often used to define the main navigation structure of a website.  

#### Considerations
**Consistency**: Static parameters should be consistently defined across your application for clarity and maintainability.  
**Combination with Dynamic Parameters:** Often, static parameters are used in combination with dynamic parameters to create more complex and useful routing patterns. For example, /blog/<string:post_slug> combines a static /blog segment with a dynamic segment for the blog post slug.
#### Summary
Static parameters in Odoo routes are the fixed parts of a URL used to define specific endpoints for controller methods. They are crucial for creating an organized and navigable structure in web applications, ensuring that URLs are user-friendly and meaningful.

### Handling Dynamic Route Parameters
Dynamic parameters are part of the route URL and are passed as arguments to your controller method.

> When a user navigates to a URL that matches one of these patterns, the corresponding method is called, and the dynamic part of the URL is passed to the method as an argument. Inside the method, you can use this parameter to perform actions, such as retrieving a record from the database.  


```
from odoo import http

class MyController(http.Controller):
    @http.route('/user/<int:user_id>', auth='public')
    def user_profile(self, user_id):
        return f"Profile of user {user_id}"
``` 
In this example, user_id is a dynamic route parameter of type integer.

### Definition and Usage

> Dynamic parameters in a route are defined by including variable parts within angle brackets < > in the URL. These parameters are then passed as arguments to your controller method.

### Types of Dynamic Route Parameters
- Integer (<int:parameter>): This captures an integer value from the URL and passes it as an integer.  
Example: /order/<int:order_id>  

- String (<string:parameter>): This captures a string. It does not include slashes.  
Example: /category/<string:category_name>  

- Path (<path:parameter>): This is similar to string but can include slashes, useful for URLs with multiple path segments.  
Example: /library/<path:book_path>  

#### Implementing Dynamic Parameters
Here’s how you can implement and use dynamic parameters in Odoo:
```
from odoo import http

class MyController(http.Controller):
    @http.route('/user/<int:user_id>', auth='public')
    def user_profile(self, user_id):
        # Logic to handle the user profile based on user_id
        return f"Profile of user with ID {user_id}"

    @http.route('/category/<string:category_name>', auth='public')
    def category(self, category_name):
        # Logic for handling specific category
        return f"Welcome to the {category_name} category"

    @http.route('/library/<path:book_path>', auth='public')
    def book(self, book_path):
        # Logic for handling book path
        return f"Book path: {book_path}"
```
In this example:

/user/<int:user_id> captures an integer user_id.
/category/<string:category_name> captures a string category_name.
/library/<path:book_path> captures a path book_path which can include slashes.



### Handling Query String Parameters

 Query string parameters are appended to the URL after a question mark ? and can include multiple parameters separated by &. These parameters are not defined in the route itself but are passed through the URL and can be accessed in the controller method.

#### Accessing Query String Parameters
In Odoo, you can access query string parameters using the **kw argument in your controller method. This argument captures all the named parameters sent in the request.

Query string parameters are not part of the route URL. They are accessible through the **kw argument in your controller method.
```
class MyController(http.Controller):
    @http.route('/search', auth='public')
    def search(self, **kw):
        query = kw.get('query', '')
        return f"Search results for {query}"
```
In this example, if the URL is /search?query=laptop, query will be "laptop".

Example
Let's say you have a search page where users can pass a query through the URL, like /search?query=odoo.

Here's how you'd handle this in an Odoo controller:
```
from odoo import http
from odoo.http import request

class MyController(http.Controller):
    @http.route('/search', auth='public', type='http')
    def search(self, **kw):
        search_query = kw.get('query', '')
        # Perform search using search_query
        return f"Results for: {search_query}"
```

In this example:

The method search captures the query string parameters using **kw.
kw.get('query', '') retrieves the value of query. If query is not provided in the URL, it defaults to an empty string.

### Handling Multiple Query Parameters
You can handle multiple query parameters in a similar way. For instance, if you have a URL like /filter?category=books&sort=price.

Your controller method would look like this:
```
@http.route('/filter', auth='public', type='http')
def filter(self, **kw):
    category = kw.get('category', 'all')
    sort_order = kw.get('sort', 'none')
    # Apply filtering based on category and sort_order
    return f"Filtering {category} sorted by {sort_order}"
```
#### Best Practices
**Validation**: Always validate the query parameters to ensure they contain safe and expected data.  
**Default Values:** Use .get() with a default value to handle cases where a parameter might not be provided.  
**Error Handling:** Include proper error handling for invalid or unexpected parameter values.  

> Query string parameters are extensively used for filtering, searching, pagination, and other dynamic data retrieval mechanisms in web applications. They are easy to implement and offer a straightforward way for users to interact with your application dynamically.



### Best Practices
**Validation**: Always validate the parameters, especially dynamic and query string parameters, to ensure they contain expected and safe data.  
**Use Appropriate Types:** Use the correct type for dynamic parameters to ensure they match the expected format.  
**Default Values:** For query string parameters, use .get() with a default value to handle cases where the parameter might not be provided.  
Proper handling of URL parameters allows for the creation of responsive and dynamic web applications in Odoo, enhancing user experience by providing tailored content based on user input and navigation

</details>
