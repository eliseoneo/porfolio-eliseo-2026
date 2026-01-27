# OCR → Section Extraction → MongoDB → Proposal PDF (FastAPI)

## Summary
A FastAPI backend that turns OCR outputs into structured sections stored in MongoDB, then generates professional PDF proposals using templates.

## Problem
- OCR text is messy and long; proposals require structured fields (dates/prices/terms).
- Need repeatable extraction + storage + document generation.

## Solution Overview (Spec)
- **Inputs**: image/PDF URL ingestion or uploaded artifacts
- **OCR**: text extraction saved to a results folder for traceability
- **Section Extraction**:
  - model-driven grouping (sentences → sections)
  - store only relevant sections (e.g., seasonal dates/pricing)
  - attach an alphanumeric `identifier` for retrieval and versioning
- **Persistence**: MongoDB collection for sections + metadata
- **PDF Generation**:
  - template-driven labeled sections (Concept, Prices, Dates, Notes)

## Deliverables
- API endpoints specification (batch OCR, extract, generate PDF)
- Storage schema for extracted sections
- PDF template layout rules and formatting constraints

## Tech
FastAPI, Python, MongoDB, NLP/transformer extraction, PDF generation

## Next Steps
- Add validation rules and confidence scoring per extracted field
- Add audit trail per identifier (source → extraction → proposal)