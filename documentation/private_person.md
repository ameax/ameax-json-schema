# ameax Private Person Exchange Format

## Example File
[View Example File](../examples/ameax_private_person.json)


## JSON Schema for validation
[View Schema File](../schemas/ameax_private_person_account.v1-0.schema.json)

### **JSON Structure: Private Person**

#### **Schema Version:** `1.0`

**Example JSON:**
```json
{
  "meta": {
    "document_type": "ameax_private_person_account",
    "schema_version": "1.0"
  },
  "salutation": "Mr.",
  "honorifics": "Dr.",
  "firstname": "John",
  "lastname": "Doe",
  "date_of_birth": "1980-05-12",
  "identifiers": {
    "customer_number": "123456",
    "ameax_internal_id": 987654
  },
  "address": {
    "route": "Musterstraße",
    "house_number": "12a",
    "postal_code": "12345",
    "locality": "Berlin",
    "country": "DE"
  },
  "communications": {
    "phone_number": "+49 30 987654",
    "mobile_phone": "+49 151 12345678",
    "email": "john.doe@example.com",
    "fax": "+49 30 765432"
  },
  "agent": {
    "external_id": "ms"
  },
  "custom_data": {
    "what_ever": "5"
  }
}
```

---

### **Explanation of JSON Structure**

#### **1. Meta Information (`meta`)** *(required, object)*

- **`document_type`** *(required, string)*: Specifies the type of document. In this case, `ameax_private_person_account` represents an individual person.
- **`schema_version`** *(required, string)*: Defines the version of the schema being used.
- **`import_mode`** *(optional, string or null)*: Controls how records are processed during import. Valid values are:
  - `"create_or_update"` (default): Creates new records or updates existing ones based on identifiers
  - `"create_only"`: Only creates new records, ignores updates to existing ones
  - `"update_only"`: Only updates existing records, ignores new ones
  - `null`: Same as default behavior (`"create_or_update"`)

#### **2. Personal Information**

- **`salutation`** *(nullable, string)*: Title describing the gender (Allowed values: Mr., Ms., Mx.).
- **`honorifics`** *(nullable, string)*: Additional academic or professional titles (e.g., Dr., Prof.).
- **`firstname`** *(required, string)*: First name of the individual.
- **`lastname`** *(required, string)*: Last name of the individual.
- **`date_of_birth`** *(nullable, string, YYYY-MM-DD format)*: The birthdate of the individual.

#### **3. Identifiers (`identifiers`)** *(nullable, object)*
- **`customer_number`** *(nullable, string)*: A unique identifier assigned to the customer within the system, used for assigning receipts.
- **`external_id`** *(nullable, string)*: An optional external reference ID to identify the record on future data updates.
- **`ameax_internal_id`** *(nullable, integer)*: An internal ameax ID that can be used as an alternative or in addition to external_id. **Note: When both `ameax_internal_id` and `external_id` are provided, `ameax_internal_id` takes precedence for record identification.**

#### **4. Address (`address`)** *(required, object)*

- **`route`** *(nullable, string)*: The street name. Can also include the house number.
- **`house_number`** *(nullable, string)*: The house or building number.
- **`postal_code`** *(required, string)*: ZIP or postal code of the location.
- **`locality`** *(required, string)*: City or town.
- **`country`** *(required, string, ISO 3166-1 alpha-2)*: Country code in ISO 3166-1 alpha-2 format.

#### **5. Communications (`communications`)** *(nullable, object)*

- **`phone_number`** *(nullable, string)*: The main phone number.
- **`mobile_phone`** *(nullable, string)*: A mobile contact number.
- **`email`** *(nullable, string, valid email format)*: Primary email address.
- **`fax`** *(nullable, string)*: Fax number (if applicable).

#### **6. Agent (`agent`)** *(nullable, object)*
- **`external_id`** *(nullable, string)*: An external reference ID to assign the agent in ameax.

#### **7. Custom Data (`custom_data`)** *(nullable, object)*

You can add any additional data that is not covered by the predefined structure. The custom data is handled individually via plugins.

- **`what_ever`** *(nullable, string)*: Example of a custom data field that can be adjusted based on system needs.

##### **Custom Fieldlib Fields (xcu_ prefix)**

Fields with the `xcu_` prefix are dynamically handled by the legacy system as custom customer fields:

- Fields must start with `xcu_` to be processed as fieldlib fields
- These fields are extracted during import and sent to the legacy API
- Available fields depend on the legacy AM system configuration
- Field types and validation are handled by the legacy system
- Non-prefixed fields remain in custom_data but are not processed as fieldlib fields

Example:
```json
"custom_data": {
  "xcu_category": 5,
  "xcu_kundenart": "private",
  "xcu_newsletter_subscriber": true,
  "notes": "VIP customer"  // non-xcu_ fields are ignored
}
```

This structured format ensures a standardized way of handling private person data within the system.

