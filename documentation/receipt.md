# ameax Receipt Exchange Format

## Example File
[View Example File](../examples/ameax_receipt.json)

### **JSON Structure: Receipt**

#### **Schema Version:** `1.0`

**Example JSON:**
```json {examples/ameax_organization.json}
```

---

### **Explanation of JSON Structure**

#### **1. Meta Information (`meta`)** *(required, object)*
- **`document_type`** *(required, string)*: Specifies the type of document. In this case, `ameax_receipt` represents a business document (invoice, offer, order, etc.).
- **`schema_version`** *(required, string)*: Defines the version of the schema being used.

#### **2. Basic Information**
- **`type`** *(required, string)*: The type of receipt (e.g., invoice, order, offer).
- **`receipt_number`** *(required, string)*: A unique identifier assigned to the receipt.
- **`business_id`** *(nullable, integer)*: Identifier for the business entity handling the receipt.
- **`user_external_id`** *(nullable, string)*: External identifier of the user processing the receipt.
- **`date`** *(required, string, YYYY-MM-DD format)*: Date of the receipt creation.
- **`customer_number`** *(required, string)*: Customer identifier to link the receipt to a specific customer.
- **`status`** *(required, string, allowed values: draft, pending, completed, canceled)*: Current status of the receipt.
- **`tax_mode`** *(required, string, allowed values: net, gross)*: Defines whether prices include tax or not.
- **`tax_type`** *(required, string, allowed values: regular, reduced, exempt)*: Specifies the applicable tax type.

#### **3. Content Details**
- **`subject`** *(nullable, string)*: The main subject of the receipt.
- **`closure`** *(nullable, string)*: Additional closing remarks on the receipt.
- **`notice`** *(nullable, string)*: Any internal or external notices related to the receipt.

#### **4. Line Items (`line_items`)** *(required, array of objects)*
Each item in the receipt includes:

- **`article_number`** *(nullable, string)*: The unique identifier of the product or service.
- **`category`** *(nullable, string)*: The category of the product or service.
- **`description`** *(required, string)*: A brief description of the product or service.
- **`quantity`** *(required, integer or float)*: The number of units of the item.
- **`price`** *(required, float)*: The unit price of the item.
- **`discount`** *(nullable, float)*: Discount applied to the item.
- **`discount_type`** *(nullable, string, allowed values: percent, amount)*: Defines how the discount is applied.
- **`tax_rate`** *(required, float)*: The applicable tax percentage.
- **`tax_type`** *(required, string, allowed values: regular, reduced, exempt)*: The type of tax applied to the item.

#### **5. Custom Data (`custom_data`)** *(nullable, object)*
This section allows for additional data fields that may be required for specific use cases.

- **`source_of_order`** *(nullable, string)*: Indicates the origin of the order (e.g., shop, marketplace, manual entry).
- **`what_ever`** *(nullable, string)*: Example of a custom data field for flexible extensions.

This structured format ensures a standardized way of handling business documents within the system.

