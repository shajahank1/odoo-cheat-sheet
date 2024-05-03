> In Odoo, if you need to display data from multiple models in a single view, you typically use related fields or computed fields to fetch and display this data. These techniques allow you to incorporate information from other models into your view seamlessly. Here are the main strategies:

### 1. Using Related Fields
Related fields in Odoo are used to fetch data from related records. They are defined in the Python model and can be displayed directly in the view. This approach is efficient when there is a direct relationship (like a Many2one link) between the model of the current view and the model from which you want to fetch data.

#### Example:
Suppose you have a model sale.order and you want to show the salesperson's phone number directly in the order form. You can add a related field to the sale.order model to fetch the phone number from the related res.users model.

```
from odoo import models, fields

class SaleOrder(models.Model):
    _inherit = 'sale.order'

    salesperson_phone = fields.Char(related='user_id.phone', string="Salesperson Phone", readonly=True)
```
In the XML view, you simply add the field like any other field:

```
<field name="salesperson_phone"/>
```
### 2. Using Computed Fields
Computed fields are used when the data requires some calculation or when the relationship between data is more complex than a simple field relation. These fields are defined in Python and can compute values from multiple models.

#### Example:
If you need to display information that aggregates data across different models, you could define a computed field. For example, if you want to display the total sales from a customer directly on the customer form in res.partner:

```
from odoo import models, fields

class ResPartner(models.Model):
    _inherit = 'res.partner'

    total_sales_amount = fields.Float(compute='_compute_total_sales_amount', string="Total Sales")

    def _compute_total_sales_amount(self):
        for partner in self:
            total_sales = self.env['sale.order'].search([('partner_id', '=', partner.id)]).mapped('amount_total')
            partner.total_sales_amount = sum(total_sales)
```
In your view, you would add:

```
<field name="total_sales_amount"/>
```
### 3. Using Context and Domain
For more complex scenarios where you might want to display records from another model in a view like a list or a Kanban, you could use a domain or context to filter records relevant to the displayed record.

#### Example:
Showing all orders from a customer directly on the customer form.

```
<field name="order_ids" context="{'default_partner_id': active_id}" domain="[('partner_id', '=', active_id)]"/>
```
This field order_ids would be a One2many field defined in res.partner, linking to sale.order, possibly using a related field or directly if the relationship is already defined.

### Best Practices
When designing views that incorporate data from multiple models:

- Ensure that your database queries are optimized to prevent performance issues.
- Always consider user permissions and access rights; make sure that displaying data from other models does not bypass security rules set in Odoo.
- Using these strategies, you can effectively display and manage data from multiple models in a single Odoo view.





