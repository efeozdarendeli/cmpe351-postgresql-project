# Requirements — initial draft

Selected: **S4 — Event Ticketing (Ladin Tickets)**, schema `ticket`. This draft now uses the Final catalog's actors and core workflow. The full BR1–BR12 enforcement plan and detailed ER model are incomplete.

## Purpose

Ladin Tickets sells concert, theatre, and cinema tickets online and at box offices. Model organizers, venues and halls, performances, categorized seats, bookings, payments/refunds, promotions, and check-in.

## Catalog actors and proposed tasks

Actor names are from the catalog; the task/data breakdown is an initial interpretation.

| Actor | Main tasks | Data read | Data created or changed |
| --- | --- | --- | --- |
| Box-office clerk | Hold seats and confirm bookings | Availability, customers, price lists, booking balances | Bookings, lines, payment records; seat changes through procedures |
| Door staff | Check a ticket in once | Booking, performance, line and prior check-in | Check-in time and gate through `check_in_ticket` |
| Event planner | Schedule events/performances and assign ushers | Venues, halls, sections, staff | Event details, schedules, assignments |
| Finance officer | Review payments/refunds and organizer agreements | Sales, payments, refunds, confidential contract terms | Financial records and contract terms, subject to the privilege design |
| Marketing officer | Maintain and review promo codes | Events and redemption counts | Promo codes and validity/usage limits |
| Administrator | Manage configuration and access | Operational and confidential data | Authorized administrative changes |

Customers are entities and initiate online bookings, but the six actors above are the catalog's explicit actor list. The security catalog also requires the read-only `auditor` role; include it in the privilege matrix.

## Confirmed catalog facts

- Venue → hall → section → seat is the seating hierarchy. Each section has a ticket category.
- Each performance belongs to an event and takes place in one hall, with start/end times.
- Prices are per performance and category. Each booking line retains the price charged.
- **A booking belongs to exactly one performance.** Each line is one ticket for one seat.
- A booking starts as a **10-minute hold**. Confirmation needs at least one line, a valid hold, and every selected seat HELD by that booking; confirmation makes the seats SOLD.
- Booking states follow HELD → CONFIRMED → CHECKED_IN. EXPIRED is allowed only from HELD; CANCELLED only from HELD or CONFIRMED. Expiry/cancellation releases seats.
- Check each ticket in once, for a CONFIRMED or CHECKED_IN booking; the first check-in changes the booking to CHECKED_IN.
- Seat state is per performance: AVAILABLE, HELD, SOLD, or BLOCKED. Keep current state and a history explaining each change; `performance.seats_remaining` must not be negative.
- A booking uses at most one promo code. Validate dates and maximum uses against redemption rows.
- Payments support card, cash, and gift vouchers. Partial refunds cannot exceed the amount paid.
- Booking total is `(sum of line prices - promo discount + 10.00 TRY per ticket) * 1.20`; payments cannot exceed the total.
- Organizer commission rates and contract fees are confidential to finance officers and administrators. Changes to those terms and price lists are audited.

These summarize the source; they are not yet a complete numbered transcription or enforcement plan for BR1–BR12.

## Successful operation — hold and confirm

1. A clerk creates a booking for one customer's selected performance.
2. `hold_seats(booking_id, seat_ids)` holds available seats and records their booking-time prices and status history.
3. Within 10 minutes, `confirm_booking(booking_id)` verifies all lines are still held by this booking and changes all those seats to SOLD.

Preserve exclusive allocation per performance/seat, correct hall membership, price snapshots, history/current-state agreement, and the booking total. Payment timing relative to confirmation is an assumption to resolve; BR2's stated confirmation conditions do not themselves require full payment.

## Rejected operation — competing hold

Another booking requests a seat already HELD or SOLD for the same performance. Reject it, preserve the existing allocation, and leave no partial seat holds, lines, or history entries from the failed operation. This must remain true under concurrent requests.

## Design assumptions and open questions

1. Represent a ticket directly by `(booking_id, line_no)`, matching the required check-in procedure arguments. Decide where check-in time, gate, and staff belong.
2. A nullable staff venue reference represents head office. Decide how top-level supervisors are represented and prohibit supervision cycles.
3. Start with one event table and checked type-specific columns; justify this against subtype tables.
4. Resolve price rounding, expired-hold cleanup, and moving bookings before implementation.
5. Finish a faithful numbered BR1–BR12 transcription, including atomic move-booking and deletion rules. Accessibility extraction can omit clipped table cells; verify them visually rather than inventing missing text.
