# Onchange methods
Onchange methods in Odoo are special types of methods triggered by changes in the UI, specifically when the value of a field in a form is modified. They are used to dynamically update other parts of the form based on those changes, enhancing the interactivity and user experience of Odoo forms.

## Key Characteristics of Onchange Methods
- UI Interaction: They are executed in real-time when the user changes a field value in the form view.

- Dynamic Updates: Useful for updating other fields or displaying warnings based on the changed data.

- No Database Writes: Changes made in onchange methods are not immediately written to the database. They only affect the current view's state.

## Creating Onchange Methods
- Decorator: Use the @api.onchange decorator, followed by the names of the fields that trigger the method.
- Field Updates: Within the method, you can modify the values of other fields, which will be reflected in the UI.
### Example of Onchange Methods
Let’s consider a scenario in a sale.order model where changing a product in an order line should automatically update the unit price.

Python Code in an Odoo Model:

```
from odoo import api, models, fields

class SaleOrderLine(models.Model):
    _name = 'sale.order.line'

    product_id = fields.Many2one('product.product', string='Product')
    unit_price = fields.Float(string='Unit Price')

    @api.onchange('product_id')
    def _onchange_product_id(self):
        if self.product_id:
            self.unit_price = self.product_id.list_price
        else:
            self.unit_price = 0.0
```
### In this example:

- Triggering Field: The @api.onchange('product_id') decorator indicates that this method should be triggered when product_id is modified.
- Method Logic: In the _onchange_product_id method, the unit price is updated based on the selected product's list price. If no product is selected, the price is set to zero.
### Usage and Best Practices
- Validation and Warnings: Onchange methods can be used to provide instant feedback or warnings based on user input.
- No Side Effects: Avoid using onchange methods for operations with side effects outside the current view, like creating or modifying other records.
- Performance: Keep the logic in onchange methods efficient, as they are triggered often and can impact the responsiveness of the UI.
- Fallback Values: Ensure that your onchange methods handle cases where the field might be empty or invalid.
> Onchange methods are integral to creating a responsive and intuitive user experience in Odoo applications. They allow for real-time interaction within forms, providing immediate feedback and dynamic content updates based on user input.
