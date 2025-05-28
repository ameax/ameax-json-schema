# ameax Receipt Exchange Format

## Example File
[View Example File](../examples/ameax_receipt.json)

## JSON Schema for validation
[View Schema File](../schemas/ameax_organization_account.v1-0.schema.json)

### **JSON Structure: Receipt**

#### **Schema Version:** `1.0`

**Example JSON:**
```json
{
  "meta": {
    "document_type": "ameax_receipt",
    "schema_version": "1.0"
  },
  "type": "invoice",
  "identifiers": {
    "receipt_number": "INV-2025-001",
    "external_id": "X234",
    "ameax_internal_id": 321654
  },
  "business_id": 1,
  "user_external_id": "JD",
  "date": "2025-02-01",
  "customer_number": "12345",
  "status": "completed",
  "tax_mode": "net",
  "tax_type": "regular",
  "subject": "Your order No. 12345",
  "closure": "Thank you for your order. Our general terms and conditions apply.",
  "notice": "Prio-A Customer",
  "related_receipts": [
    { "type": "credit_note", "receipt_number": "CN-2025-002" },
    { "type": "invoice", "external_id": "X456" }
  ],
  "pursued_from": { "type": "order", "receipt_number": "ORD-2024-099" },
  "line_items": [
    {
      "article_number": "ART-001",
      "category": "Consulting",
      "description": "Consulting Services",
      "quantity": 1,
      "price": 200,
      "discount": 20,
      "discount_type": "percent",
      "tax_rate": 19.0,
      "tax_type": "regular"
    },
    {
      "article_number": "ART-002",
      "category": "Book",
      "description": "Book",
      "quantity": 2,
      "price": 20,
      "discount": 0.5,
      "discount_type": "amount",
      "tax_rate": 7.0,
      "tax_type": "reduced"
    }
  ],
  "custom_data": {
    "source_of_order": "shop",
    "what_ever": "x12"
  }
}
```

---

### **Explanation of JSON Structure**

#### **1. Meta Information (`meta`)** *(required, object)*
- **`document_type`** *(required, string)*: Specifies the type of document. In this case, `ameax_receipt` represents a business document (invoice, offer, order, etc.).
- **`schema_version`** *(required, string)*: Defines the version of the schema being used.

#### **2. Basic Information**
- **`type`** *(required, string)*: The type of receipt (e.g., invoice, order, offer).
- **`identifiers`** *(required, object)*: Contains unique identifiers for the receipt.
    - **`receipt_number`** *(required, string)*: A unique identifier assigned to the receipt.
    - **`external_id`** *(nullable, string)*: An optional external reference ID.
    - **`ameax_internal_id`** *(nullable, integer)*: An internal ameax ID that can be used as an alternative or in addition to external_id. **Note: When both `ameax_internal_id` and `external_id` are provided, `ameax_internal_id` takes precedence for record identification.**
- **`business_id`** *(nullable, integer)*: Identifier for the business entity handling the receipt.
- **`user_external_id`** *(nullable, string)*: External identifier of the user processing the receipt.
- **`date`** *(required, string, YYYY-MM-DD format)*: Date of the receipt creation.
- **`customer_number`** *(required, string)*: Customer identifier to link the receipt to a specific customer.
- **`status`** *(required, string, allowed values: draft, pending, completed, canceled)*: Current status of the receipt.
- **`tax_mode`** *(required, string, allowed values: net, gross)*: Defines whether prices include tax or not.
- **`tax_type`** *(required, string, allowed values: regular, reduced, exempt)*: Specifies the applicable tax type.


#### **3. Receipt Relations** *(nullable, object)*
- **`related_receipts`** *(nullable, array of objects)*: List of related receipts. Each related receipt must contain:
    - **`type`** *(required, string)*: Type of the related receipt (e.g., invoice, credit_note).
    - **`receipt_number`** *(nullable, string)*: The receipt number, if available.
    - **`external_id`** *(nullable, string)*: External identifier of the receipt, if applicable.
- **`pursued_from`** *(nullable, object)*: The receipt that preceded this one, containing:
    - **`type`** *(required, string)*: Type of the related receipt (e.g., order, quote).
    - **`receipt_number`** *(required, string)*: The receipt number of the pursued document.

#### **4. Content Details**
- **`subject`** *(nullable, string)*: The main subject of the receipt.
- **`closure`** *(nullable, string)*: Additional closing remarks on the receipt.
- **`notice`** *(nullable, string)*: Any internal or external notices related to the receipt.

#### **5. Line Items (`line_items`)** *(required, array of objects)*
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

#### **6. Custom Data (`custom_data`)** *(nullable, object)*
This section allows for additional data fields that may be required for specific use cases.

- **`source_of_order`** *(nullable, string)*: Indicates the origin of the order (e.g., shop, marketplace, manual entry).
- **`what_ever`** *(nullable, string)*: Example of a custom data field for flexible extensions.

This structured format ensures a standardized way of handling business documents within the system.
