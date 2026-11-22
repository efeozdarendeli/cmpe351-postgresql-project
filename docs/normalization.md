# Normalization — B2 and B3 working sheet

Status: **not solved yet**. The actual S4 inputs have now been recovered from Project 03 · Final and recorded below. The guide's community garden example must not be substituted for these exercises.

## B2 — legacy export

The catalog supplies:

```text
legacy_sales(order_no, order_date, customer_email, customer_name, customer_city,
             performance_code, performance_start, event_code, event_title,
             organizer_name, hall_code, venue_city, seats, category_code,
             list_price, paid_price)

order_no            -> order_date, customer_email, performance_code
customer_email      -> customer_name, customer_city
performance_code    -> performance_start, event_code, hall_code
event_code          -> event_title, organizer_name
hall_code           -> venue_city
category_code       -> list_price
order_no, seat_code -> category_code, paid_price
```

`seats` contains values such as `PAR-C12, PAR-C13, PAR-C14`; after flattening, each row contains one `seat_code`. The old system used one fixed category price for the season. Cancelled orders remain in the export, so the same performance/seat can occur in different orders. Do not assume `(performance_code, seat_code)` is a candidate key.

Extra question: explain why `category_code -> list_price` no longer holds with per-performance price lists, and why every ticket still stores `paid_price` despite being able to look up prices, linking the explanation to BR4 and BR5.

No complete sample dataset was present in the recovered exercise text. Prepare a small illustrative sample for the reconstruction checks and label it as student-created.

Required analysis:

1. Copy the supplied relation and dependencies faithfully, distinguishing any additional assumptions.
2. Flatten repeating/non-atomic values to obtain a 1NF sample; define exactly what one row represents.
3. Compute attribute closures to find **all** candidate keys. Show that no proper subset of each key determines all attributes and explain why no keys are missing.
4. For 2NF, identify each partial dependency on a proper subset of a candidate key and show the relations after the split.
5. For 3NF, identify each violating dependency and show the relations after the split.
6. For BCNF, identify any remaining determinant that is not a superkey and show the final decomposition. If a stage already satisfies the target form, explain why.
7. Establish losslessness for each binary split: the intersection of the resulting attribute sets must determine one side under the relevant dependencies.
8. Reconstruct the flattened sample and run `original EXCEPT ALL reconstructed` and `reconstructed EXCEPT ALL original`. Save actual output; both must be empty. Sample agreement supplements the losslessness argument rather than proving it for every instance.
9. Check dependency preservation separately using the dependencies projected onto the resulting relations. Identify any dependency needing enforcement across tables.
10. Answer the exact extra question from the catalog.

The supplied inputs above are recorded; candidate-key closures, decompositions, proofs, sample rows, SQL checks, and the extra-question answer are still to be written.

## B3 — 3NF versus BCNF

The catalog supplies `usher_assignment(performance_code, section_code, usher_id)`: for a performance and section there is exactly one usher, and each usher always works in the same section. Translating those statements gives:

```text
performance_code, section_code -> usher_id
usher_id                      -> section_code
```

These are the exercise's assumptions; do not automatically impose the second FD on live staff assignments in the new application.

- List the relation's attributes and dependencies.
- Compute all candidate keys and identify prime attributes.
- For each nontrivial dependency `X -> A`, show why it satisfies 3NF: `X` is a superkey or `A` is prime.
- Identify a determinant that is not a superkey, explaining the BCNF violation.
- Decompose and establish losslessness.
- Identify the dependency that crosses the resulting tables, explain whether it is preserved, and propose a concrete enforcement method if needed.

Keep B3 separate from the larger B2 exercise. Do not substitute an invented example for the assigned relation.
