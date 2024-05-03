> Displaying master-detail relationships in Odoo involves setting up views where you can see a master record and its associated detail records (often referred to as child records) at the same time. This is commonly done using one of the various relational field types that Odoo supports, such as One2many, Many2one, and Many2many. The One2many fields are particularly useful for displaying master-detail relationships.

### Example Scenario
Let's say you have a model master.model that represents master records, and another model detail.model that represents detail records. A master record can have multiple detail records. We will set up the models and the views to show this relationship.

#### Step 1: Define the Models
First, you need to define the models in Python. Here’s how you might set this up:

```
from odoo import models, fields

class MasterModel(models.Model):
    _name = 'master.model'
    _description = 'Master Model'

    name = fields.Char(string="Name")
    detail_ids = fields.One2many('detail.model', 'master_id', string="Details")

class DetailModel(models.Model):
    _name = 'detail.model'
    _description = 'Detail Model'

    name = fields.Char(string="Name")
    master_id = fields.Many2one('master.model', string="Master", ondelete="cascade")
```
In this setup:

- MasterModel has a field detail_ids which is a One2many field. This field shows all DetailModel records related to a MasterModel.
- DetailModel has a Many2one field master_id pointing back to MasterModel, establishing the relationship.
#### Step 2: Define the Views
Next, you set up the views in XML to display these relationships. Here’s an example of how to define the master and detail views.

Master Form View
This view shows the master record and embeds the detail records directly within it.

```
<odoo>
    <record id="view_form_master_model" model="ir.ui.view">
        <field name="name">master.model.form</field>
        <field name="model">master.model</field>
        <field name="arch" type="xml">
            <form string="Master Model">
                <sheet>
                    <group>
                        <field name="name"/>
                    </group>
                    <notebook>
                        <page string="Details">
                            <field name="detail_ids">
                                <tree editable="bottom">
                                    <field name="name"/>
                                </tree>
                            </field>
                        </page>
                    </notebook>
                </sheet>
            </form>
        </field>
    </record>
</odoo>
```
##### In this form view:

The detail_ids field uses a tree sub-view which is embedded within the form. This allows editing of the detail records directly within the master form, making it easy to manage the master-detail relationship.
The details are placed inside a notebook page, which is a common UI pattern in Odoo for organizing related datasets.
#### Step 3: Add Menu and Action
You'll also need to set up a menu item and an action to access this master model form.

```
<record id="master_model_action" model="ir.actions.act_window">
    <field name="name">Master Model</field>
    <field name="type">ir.actions.act_window</field>
    <field name="res_model">master.model</field>
    <field name="view_mode">form,tree</field>
</record>

<menuitem id="master_model_menu" name="Master Model"
          action="master_model_action" parent="base.menu_custom"/>
```
> These steps set up a basic master-detail relationship in Odoo where you can create, view, and manage master records along with their related detail records directly from the master's form view. This is essential for applications like order management systems, where you have orders (master) and order lines (details), among many other potential applications.
