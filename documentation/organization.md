# ameax Organization Exchange Format

## Example File
[View Example File](../examples/ameax_organization.json)

## JSON Schema for validation
[View Schema File](../schemas/ameax_organization_account.v1-0.schema.json)

### **JSON Structure: Organization (Company)**

#### **Schema Version:** `1.0`

**Example JSON:**

```json
{
  "meta": {
    "document_type": "ameax_organization_account",
    "schema_version": "1.0"
  },
  "name": "Example GmbH",
  "additional_name": "Example Solutions",
  "identifiers": {
    "customer_number": "12345",
    "external_id": "X234",
    "ameax_internal_id": 789123
  },
  "address": {
    "route": "Musterstraße",
    "house_number": "12a",
    "postal_code": "12345",
    "locality": "Berlin",
    "country": "DE"
  },
  "social_media": {
    "web": "https://example.com"
  },
  "communications": {
    "phone_number": "+49 30 123456",
    "mobile_phone": "+49 170 654321",
    "email": "info@example.com",
    "fax": "+49 30 654321"
  },
  "business_information": {
    "vat_id": "DE123456789",
    "iban": "DE123456789123465798"
  },
  "agent": {
    "external_id": "ms"
  },
  "custom_data": {
    "customer_segment": "Premium",
    "notes": "Important client",
    "xcu_category": 5
  },
  "contacts": [
    {
      "salutation": "Mr.",
      "honorifics": "Dr.",
      "firstname": "John",
      "lastname": "Doe",
      "identifiers": {
        "external_id": "789456",
        "ameax_internal_id": 456789
      },
      "date_of_birth": "1980-05-12",
      "employment": {
        "job_title": "CEO",
        "department": "Management"
      },
      "communications": {
        "phone_number": "+49 30 987654",
        "mobile_phone": "+49 151 12345678",
        "email": "john.doe@example.com",
        "fax": "+49 30 765432"
      },
      "custom_data": {
        "linkedin": "https://linkedin.com/in/johndoe"
      }
    }
  ]
}
```

---

### **Explanation of JSON Structure**

#### **1. Meta Information (`meta`)** *(required, object)*

- **`document_type`** *(required, string)*: Specifies the type of document. In this case, `ameax_organization_account` represents an organization.
- **`schema_version`** *(required, string)*: Defines the version of the schema being used.

#### **2. Basic Information**

- **`name`** *(required, string)*: The official name of the company.
- **`additional_name`** *(nullable, string)*: An alternative or subsidiary name used by the company.

#### **3. Identifiers** *(nullable, object)*
- **`customer_number`** *(nullable, string)*: A unique identifier assigned to the customer within the system e.g. debtor number. This one is used to assign receipts to the customer.
- **`external_id`** *(nullable, string)*: An optional external reference ID to identify the record on future data updates.
- **`ameax_internal_id`** *(nullable, integer)*: An internal ameax ID that can be used as an alternative or in addition to external_id. **Note: When both `ameax_internal_id` and `external_id` are provided, `ameax_internal_id` takes precedence for record identification.**

#### **4. Address** *(required, object)*

- **`route`** *(nullable, string)*: The street name. Can also be route with house_number
- **`house_number`** *(nullable, string)*: The house or building number.
- **`postal_code`** *(required, string)*: ZIP or postal code of the location.
- **`locality`** *(required, string)*: City or town.
- **`country`** *(required, string, ISO 3166-1 alpha-2)*: Country code in ISO 3166-1 alpha-2 format.

#### **5. Social Media (`social_media`)** *(nullable, object)*

- **`web`** *(nullable, string, URL)*: The company’s website URL.

#### **6. Communications (`communications`)** *(nullable, object)*

- **`phone_number`** *(nullable, string)*: The main phone number.
- **`mobile_phone`** *(nullable, string)*: A mobile contact number.
- **`email`** *(nullable, string, valid email)*: Primary email address of the business like info@example.com.
- **`fax`** *(nullable, string)*: Fax number (if applicable).

#### **7. Business Information (`business_information`)** *(nullable, object)*

- **`vat_id`** *(nullable, string, VAT format)*: VAT Identification number, applicable for tax purposes.
- **`iban`** *(nullable, string, IBAN format)*: International Bank Account Number for financial transactions.

#### **8. Agent (`agent`)** *(nullable, object)*
- **`external_id`** *(nullable, string)*: An external reference ID to assign the agent in ameax.

#### **9. Custom Data (`custom_data`)** *(nullable, object)*

You can add any data here that's not covered by the structure. The custom data is handled individually via plugins.

- **`customer_segment`** *(nullable, string)*: Classification of the customer (e.g., Premium, Standard).
- **`notes`** *(nullable, string)*: Any additional information or remarks about the customer.
- **`xcu_category`** *(nullable, integer)*: A custom categorization field.

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
  "xcu_kundenart": "wholesale",
  "xcu_rabattsatz": 10.5,
  "other_field": "ignored"  // non-xcu_ fields are ignored
}
```

#### **10. Contacts (`contacts`)** *(nullable, array of objects)*

Each contact within the company contains the following fields:

- **`salutation`** *(nullable, string)*: Title describing the gender. Used to address the person (Allowed values: Mr., Ms., Mx.)
- **`honorifics`** *(nullable, string)*: Additional academic or professional titles (e.g., Dr., Prof.).
- **`firstname`** *(nullable, string)*: First name of the contact.
- **`lastname`** *(required, string)*: Last name of the contact.
- **`identifiers.external_id`** *(nullable, string)*: Optional unique ID for the contact to identify the record on updates.
- **`identifiers.ameax_internal_id`** *(nullable, integer)*: An internal ameax ID for the contact that can be used as an alternative or in addition to external_id. **Note: When both `ameax_internal_id` and `external_id` are provided, `ameax_internal_id` takes precedence for record identification.**
- **`date_of_birth`** *(nullable, string, YYYY-MM-DD format)*: The birthdate of the contact.
- **`employment.job_title`** *(nullable, string)*: The job title of the contact within the company.
- **`employment.department`** *(nullable, string)*: The department where the contact works.

##### **Contact Communications (`communications`)** *(nullable, object)*

- **`phone_number`** *(nullable, string)*: Office phone number of the contact.
- **`mobile_phone`** *(nullable, string)*: Personal mobile number.
- **`email`** *(nullable, string, valid email format)*: Contact email address.
- **`fax`** *(nullable, string)*: Fax number if available.

##### **Contact Custom Data (`custom_data`)** *(nullable, object)*

You can add any data to the contact here. It will be handled via plugin on import

- **`linkedin`** *(nullable, string, URL)*: URL of the LinkedIn profile of the contact.

###### **Custom Fieldlib Fields (xpe_ prefix)**

Fields with the `xpe_` prefix are dynamically handled by the legacy system as custom person/contact fields:

- Fields must start with `xpe_` to be processed as fieldlib fields
- These fields are extracted during import and sent to the legacy API
- Available fields depend on the legacy AM system configuration
- Field types and validation are handled by the legacy system
- Non-prefixed fields remain in custom_data but are not processed as fieldlib fields

Example:
```json
"custom_data": {
  "xpe_employee_id": "EMP001",
  "xpe_department": "IT",
  "xpe_cost_center": "CC123",
  "linkedin": "https://linkedin.com/in/johndoe"  // non-xpe_ field preserved but not processed as fieldlib
}
```


This structured format ensures a standardized way of handling customer data within the system.

