# Relations Between Models
In Odoo, relations between models are established using special field types that define how models are interconnected. These relations are fundamental for building complex applications where models need to reference each other. Here are the primary types of relational fields in Odoo:

## 1. Many2one (many2one)
Description: Creates a many-to-one relationship. It represents a situation where a record in one model can refer to one record in another model.
Usage: Commonly used for fields like customer, user, or product where each record references another single record from a different model.
Example: Linking an order to a customer.
```
class SaleOrder(models.Model):
    _name = 'sale.order'
    customer_id = fields.Many2one('res.partner', string='Customer')
```
In this case, customer_id in a sale.order record will refer to a record in the res.partner model.
## 2. One2many (one2many)
Description: Represents a one-to-many relationship. It is essentially the reverse of a Many2one field.
Usage: Used when one record in a model can be linked to multiple records in another model.
Example: Linking a customer to multiple orders.
```
class ResPartner(models.Model):
    _name = 'res.partner'
    order_ids = fields.One2many('sale.order', 'customer_id', string='Orders')
```
Here, order_ids provides a list of sale.order records linked to a res.partner record. Note that one2many fields do not directly store data in the database; they are virtual fields.
## 3. Many2many (many2many)
Description: Creates a many-to-many relationship where a record in one model can be related to many records in another model and vice versa.
Usage: Useful for scenarios like tagging, where each record can have multiple associated records from another model.
Example: Linking products to multiple tags and tags to multiple products.
```
class ProductTemplate(models.Model):
    _name = 'product.template'
    tag_ids = fields.Many2many('product.tag', string='Tags')

class ProductTag(models.Model):
    _name = 'product.tag'
    product_ids = fields.Many2many('product.template', string='Products')
```
In this case, each product can have multiple tags, and each tag can be associated with multiple products.
### Usage of Relational Fields
- Data Integrity: Odoo handles the integrity of these relationships, ensuring that the database remains consistent (e.g., when records are deleted).
- Flexibility: These relationships allow for the creation of complex data structures and business logic.
- UI Interaction: In Odoo's views (forms, lists), these relationships enable the embedding of related records and the easy navigation between them.
> Relational fields in Odoo are powerful tools for database design, allowing developers to accurately model real-world relationships and dependencies in business applications.
