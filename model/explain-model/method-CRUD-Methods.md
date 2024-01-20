# CRUD methods
CRUD methods in Odoo are fundamental operations that allow you to interact with the database to Create, Read, Update, and Delete records. These methods are part of Odoo's ORM (Object-Relational Mapping) and are essential for managing data in Odoo applications.

## 1. Create (.create(vals))
The create method is used to create a new record in the database.

### Usage:
vals is a dictionary containing the initial values of the new record.
Example:
```
new_partner = self.env['res.partner'].create({'name': 'New Partner', 'email': 'new@example.com'})
```
## 2. Read (.browse(ids), .search(domain), Field Access)
- Browse:

The browse method is used to retrieve records by their IDs.
Returns a recordset containing the records of the given IDs.
Example:
```
partner = self.env['res.partner'].browse(1)  # Retrieves the record with ID 1
```
- Search:

The search method is used to retrieve records matching a domain.
Returns a recordset containing all records that match the domain criteria.
Example:
```
customers = self.env['res.partner'].search([('customer', '=', True)])\
```
- Field Access:

Reading the value of a field is as simple as accessing a property.
Example:
```
name = partner.name  # Reading the name field of the partner record
```
## 3. Update (.write(vals))
The write method is used to update existing records.

### Usage:
vals is a dictionary where keys are field names, and values are the new values for those fields.
Example:
```
partner.write({'email': 'updated@example.com'})  # Updates the email of the partner
```
## 4. Delete (.unlink())
The unlink method is used to delete records.

### Usage:
Deletes all records in the recordset.
Example:
```
partner.unlink()  # Deletes the partner record
```
### Best Practices and Considerations
- Security Checks: Odoo automatically performs access rights checks according to the user's permissions.
- Record Rules: Custom record rules can further restrict CRUD operations based on complex domain rules.
- Data Integrity: When using write or unlink, be cautious about data integrity, especially with relational fields.
- Transactions: CRUD operations are transactional. If an error occurs during a transaction, all changes made in that transaction are rolled back.
- Performance: When dealing with many records, performance considerations should be taken into account. For example, it's often more efficient to use write on a recordset than on individual records in a loop.
> CRUD methods are the backbone of data manipulation in Odoo, allowing developers to interact with database records in a high-level, Pythonic way, abstracting the underlying SQL details. They are used extensively in Odoo development for managing business data.
