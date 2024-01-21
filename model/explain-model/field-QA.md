# how to define a non database field in model
In Odoo, you can define non-database (also known as "computed" or "functional") fields in a model. These fields do not correspond to a column in the database. Instead, their values are calculated on-the-fly using a method. This approach is useful for fields whose value depends on other fields and can change over time.

## Steps to Define a Non-Database Field
- Declare the Field: Use a field type (like fields.Char, fields.Float, fields.Date, etc.) and set compute='method_name'.

- Define the Compute Method: Create a method with the name specified in the compute attribute. This method will compute the field's value.

- Use @api.depends Decorator (optional but recommended): Decorate the compute method with @api.depends followed by the fields it depends on. This ensures the computed field is updated when any of these fields change.

### Example
Let's say we have a model sale.order and we want to add a non-database field that calculates the total weight of the order based on the weight of individual order lines.

```
from odoo import models, fields, api

class SaleOrder(models.Model):
    _inherit = 'sale.order'

    total_weight = fields.Float(compute='_compute_total_weight', string='Total Weight')

    @api.depends('order_line.product_id', 'order_line.product_uom_qty')
    def _compute_total_weight(self):
        for record in self:
            total_weight = sum(line.product_id.weight * line.product_uom_qty for line in record.order_line)
            record.total_weight = total_weight
```
### In this example:
> total_weight is a computed field of type Float.  
> _compute_total_weight is the method that computes the value of total_weight.  
> The @api.depends decorator ensures that the total_weight field is recalculated whenever a product's weight or quantity in any order line is changed.  
### Important Notes
- Performance: While computed fields are powerful, they can impact performance, especially if they depend on fields from other models or involve complex computations.
- Storing Computed Fields: Optionally, computed fields can be stored in the database by setting store=True. This can improve performance but at the cost of database space and less real-time accuracy.
- Recordsets: Remember that methods for computed fields typically operate on recordsets (multiple records), so they should be written to handle more than one record at a time.
