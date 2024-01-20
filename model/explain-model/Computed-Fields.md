# Computed Fields
Computed fields in Odoo are fields whose values are calculated on the fly using specified Python methods. They are particularly useful for displaying data that is derived from other fields without needing to store the data in the database, although you have the option to store them if required.

## Key Characteristics of Computed Fields
- Dynamic Calculation: The value of a computed field is calculated dynamically by a function, often based on other field values.

- Not Stored by Default: By default, computed fields are not stored in the database. Their values are calculated each time they are accessed.

- Option to Store: You can choose to store computed fields in the database. This is useful if you need to make them searchable or if the computation is resource-intensive.

- Dependencies: Computed fields depend on other fields for their calculation. These dependencies need to be specified so Odoo knows when to recalculate the computed field.

## Creating Computed Fields
- Define the Field: Declare the field in your model and set compute to the name of the computing method.
- Create a Compute Method: Define the method responsible for calculating the field's value. Use the @api.depends decorator to specify field dependencies.
### Example of Computed Fields
Python Code in an Odoo Model:

```
from odoo import models, fields, api

class SaleOrder(models.Model):
    _name = 'sale.order'

    order_line_ids = fields.One2many('sale.order.line', 'order_id', string='Order Lines')
    total_amount = fields.Float(compute='_compute_total_amount', string='Total Amount', store=True)

    @api.depends('order_line_ids.price_total')
    def _compute_total_amount(self):
        for record in self:
            record.total_amount = sum(line.price_total for line in record.order_line_ids)
```
### In this example:

- total_amount is a computed field in the sale.order model.
- It depends on price_total of each line in order_line_ids.
- The _compute_total_amount method calculates the sum of price_total from all order lines and assigns it to total_amount.
- The computed field is stored in the database, as indicated by store=True.
## Use Cases for Computed Fields
- Calculating totals, averages, or other aggregated values from related records.
- Concatenating strings from different fields to create a new meaningful representation.
- Converting data from one format to another for display purposes.
- Determining states or conditions based on complex logic involving multiple fields.
> Computed fields are a powerful feature in Odoo for dynamic data representation. They enhance the flexibility of data handling in models and are essential for creating efficient, data-driven business applications.
