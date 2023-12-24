# View Records
```
Views are what define how records should be displayed to end-users.
They are specified in XML which means that they can be edited independently from the models that they represent. 
```

## View Types
- **Form** -> display and edit the data from a single record
- **List** -> view and edit multiple records
- **Search** -> apply filters and perform searches (the result are displayed in the current view list, kanban…)
- **Kanban** -> displays records as “cards” configurable as a small template
- **Qweb** -> templating of reporting, website…
- **Graph** -> visualize aggregations over a number of records or record groups
- **Pivot** -> display aggregations as a pivot table
- **Calendar** -> display records as events in a daily, weekly, monthly or yearly calendar

### Enterprise feature
- **Cohort** -> display and understand the way some data changes over a period of time
- **Gantt** -> display records as a Gantt charts (for scheduling)
- **Grid** -> display computed information in numerical cells are hardly configurable
- **Map** -> display records on a map and the routes between them

<details>
<summary>  Characteristics of Form Views</summary>
   
- **Single Record Focus:** Each Form View is associated with one record from a model (database table). When you open a Form View, you're either creating a new record or editing an existing one.  

- **Layout and Fields:** The layout of a Form View is defined in XML and determines which fields of the record are displayed and how they are arranged. This can include text fields, numeric fields, drop-down lists, checkboxes, date fields, and more.  

- **Customizable Interface:** You can customize Form Views to include specific fields, group them under sections or tabs, and even include instructions or tooltips for users.  

- **Data Validation:** Form Views often have built-in validation to ensure that the data entered meets certain criteria before it can be saved. For example, a field might be marked as mandatory, or a numeric field might have a specified range.  

- **Support for Various Widgets:** Form Views support different widgets to enhance user interaction, like calendars for date fields, buttons for triggering actions, and relational fields (like Many2one) to link to other records.  

- **Dynamic Behavior:** You can program Form Views to show or hide fields, change options dynamically, or even update other parts of the UI based on user input or other conditions.  
</details>
<details>
<summary> Functionality in Form Views </summary>

- **CRUD Operations:** Form Views are central to Create, Read, Update, and Delete (CRUD) operations in Odoo. They provide the interface for users to input and modify data.

- **Attachments and Notes:** Users can often attach files or add notes directly within a Form View.

- **Chatter Feature:** Many Form Views include Odoo's "Chatter" feature at the bottom, allowing users to log notes, send messages, and track the history of changes and communications related to the record.
</details>
<details>
   <summary> Technical Aspects </summary>

- **XML Definition:** The structure and fields of a Form View are defined in an XML file. This definition includes the field types, labels, default values, and layout.

- **Inheritance and Extension:** Form Views can be inherited and extended in custom modules, allowing developers to add or modify fields and behaviors without altering the base functionality.

- **Model Binding:** Each Form View is bound to a specific model (database table), and the fields displayed correspond to the columns in the model.
</details>

*Form Views are integral to the user experience in Odoo, providing a user-friendly and flexible way to manage detailed records in the database. They are a key point of interaction for users performing data entry, editing, and analysis within the system.* 

```
<record id="view_employee_form" model="ir.ui.view">
    <field name="name">employee.form</field>
    <field name="model">hr.employee</field>
    <field name="arch" type="xml">
        <form string="Employee">
            <sheet>
                <group>
                    <field name="name"/>
                    <field name="job_id"/>
                    <field name="department_id"/>
                </group>
                <group>
                    <field name="work_email"/>
                    <field name="work_phone"/>
                </group>
                <notebook>
                    <page string="Personal Information">
                        <group>
                            <field name="birthday"/>
                            <field name="gender"/>
                            <field name="marital"/>
                        </group>
                    </page>
                    <page string="Work Information">
                        <group>
                            <field name="coach_id"/>
                            <field name="manager_id"/>
                        </group>
                    </page>
                </notebook>
            </sheet>
        </form>
    </field>
</record>

```
### Form ```<form>```
> **Properties**: A form view defines how a single record is displayed. It's the main container for all other elements. The form view is defined in XML and can include other components like groups, notebooks, and fields.
**Usage**: Used for detailed viewing and editing of individual records. It's where users will spend most of their time creating, updating, or reading data.

### Group ```<group>```
> **Purpose**: Used to cluster related fields together. This helps in creating a logical and organized structure on the form.
**Functionality**: Fields within a group are typically displayed together, making it easier for users to understand the context and relationship between them. Groups can be nested for further organization.
**Properties**: Groups are used within form views to organize fields logically. They can be nested (groups within groups) and can have attributes like string (label of the group), colspan, col, etc.
**Usage**: Enhance readability and structure of forms by clustering related fields, improving user experience and data entry efficiency.
### Notebook ```<notebook>```
> **Purpose**: Functions like a tabbed interface, allowing you to organize content into different tabs (pages). This is particularly useful for forms with a lot of fields or complex information.
**Functionality**: Each tab (page) within a notebook can contain groups and individual fields. This arrangement helps in separating different aspects of a record, making the form less cluttered and more user-friendly.
> **Properties**: A notebook is a container for pages, allowing you to create a tabbed interface within a form view.
**Usage**: Organizes information into separate tabs (pages), making forms with many fields more manageable and less overwhelming.
### Page ```<page>```
> **Purpose**: Represents an individual tab within a notebook.
**Functionality**: Pages are used to categorize and separate information into distinct sections. For instance, in an employee form, you might have separate tabs for personal details, job-related information, and payroll details.
> **Properties**: Pages are used inside a notebook to create individual tabs. Each page can contain groups and fields.
Usage: Separates differe  
### Field ```<field>```
> **Properties**: Fields correspond to the columns in the database model. Properties include the field type (e.g., char, text, many2one), string (label), required, readonly, etc.
> **Usage**: The primary means of displaying and capturing data. They are the building blocks of forms and are used to show information or allow users to enter/edit data.  
### Record ```<record>```
> **Properties**: A record is an instance of a model, representing a row in a database table. It contains the data for that instance.
**Usage**: Records are the central elements of data manipulation in Odoo. All forms, views, and operations are essentially about creating, reading, updating, or deleting records.


