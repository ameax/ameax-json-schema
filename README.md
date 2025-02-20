### **API Documentation: ameax Data Exchange Format**

#### **Introduction**

This documentation describes the JSON format for exchanging customer data. There are three types of data structures:

1. **Organizations** (with contacts)
2. **Private persons**
3. **Receipts** (invoices, offers, orders, etc.)

Data can be transferred via **Webhook** for instant processing or via **FTP/S** for slightly delayed processing. Processing is always handled in a queue-based system to ensure scalability.

---

### **Data Transfer Methods**

#### **Webhook (Immediate Processing)**

To send data via Webhook, make a `POST` request to:

```plaintext
https://{replace_with_your_database_name}.ameax.de/rest-api/json-import
```

**Headers:**

- `Content-Type: application/json`
- `Authorization: Bearer <your-api-key>`

**Example Request:**

```bash
curl -X POST https://{replace_with_your_database_name}.ameax.de/rest-api/json-import \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -d @organization.json
```

Webhook data is processed immediately through the queue.

---

#### **FTP/TLS Upload (Delayed Processing)**

Customers may also upload JSON files via FTP/TLS. Processing starts a few minutes after file upload.

**FTP/S Server Details:**

- **Host:** `ftpupload.aranes.de`
- **Port:** `21` (Explicit TLS) / `990` (Implicit TLS)
- **Username/Password:** Provided upon request
- **Directory Structure:**
    - `/incoming/` → Upload new files
    - `/processed/` → Successfully processed files
    - `/failed/` → Files that failed validation

**Example Upload via FTP/TLS:**

```bash
lftp -e "set ftp:ssl-force true; open ftp.example.com; user your_username your_password; cd /incoming; put organization.json; bye"
```

---

### Formats

- [Organization](documentation/organization.md)
- [Private person](documentation/private_person.md)
- [Receipt](documentation/receipt.md)

### **JSON Schema Validation**

The first step in processing any file is validation against a JSON Schema. The schema file will be available in the GitHub repository.

Example validation using `ajv-cli`:

```bash
ajv validate -s organization_schema.json -d organization.json
```

---

##
