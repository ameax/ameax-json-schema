# ameax Sale Exchange Format

## Example File
[View Example File](../examples/ameax_sale.json)

## JSON Schema for validation
[View Schema File](../schemas/ameax_sale.v1-0.schema.json)

### **JSON Structure: Sale**

#### **Schema Version:** `1.0`

**Example JSON:**
```json
{
  "meta": {
    "document_type": "ameax_sale",
    "schema_version": "1.0"
  },
  "identifiers": {
    "external_id": "S234",
    "ameax_internal_id": 147258
  },
  "customer": {
    "customer_number": "12345",
    "external_id": "X234"
  },
  "subject": "Opportunity 12345",
  "description": "Description of the opportunity",
  "sale_status": "active",
  "selling_status": "identification",
  "user_external_id": "JD",
  "date": "2025-02-01",
  "amount": 1000.00,
  "probability": 50,
  "close_date": "2025-07-01",
  "rating": {
    "relationship": {
      "rating": 4,
      "source": "known"
    },
    "proposition": {
      "rating": 3,
      "source": "assumed"
    },
    "trust": {
      "rating": 5,
      "source": "known"
    },
    "competition": {
      "rating": 2,
      "source": "guessed"
    },
    "need_for_action": {
      "rating": 4,
      "source": "known"
    },
    "buying_process": {
      "rating": 3,
      "source": "assumed"
    },
    "price": {
      "rating": 4,
      "source": "known"
    }
  },
  "custom_data": {
    "xsa_category": "industry",
    "what_ever": "x12"
  }
}
```

---

### **Explanation of JSON Structure**

#### **1. Meta Information (`meta`)** *(required, object)*
- **`document_type`** *(required, string)*: Specifies the type of document. In this case, `ameax_sale` represents a sales opportunity.
- **`schema_version`** *(required, string)*: Defines the version of the schema being used.

#### **2. Identifiers** *(required, object)*
- **`external_id`** *(required, string)*: A unique external identifier for the sales record.
- **`ameax_internal_id`** *(nullable, integer)*: An internal ameax ID that can be used as an alternative or in addition to external_id. **Note: When both `ameax_internal_id` and `external_id` are provided, `ameax_internal_id` takes precedence for record identification.**

#### **3. Customer Information** *(required, object)*
At least one of the following fields must be present:
- **`customer_number`** *(required if no external_id)*: Internal customer number
- **`external_id`** *(required if no customer_number)*: External customer identifier

#### **4. Basic Information**
- **`subject`** *(required, string)*: Title or subject of the sales opportunity
- **`description`** *(nullable, string)*: Detailed description of the opportunity
- **`sale_status`** *(required, string)*: Current status of the sale
  - Allowed values: "active", "inactive", "completed", "cancelled"
- **`selling_status`** *(required, string)*: Current stage in the sales process
  - Allowed values: "identification", "acquisition", "qualification", "proposal", "sale"
- **`user_external_id`** *(required, string)*: External identifier of the responsible user
- **`date`** *(required, string, YYYY-MM-DD format)*: Date of the sales opportunity
- **`amount`** *(required, number)*: Expected sales amount (decimal with 2 decimal places)
- **`probability`** *(required, integer)*: Probability of closing the sale (0-100)
- **`close_date`** *(required, string, YYYY-MM-DD format)*: Expected closing date

#### **5. Rating System** *(required, object)*
The rating system contains evaluations for various aspects of the sales opportunity. Each rating consists of:
- **`rating`** *(required, integer)*: Rating from 1-7
- **`source`** *(required, string)*: Source of the rating
  - "known": Verified information
  - "assumed": Well-founded assessment
  - "guessed": Uncertain information

Rating categories:
- **`relationship`**: Quality of relationship
- **`proposition`**: Value proposition
- **`trust`**: Level of trust
- **`competition`**: Competitive situation
- **`need_for_action`**: Need for action
- **`buying_process`**: Buying process
- **`price`**: Price situation

#### **6. Custom Data** *(nullable, object)*
This section allows for additional data fields that may be required for specific use cases.
- **`xsa_category`** *(nullable, string)*: Example of a custom categorization field
- **`what_ever`** *(nullable, string)*: Example of a custom data field for flexible extensions

This structured format ensures a standardized way of handling sales opportunities within the system. 