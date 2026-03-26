# Review Findings: Receipt Status & Dokumentation

**Datum:** 2026-02-17
**Scope:** `schemas/ameax_receipt.v1-0.schema.json`, `documentation/receipt.md`, `examples/ameax_receipt.json`
**Status:** Nur Analyse — keine Änderungen, Entscheidungen hängen von Server-Projekt ab

---

## 1. Status-Vollständigkeit

### 1.1 Schema-Enum vs. Dokumentation

Das Schema definiert 13 Status-Werte (Zeile 39):

| Status | Offer | Order | Invoice/CN/Cancel | Delivery Note | Doku vorhanden? |
|---|---|---|---|---|---|
| `draft` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `outstanding` | ✅ | | ✅ | ✅ | ✅ |
| `accepted` | ✅ | | | | ✅ |
| `obsolet` | ✅ | | | | ✅ |
| `refused` | ✅ | | | | ✅ |
| `in_progress` | | ✅ | | | ✅ |
| `completed` | | ✅ | ✅ | ✅ | ✅ |
| `cancelled` | | ✅ | | | ✅ |
| `read_for_dispatch` | | | ✅ | | ✅ |
| `on_hold` | | | ✅ | | ✅ |
| **`outstanding_payment`** | | | ? | | ⚠️ **FEHLT** |
| **`cancellation`** | | | ? | | ⚠️ **FEHLT** |
| **`paused`** | | | ? | | ⚠️ **FEHLT** |

### 1.2 Findings zu fehlenden Status

- **`outstanding_payment`**: Im Schema vorhanden, in der Doku keinem Typ zugeordnet. Wird laut Analyse im Server-Plugin für Invoices verwendet, fehlt aber in `availableByType()` auf dem Server. **Abwarten, ob Server nachbessert — dann für Invoice-Typen dokumentieren.**
- **`cancellation`**: Im Schema vorhanden, keinem Typ zugeordnet in der Doku. Klären: Welcher Typ nutzt diesen Status? Ist das ein Legacy-Wert?
- **`paused`**: Im Schema vorhanden, keinem Typ zugeordnet in der Doku. Laut Server-Analyse ebenfalls keinem Typ zugeordnet. Klären: Soll `paused` entfernt werden oder einem Typ zugewiesen werden?

---

## 2. Deprecation-Hinweis für `pending`

- **`pending` ist NICHT im Schema-Enum** — korrekt.
- **`pending` ist NIRGENDS in der Dokumentation erwähnt** — ⚠️ problematisch.
- `pending` war seit Juni 2025 der Default-Status im PHP API-Package (`ameax-json-import-api`). Bestehende Clients könnten diesen Wert noch senden.
- Im API-Package (Branch `feature/receipt-status-expansion`) wird `pending` als `@deprecated` beibehalten.

### Empfehlung

Ein Abschnitt "Deprecated Values" sollte ergänzt werden, z.B.:

> **Deprecated Status Values**
>
> | Value | Deprecated since | Hinweis |
> |---|---|---|
> | `pending` | v1.0 (2026-xx) | War der ursprüngliche Default-Status im API-Package. Wird serverseitig auf `draft` gemappt (Mapping-Entscheidung beim Server-Projekt abwarten). Neue Implementierungen sollten `draft` verwenden. |

**Abhängigkeit:** Wie genau `pending` gemappt wird (→ `draft`?) muss im Server-Projekt entschieden werden.

---

## 3. Delivery Note (`deliverynote`)

### Finding: Typ fehlt im Schema!

- Die **Dokumentation** listet "Delivery Note" als Typ mit Status `draft`, `outstanding`, `completed` (Zeile 105).
- Das **Schema** definiert als `type`-Enum nur: `offer`, `order`, `invoice`, `credit_note`, `cancellation_document` (Zeile 23).
- **`deliverynote` (oder `delivery_note`) fehlt im Schema-Enum!**
- Der Server kennt `deliverynote` als Typ.

### Handlungsbedarf

Entweder:
1. `delivery_note` zum Schema-Enum hinzufügen (konsistent mit Snake-Case-Konvention der anderen Typen) — **Vorsicht:** Server verwendet `deliverynote` (ohne Underscore), Schema-Konvention wäre `delivery_note`.
2. Oder Dokumentation korrigieren, falls Delivery Notes bewusst nicht über die API importiert werden sollen.

**Klärung nötig:** Soll der Import-Typ `deliverynote` oder `delivery_note` heißen? Welche Konvention gilt?

---

## 4. Allgemeine Konsistenz-Findings

### 4.1 ⚠️ Falscher Schema-Link in der Dokumentation

`documentation/receipt.md`, Zeile 7:
```markdown
[View Schema File](../schemas/ameax_organization_account.v1-0.schema.json)
```
Verlinkt auf das **Organization-Schema** statt auf das Receipt-Schema. Muss sein:
```markdown
[View Schema File](../schemas/ameax_receipt.v1-0.schema.json)
```

### 4.2 ⚠️ `tax_type` Enum-Mismatch (Top-Level)

- **Schema** (Zeile 41): `regular`, `reduced`, `exempt_eu`, `exempt_third`, `exempt_other`
- **Dokumentation** (Zeile 107): `regular`, `reduced`, `exempt`

Die Dokumentation zeigt die **Line-Item-Werte** statt der **Top-Level-Werte**. Die Top-Level-`tax_type` hat differenziertere Exempt-Kategorien.

**Korrektur nötig:** Doku Zeile 107 muss die 5 Werte des Top-Level-Schemas zeigen.

### 4.3 ⚠️ `import_status` nicht dokumentiert

Das Schema definiert `meta.import_status` (Zeilen 16-19) als optionales Objekt mit `additionalProperties: true`. Dieses Feld wird in der Dokumentation nicht erwähnt.

**Klären:** Ist `import_status` ein Response-Feld (vom Server befüllt) oder kann es beim Import mitgeschickt werden? Falls nur Response → ggf. im Schema als read-only kennzeichnen oder in der Doku als "wird vom Server befüllt" dokumentieren.

### 4.4 `pursued_from.external_id` nicht dokumentiert

- **Schema** (Zeilen 61-72): `pursued_from` unterstützt sowohl `receipt_number` als auch `external_id` (via `anyOf`).
- **Dokumentation** (Zeilen 115-118): Erwähnt nur `type` und `receipt_number`, nicht `external_id`.

**Korrektur nötig:** `external_id` als Alternative zu `receipt_number` dokumentieren.

### 4.5 Inkonsistenz zwischen Dokumentations-Beispiel und Example-Datei

- Doku-Beispiel (Zeile 28): `"sale_external_id": "SALE-2025-042"`
- Example-Datei (Zeile 14): `"sale_external_id": "OP123"`

Keine funktionale Auswirkung, aber sollte vereinheitlicht werden.

### 4.6 `customer_external_id` — Status

- **Schema:** Vorhanden (Zeile 38), inklusive `anyOf`-Constraint (Zeilen 137-140).
- **Dokumentation:** Vorhanden und korrekt dokumentiert (Zeile 100), inklusive Hinweis auf Pflicht-Alternative.
- **Befund: ✅ Korrekt dokumentiert.** Kein Handlungsbedarf.

---

## 5. Zusammenfassung der Handlungsbedarfe

### Sofort umsetzbar (keine Server-Abhängigkeit)

| # | Finding | Priorität |
|---|---|---|
| 4.1 | Schema-Link korrigieren (organization → receipt) | Hoch |
| 4.2 | Top-Level `tax_type` Enum in Doku korrigieren | Hoch |
| 4.4 | `pursued_from.external_id` dokumentieren | Mittel |
| 4.5 | Example-Wert für `sale_external_id` vereinheitlichen | Niedrig |

### Abhängig von Server-Entscheidungen

| # | Finding | Abhängigkeit |
|---|---|---|
| 1.2 | `outstanding_payment` Typ-Zuordnung | Server: `availableByType()` für Invoices |
| 1.2 | `cancellation` Typ-Zuordnung | Server: Klären, ob Legacy oder aktiv |
| 1.2 | `paused` Typ-Zuordnung | Server: Keinem Typ zugeordnet |
| 2 | `pending` Deprecation-Hinweis | Server: Mapping-Entscheidung (→ `draft`?) |
| 3 | `delivery_note` Typ zum Schema hinzufügen | Server: Naming-Konvention klären |
| 4.3 | `import_status` dokumentieren | Server: Zweck des Feldes klären |
