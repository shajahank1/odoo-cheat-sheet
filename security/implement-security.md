# Security in Odoo
- https://www.odoo.com/documentation/17.0/developer/tutorials/getting_started/05_securityintro.html
- https://www.odoo.com/documentation/17.0/developer/reference/backend/security.html#reference-security
- https://www.odoo.com/documentation/17.0/developer/tutorials/restrict_data_access.html

### Overview
> Odoo's robust security framework is designed to ensure data protection and controlled access at various levels within the application. This framework encompasses several mechanisms, including Model Level Access Rights, Record Rules, Field Level Access Rights, Views and Menus, Controller Level Security, and User Level Access Rights. Understanding and correctly implementing these mechanisms is crucial for safeguarding data and maintaining the integrity of the Odoo environment.


## Model Level Access Rights
Purpose: Controls CRUD operations on models per user group.
### Implementation Guide:

- Define access rights in XML or CSV.
- Each record should link a user group to a model and specify allowed operations.
**Example:**
```
<record id="access_product_manager" model="ir.model.access">
    <field name="name">product_manager_access</field>
    <field name="model_id" ref="product.model_product_template"/>
    <field name="group_id" ref="product.group_product_manager"/>
    <field name="perm_read" eval="True"/>
    <field name="perm_write" eval="True"/>
    <field name="perm_create" eval="True"/>
    <field name="perm_unlink" eval="True"/>
</record>
```
Here, users in the 'Product Manager' group have full access to the product.template model.

## Using CSV for Model Access Rights
> CSV files for access rights typically define which user groups can perform CRUD (Create, Read, Update, Delete) operations on specific models.

### Structure of CSV for Access Rights:
- Model ID: The model to which the access rights apply.
- Group ID: The user group these rights apply to.
- Access Rights: Boolean values for read, write, create, and delete permissions.
**Example:**
Here is an example of a CSV file to define access rights:

```
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_rule_sample,Sample Model Access,sample_module.model_sample_model,sample_module.group_sample_user,True,True,False,False
```
> In this example, users in the group_sample_user group can read and write sample_model records but can't create or delete them.

## Implementing Access Rights Using CSV:
- Create the CSV File:
    Create a CSV file in the security/ directory of your module.  
    Name it appropriately, like ir_model_access.csv.  
- Define Access Rules:
    Fill in the CSV with the necessary access rules.  
    Add the CSV File to __manifest__.py:  
- Include the CSV file in the data section of your module's __manifest__.py file:
```
'data': [
    'security/ir_model_access.csv',
    # other files
],
```
- Update the Module:
    Update your module in Odoo for the changes to take effect.   
    Using CSV for Data Import/Export  

## 2. Record Rules
Purpose: Row-level access control for records.

### Implementation Guide:
- Defined in XML.
- Use Odoo domain syntax to specify conditions.
**Example:**
```

<record id="product_template_multi_company_rule" model="ir.rule">
    <field name="name">Product Template Multi-Company</field>
    <field name="model_id" ref="product.model_product_template"/>
    <field name="domain_force">['|',('company_id','=',False),('company_id','child_of',[user.company_id.id])]</field>
</record>
```
This rule ensures users see product.template records from their company.

## 3. Field Level Access Rights (ir.model.fields)
Purpose: Controls the visibility of specific fields.

### Implementation Guide:
- Defined in Python code.
- Use the groups attribute to specify which user groups can access the field.
**Example:**

```
class ResPartner(models.Model):
    _inherit = 'res.partner'

    secret_info = fields.Char(string='Secret Info', groups='base.group_system')
```
The secret_info field is only visible to users in the 'System' group.

## 4. Views and Menus
Purpose: Control access to specific UI views and menus.
## Implementation Guide:
- Use XML to define which views and menus are accessible to which groups.
**Example:**

```
<menuitem id="menu_sample_manager"
          name="Sample Manager Menu"
          parent="base.menu_management"
          groups="module_name.group_sample_manager"/>
```
This menu item is only visible to users in the 'Sample Manager' group.

## 5. Controller Level Security
Purpose: Secure HTTP endpoints in Odoo.

## Implementation Guide:
- Use @http.route decorator in Python.
- Specify auth parameter to control access.
**Example:**

```
from odoo import http

class MyController(http.Controller):
    @http.route('/my_endpoint', auth='user')
    def my_endpoint(self):
        # Your code here
```
This route is accessible only to authenticated users.

## 6. User Level Access Rights
Purpose: Specify rights at the individual user level.

## Implementation Guide:
- In Odoo's UI, go to Settings -> Users & Companies -> Users.
- Select a user and configure specific rights under the 'Access Rights' tab.
**Example:**

> For a specific user, you might grant additional rights like 'Technical Features' or access to specific applications.
### Ensuring Secure Access
- Regularly update Odoo and all dependencies.
- Use strong, unique passwords and consider implementing two-factor authentication.
- Educate users about security best practices.
- Regularly review and audit security settings.
- Use HTTPS and secure network configurations.

### Best Practices for Implementing Security
- Principle of Least Privilege: Assign only the necessary rights to each user or user group.
- Regular Audits and Reviews: Periodically review security settings, especially after updates or changes in business processes.
- User Education: Train users on security best practices and the importance of data protection.
- Monitoring and Logging: Utilize Odoo's logging capabilities to monitor activities and track changes, especially for sensitive data.
- Secure Configuration: Ensure secure configurations in both Odoo and the underlying infrastructure, including database and network settings.
### Ensuring Secure Access
- Use HTTPS: Always use SSL/TLS to encrypt data in transit.
- Strong Authentication: Enforce strong passwords and consider two-factor authentication (2FA).
- Data Encryption: Encrypt sensitive data at rest in the database.
- Regular Updates: Keep Odoo and its dependencies up-to-date with the latest security patches.
> By adhering to these security practices and properly configuring the various security mechanisms, you can ensure a secure and reliable Odoo environment, safeguarding your data and operations against unauthorized access and potential security threats.
