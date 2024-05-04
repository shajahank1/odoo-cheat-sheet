## mode property in view-record field
> In Odoo, when you define a view that inherits from another view (using the inherit_id attribute), you can specify the mode attribute to control how inheritance is applied. The mode attribute determines how the inheritance hierarchy is resolved and how views are combined when multiple views are involved.

### Here's an explanation of the different modes:

#### extension:
- This mode is used when you want to extend the functionality of an existing view without fully overriding it.
- When the view is requested, Odoo looks up the closest primary view (defined by inherit_id) and applies all views inheriting from it with the same model as the current view.
- In other words, the current view extends the functionality of the closest primary view by adding or modifying its elements.
#### primary:
- This mode is used when you want to fully resolve the closest primary view and then apply the inheritance specifications of the current view.
- When the view is requested, Odoo fully resolves the closest primary view, even if it uses a different model than the current one.
- After resolving the closest primary view, the inheritance specifications of the current view are applied, and the resulting view is used as if it were the actual view of the current model.
- This mode is useful when you want to completely override the behavior of the closest primary view and provide a customized view specific to the current model.
#### mode not specified:
- If you don't specify the mode attribute, Odoo defaults to extension mode.
- This means that by default, views inherit from the closest primary view and extend its functionality.
Now, let's consider a scenario where you might want to override the mode attribute while using inherit_id. One such case is delegation inheritance, where your derived model is separated from its parent model, and views matching one model won’t match with the other.

For example, suppose you inherit from a view associated with the parent model and want to customize the derived view to show data from the derived model. In this case, the mode of the derived view needs to be set to primary because it is the base (and maybe only) view for that derived model. Otherwise, the view matching rules won’t apply correctly, and you won't be able to customize the view as intended. By setting the mode to primary, you ensure that the view is fully resolved and then customized according to the specifications of the derived model.

> In summary, the mode attribute allows you to control how views are inherited and combined, and you can choose the appropriate mode based on your specific customization requirements and inheritance hierarchy.


### Let's consider an example to illustrate how the mode attribute works in practice:

- Suppose we have two models in our Odoo application: parent.model and child.model. The child.model inherits from the parent.model, and we want to customize the view associated with the child.model.

#### Here's how we can use the mode attribute:

Parent Model View (Primary View):The parent model parent.model has a primary view defined as follows:
```
<record id="view_parent_model_form" model="ir.ui.view">
    <field name="name">parent.model.form</field>
    <field name="model">parent.model</field>
    <field name="arch" type="xml">
        <!-- Primary view content goes here -->
    </field>
</record>
```
Child Model View (Inherited View):The child model child.model inherits from the primary view of the parent model. We define an inherited view for the child model:
```
<record id="view_child_model_form_inherited" model="ir.ui.view">
    <field name="name">child.model.form.inherited</field>
    <field name="model">child.model</field>
    <field name="inherit_id" ref="parent_model.view_parent_model_form"/>
    <field name="arch" type="xml">
        <!-- Inherited view content goes here -->
    </field>
</record>
```
- Setting the Mode:When defining the inherited view for the child model, we specify the mode attribute based on our customization requirements:
- If we want to extend the functionality of the parent model view without fully overriding it, we use the extension mode (which is the default behavior if mode is not specified explicitly).
```
<record id="view_child_model_form_inherited" model="ir.ui.view">
    <field name="name">child.model.form.inherited</field>
    <field name="model">child.model</field>
    <field name="inherit_id" ref="parent_model.view_parent_model_form"/>
    <field name="arch" type="xml">
        <!-- Inherited view content goes here -->
    </field>
</record>
```
- If we want to fully resolve the parent model view and then apply the customization specifications of the child model, we use the primary mode.
```
<record id="view_child_model_form_inherited" model="ir.ui.view">
    <field name="name">child.model.form.inherited</field>
    <field name="model">child.model</field>
    <field name="inherit_id" ref="parent_model.view_parent_model_form"/>
    <field name="arch" type="xml">
        <!-- Inherited view content goes here -->
    </field>
    <field name="mode">primary</field>
</record>
```
> In the example above, when using the primary mode, the view associated with the parent model is fully resolved first, and then the customization specified for the child model is applied on top of it. This allows for a complete customization of the view specific to the child model while ensuring that the inheritance hierarchy is respected.
