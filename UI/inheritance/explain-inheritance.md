# Inheritance
- Inheritance allows for customizing delivered views. It makes it possible, for example, to add content as modules are installed, or to deliver different displays according to the action.

- Inherit views generally share the common structure defined below. Placeholders are denoted in all caps. This synthetic view will update a node targeted by an XPath, and another targeted by its name and attributes.
```
<record id="ADDON.MODEL_view_TYPE" model="ir.ui.view">
    <field name="model">MODEL</field>
    <field name="inherit_id" ref="VIEW_REFERENCE"/>
    <field name="mode">MODE</field>
    <field name="arch" type="xml">
        <xpath expr="XPATH" position="POSITION">
            <CONTENT/>
        </xpath>
        <NODE ATTRIBUTES="VALUES" position="POSITION">
            <CONTENT/>
        </NODE>
    </field>
</record>
```
The inherit_id and mode fields determine the view resolution. The xpath or NODE elements indicate the inheritance specs. The expr and position attributes specify the inheritance position.

### View resolution
Resolution generates the final arch for a requested/matched primary view as follow:

  1. if the view has a parent, the parent is fully resolved, then the current view’s inheritance specs are applied;

  2. if the view has no parent, its arch is used as-is;

  3. the current view’s children with mode extension are looked up, and their inheritance specs are applied depth-first (a child view is applied, then its children, then its siblings).

The inheritance is applied according to the inherit_id field. If several view records inherit the same view, the order is determined by the priority.

The result of applying children views yields the final arch.

### Inheritance specs
Inheritance specs are applied sequentially and are comprised of:

  1. an element locator to match the inherited element in the parent view;

  2. children element to modify the inherited element.

There are three types of element locators:

- An xpath element with an expr attribute. expr is an XPath expression1 applied to the current arch, matching the first node it finds;

- A field element with a name attribute, matching the first field with the same name.

 > Note
   - All other attributes are ignored.

Any other element, matching the first element with the same name and identical attributes.

 > Note
    - The attributes position and version are ignored.

**1** An extension function is added for simpler matching in QWeb views: hasclass(*classes) matches if the context node has all the specified classes.

```

<xpath expr="page[@name='pg']/group[@name='gp']/field" position="inside">
    <field name="description"/>
</xpath>

<div name="name" position="replace">
    <field name="name2"/>
</div>
```

### Inheritance position
The inheritance specs accept an optional position attribute, defaulting to inside, that specifies how the matched node should be modified.

#### inside
The content of the inheritance spec is appended to the matched node.

```
<notebook position="inside">
    <page string="New feature">
        ...
    </page>
</notebook>
```
#### after
The content of the inheritance spec is appended to the matched node’s parent after the matched node.

```
<xpath expr="//field[@name='x_field']" position="after">
    <field name="x_other_field"/>
</xpath>
```
#### before
The content of the inheritance spec is appended to the matched node’s parent before the matched node.

```
<field name=x_field" position="before">
    <field name="x_other_field"/>
</field>
```
#### replace
The content of the inheritance spec replaces the matched node. Any text node containing only $0 within the contents of the spec is replaced by a copy of the matched node, effectively wrapping the matched node.

```

<xpath expr="//field[@name='x_field']" position="replace">
    <div class="wrapper">
        $0
    </div>
</xpath>
```
#### attributes
The content of the inheritance spec should be made of only attribute elements, each with a name attribute and an optional body.

- If the attribute element has a body, a new attributed named after its name is added to the matched node with the attribute element’s text as value.

- If the attribute element has no body, the attribute named after its name is removed from the matched node.

- If the attribute element has an add attribute, a remove attribute, or both, the value of the matched node’s attribute named after name is recomputed to account for the value(s) of add, remove, and an optional separator attribute defaulting to ,. add includes its value(s), separated by separator. remove removes its value(s), separated by separator.

 Example
```
<field name="x_field" position="attributes">
    <attribute name="invisible">True</attribute>
    <attribute name="class" add="mt-1 mb-1" remove="mt-2 mb-2" separator=" "/>
</field>
```
#### move
The attribute position="move" is set on the content of the inheritance spec to specify how nodes are moved relatively to the inheritance spec’s element locator, on which the attribute position must also be set, with values inside, replace, after, or before.

 Example
```
<xpath expr="//@target" position="after">
    <xpath expr="//@node" position="move"/>
</xpath>

<field name="target_field" position="after">
    <field name="my_field" position="move"/>
</field>
```
