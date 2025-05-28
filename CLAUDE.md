# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains JSON Schema definitions and documentation for the ameax data exchange API. It defines standardized formats for exchanging customer data between systems.

## Architecture

The project defines 4 main data types:
- **Organizations** (`ameax_organization_account.v1-0.schema.json`) - Business entities with contacts
- **Private Persons** (`ameax_private_person_account.v1-0.schema.json`) - Individual customers
- **Receipts** (`ameax_receipt.v1-0.schema.json`) - Financial documents with line items
- **Sales** (`ameax_sale.v1-0.schema.json`) - Sales opportunities with 7-point qualification

All schemas follow JSON Schema Draft-07 specification and include:
- Required `meta` object with `document_type` and `schema_version`
- Optional `custom_data` object for extensibility
- Support for both internal (`customer_number`) and external (`external_id`) identifiers

## Key Technical Details

### Data Validation
- All JSON documents must validate against their respective schemas before processing
- Schemas use format validation for emails, dates (YYYY-MM-DD), and URLs
- Country codes follow ISO 3166-1 alpha-2 standard
- Nullable fields explicitly support null values

### Integration Methods
1. **Webhook (Immediate)**: POST to `https://{database_name}.ameax.de/rest-api/imports`
2. **FTP/TLS (Delayed)**: Upload to `/incoming/` directory with queue-based processing

### Schema Versioning
- Current version: 1.0 for all schemas
- Version specified in both filename and meta.schema_version field

## Development Notes

This is a documentation/specification repository - no build or test commands required. When modifying schemas:
- Ensure backward compatibility when possible
- Update corresponding documentation in `/documentation/`
- Update example files in `/examples/` to reflect changes
- Maintain consistency across all four schema types