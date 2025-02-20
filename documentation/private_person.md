# ameax Private Person Exchange Format

## Example File
[View Example File](../examples/ameax_private_person.json)

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
    "customer_number": "123456"
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

#### **2. Personal Information**

- **`salutation`** *(nullable, string)*: Title describing the gender (Allowed values: Mr., Ms., Mx.).
- **`honorifics`** *(nullable, string)*: Additional academic or professional titles (e.g., Dr., Prof.).
- **`firstname`** *(required, string)*: First name of the individual.
- **`lastname`** *(required, string)*: Last name of the individual.
- **`date_of_birth`** *(nullable, string, YYYY-MM-DD format)*: The birthdate of the individual.

#### **3. Identifiers (`identifiers`)** *(nullable, object)*
- **`customer_number`** *(nullable, string)*: A unique identifier assigned to the customer within the system, used for assigning receipts.

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

#### **6. Custom Data (`custom_data`)** *(nullable, object)*

You can add any additional data that is not covered by the predefined structure. The custom data is handled individually via plugins.

- **`what_ever`** *(nullable, string)*: Example of a custom data field that can be adjusted based on system needs.

This structured format ensures a standardized way of handling private person data within the system.

