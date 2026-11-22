# Source notes and scenario choice

## Sources

- Supplied `CMPE351_PostgreSQL_Project_Guide.pdf` in Downloads, read locally.
- **Project 03 · Final — PostgreSQL Database Project**, read in the user's authenticated Chrome session at `https://classroom.magibu.ai/` on 22 November 2026.
- Supplied milestone/submission screenshots and the lecturer's email.

The page uses Flutter CanvasKit. Its saved MHTML contains a canvas rather than the assignment text. Enabling accessibility exposes text for visible portions of the assignment; scrolling and capturing that layer recovered the catalog and B2/B3 inputs.

The working reading copy is stored locally in Downloads as `CMPE351_Project03_Final_extracted.txt`. It is not a layout-faithful export: table cells can appear out of order or be omitted, repeated labels were deduplicated, and table numbers need visual verification. Do not infer missing rules or volumes from it. The live rendered assignment remains authoritative.

## Selected scenario: S4 — Event Ticketing

The assignment explicitly states that every scenario has the same structure and difficulty. This comparison is a judgment about manageability, not an official ranking:

| Scenario | Main additional complexity |
| --- | --- |
| S1 Furniture ERP | Material/product stock, BOM consumption, production, purchasing, credit limits and invoicing |
| S2 Quiz & Exam Platform | Versioned questions, four answer types, automatic/manual marking, appeals and finalization |
| S3 Food Ordering | Recipes/prep recipes, ingredient stock, purchasing, delivery shifts and wallets |
| **S4 Event Ticketing** | Seat holds/status history, bookings, prices, promotions and check-in |
| S5 Scooter Rental | Batteries, parking, tariffs, wallets, maintenance and rebalancing |

Choose S4 for its clear core workflow: **hold seats → confirm booking → check tickets in**. Its B2 exercise also has a repeating-seat field rather than multivalued contact details. It still requires the full common rubric, concurrency, refunds, promotions, and all catalog objects.
