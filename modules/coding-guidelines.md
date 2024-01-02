# Coding guidelines
https://www.odoo.com/documentation/17.0/developer/tutorials/getting_started/16_final_word.html
https://www.odoo.com/documentation/17.0/contributing/development/coding_guidelines.html

> Odoo's coding guidelines and naming conventions are essential for maintaining consistency, readability, and quality in module development. Here's an overview of these guidelines:

## Python Coding Guidelines:
- PEP 8 Compliance:
Follow PEP 8 style guide for Python code.
Example: Use snake_case for variable and method names.
- Docstrings:
Use docstrings to describe classes and methods.
**Example:**
```
def compute_amount(self):
    """Compute the amount and return the value."""
```
- Recordset Methods:
Prefer recordset methods over traditional looping.  
Example: Use filtered() and mapped() for recordset operations.
- No SQL Injection:
  Avoid SQL injections by using ORM methods or parameterized queries.
- XML Guidelines:
IDs for Record Tags:  
Use meaningful IDs for XML records.  
**Example:**
  ```
   <record id="model_name_view_type" model="ir.ui.view">
  ```
- XPath for Inheritance:
Prefer using xpath expressions for inheriting views.
**Example:**
```
<xpath expr="//field[@name='partner_id']" position="after">
    <field name="custom_field"/>
</xpath>
```

## Naming Conventions:
- Model Names:
Use dot notation, all lowercase. 
Example: my_module.my_model  
- Field Names:
Use snake_case.  
Example: total_amount, product_id  
- Method Names:
Use snake_case.  
Example: compute_total, action_confirm  
- XML IDs:
 Use a combination of model name and purpose.  
 Example: model_name_form_view, model_name_tree_view  
- Module Structure:
Organize module files consistently with Odoo's structure (controllers, models, views, data, etc.).  
- Class Inheritance:
For Python class inheritance, use CamelCase and prepend with _ for private models.  
Example: _MyCustomClass  
- File Names:
 Name files after their purpose (models, views, data).  
 Example: views/my_module_views.xml, models/my_module_models.py  
### Other Best Practices:
- Security: Always consider security in your code, especially in public-facing controllers.
- Performance: Write efficient code, mindful of database queries and memory usage.
- Documentation: Document the module's functionality, installation, and configuration steps in a README file.
- Testing: Include unit tests to ensure module functionality and to catch regressions.
> Following these guidelines helps maintain high standards in Odoo development, ensuring code clarity, efficiency, and maintainability.







