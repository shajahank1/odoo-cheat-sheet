# Action methods 
Action methods in Odoo are custom methods defined in models that perform specific actions related to the business logic. Unlike CRUD (Create, Read, Update, Delete) methods which are standard across models for basic database operations, action methods are tailored to specific requirements of the business or application.

## Characteristics of Action Methods
- Custom Business Logic: They encapsulate specific business operations, such as processing an order, calculating totals, changing states of records, or any other custom logic.

- Triggered by User Actions: Often triggered by user actions from the UI, like clicking a button or changing a field value.

- Return Values: Can return specific values or actions, such as opening a form view, returning a warning, or even triggering a workflow.

- API Decorators: Typically use @api.multi or @api.one decorators, depending on whether they operate on single records or multiple records simultaneously.

### Example of an Action Method
Let's consider an example of an action method in a hypothetical sale.order model, which confirms an order.

Python Code in an Odoo Model:

```
from odoo import api, models, exceptions

class SaleOrder(models.Model):
    _name = 'sale.order'

    state = fields.Selection([
        ('draft', 'Draft'),
        ('confirmed', 'Confirmed'),
        ('done', 'Done'),
        ('cancelled', 'Cancelled')
    ], default='draft')

    @api.multi
    def action_confirm(self):
        for order in self:
            if order.state != 'draft':
                raise exceptions.UserError("Only draft orders can be confirmed.")
            order.write({'state': 'confirmed'})
            # Additional logic for order confirmation
```
### In this example:

- Method Definition: action_confirm is an action method that changes the state of a sale order to 'confirmed'.
- Check Conditions: It first checks if the order is in the 'draft' state. If not, it raises an error.
- Update State: If the condition is met, it updates the state to 'confirmed'.
- Business Logic: This method can contain other business logic needed when confirming an order.
## Usage and Best Practices
- User Interface: Action methods are often linked to buttons in the Odoo UI.
- Consistent State: Ensure the method leaves the system in a consistent state, handling all necessary changes and checks.
- Error Handling: Use exceptions to handle error conditions and provide feedback to the user.
- Transactions: Remember that operations in Odoo are transactional. If an error occurs, changes made during the method execution are rolled back.
- Record Rules and Access Rights: Be mindful of record rules and access rights to ensure the method respects the security constraints of the system.
> Action methods are a powerful way to implement the unique business logic of your Odoo application, providing flexibility and control over how your models behave and interact with users.




