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
