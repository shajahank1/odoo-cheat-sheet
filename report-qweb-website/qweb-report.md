# QWeb Reports
QWeb Reports in Odoo are an essential feature used to generate printable PDF documents or HTML reports. These reports are built using QWeb, Odoo's templating engine, which allows for the creation of detailed and styled documents that can include dynamic data from the Odoo database.

### Key Features of QWeb Reports
- Dynamic Data Integration: QWeb Reports can dynamically pull and display data from the Odoo database using Odoo's ORM, making them ideal for generating up-to-date reports like invoices, sales orders, or payslips.

- HTML and CSS for Layout: The layout of a QWeb Report is defined using HTML and styled with CSS, giving great flexibility and control over the report's appearance.

- Template Inheritance: Similar to other QWeb templates, QWeb Reports can inherit from and extend other templates, promoting reusability and consistency across reports.

- PDF Generation: While QWeb templates are written in HTML, Odoo can convert these templates into PDFs for printing or downloading.

- Customization and Extensibility: Developers can create entirely custom reports or extend existing ones to meet specific business requirements.

### Basic Structure of a QWeb Report
> A QWeb report is defined in an XML file and includes HTML for layout, along with QWeb directives (t-if, t-foreach, t-esc, etc.) for dynamic content.

Example of a Simple QWeb Report Template
```
<t t-name="my_module.report_example">
    <t t-call="web.html_container">
        <t t-foreach="docs" t-as="doc">
            <div class="page">
                <h2>Report for <t t-esc="doc.name"/></h2>
                <p>Date: <span t-field="doc.date"/></p>
                <!-- More report content -->
            </div>
        </t>
    </t>
</t>
```
### In this example:

t-name defines the unique name of the report template.  
t-call="web.html_container" wraps the content in standard HTML for reports.  
t-foreach and t-as are used to iterate over the documents (docs) for which the report is being generated.  
t-esc and t-field are used to insert dynamic content into the report.  
### Creating and Registering a QWeb Report
To make the report available in Odoo, you must register it in the system. This is typically done in a data file (XML) that is included in the module's __manifest__.py.

### Example of Report Registration
```
<record id="action_report_example" model="ir.actions.report">
    <field name="name">Example Report</field>
    <field name="model">my_module.my_model</field>
    <field name="report_type">qweb-pdf</field>
    <field name="report_name">my_module.report_example</field>
    <field name="report_file">my_module/report/report_template</field>
    <field name="binding_model_id" ref="my_module.model_my_model"/>
</record>
```
### In this registration:

The report is linked to a model (my_module.my_model). 
report_name corresponds to the t-name in the QWeb template. 
report_file is the path to the QWeb template file. 
### Conclusion
> QWeb Reports are a powerful tool in Odoo for generating professional-quality PDF documents. They offer significant flexibility for customization, allowing businesses to tailor their reports to meet specific needs and branding requirements. Proper understanding and use of QWeb Reports are crucial for businesses that rely on accurate and timely reporting.
