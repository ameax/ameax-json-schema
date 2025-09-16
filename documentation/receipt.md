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
  "sale_external_id": "SALE-2025-042",
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
      "uom": "hours",
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
      "uom": "pieces",
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
- **`import_mode`** *(optional, string or null)*: Controls how records are processed during import. Valid values are:
  - `"create_or_update"` (default): Creates new records or updates existing ones based on identifiers
  - `"create_only"`: Only creates new records, ignores updates to existing ones
  - `"update_only"`: Only updates existing records, ignores new ones
  - `null`: Same as default behavior (`"create_or_update"`)

#### **2. Basic Information**
- **`type`** *(required, string)*: The type of receipt (e.g., invoice, order, offer).
- **`identifiers`** *(required, object)*: Contains unique identifiers for the receipt.
    - **`receipt_number`** *(required, string)*: A unique identifier assigned to the receipt.
    - **`external_id`** *(nullable, string)*: An optional external reference ID.
    - **`ameax_internal_id`** *(nullable, integer)*: An internal ameax ID that can be used as an alternative or in addition to external_id. **Note: When both `ameax_internal_id` and `external_id` are provided, `ameax_internal_id` takes precedence for record identification.**
- **`business_id`** *(nullable, integer)*: Identifier for the business entity handling the receipt.
- **`user_external_id`** *(nullable, string)*: External identifier of the user processing the receipt.
- **`sale_external_id`** *(nullable, string)*: External identifier to link this receipt to a sales opportunity. **Note: When using this field, the corresponding sale record should be synchronized first to ensure proper linkage.**
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
- **`uom`** *(nullable, string)*: Unit of measurement (e.g., pieces, hours, kg, meters).
- **`price`** *(required, float)*: The unit price of the item.
- **`discount`** *(nullable, float)*: Discount applied to the item.
- **`discount_type`** *(nullable, string, allowed values: percent, amount)*: Defines how the discount is applied.
- **`tax_rate`** *(required, float)*: The applicable tax percentage.
- **`tax_type`** *(required, string, allowed values: regular, reduced, exempt)*: The type of tax applied to the item.

#### **6. Document PDF (`document_pdf`)** *(nullable, object)*
Allows attaching the PDF document for this receipt. Two methods are supported:

- **`type`** *(required, string, allowed values: base64, url)*: Method of providing the PDF document.
  - `"base64"`: Direct embedding of the PDF as Base64-encoded string. **Note: Use this method only for PDFs smaller than 1MB to avoid performance issues.**
  - `"url"`: External HTTPS URL to download the PDF.

When `type` is `"base64"`:
- **`content`** *(required, string)*: Base64-encoded PDF content.

When `type` is `"url"`:
- **`url`** *(required, string)*: HTTPS URL to download the PDF. Can include authentication parameters (e.g., pre-signed S3 URLs, API tokens as query parameters).

**Examples:**

Base64 method (for small PDFs < 1MB):
```json
"document_pdf": {
  "type": "base64",
  "content": "JVBERi0xLjQKJeLjz9MK..."
}
```

URL method (recommended for larger files):
```json
"document_pdf": {
  "type": "url",
  "url": "https://api.example.com/documents/INV-2025-001.pdf?token=abc123"
}
```

#### **7. Custom Data (`custom_data`)** *(nullable, object)*
This section allows for additional data fields that may be required for specific use cases.

- **`source_of_order`** *(nullable, string)*: Indicates the origin of the order (e.g., shop, marketplace, manual entry).
- **`what_ever`** *(nullable, string)*: Example of a custom data field for flexible extensions.

This structured format ensures a standardized way of handling business documents within the system.
