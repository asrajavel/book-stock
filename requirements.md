# Bookstock requirements

Stage: clickable mock only (`index.html`, data in memory). No backend yet.
Separate from the Book Distribution app for now.

## Problem

Volunteers hold books. Some own them; some take them from the Temple on credit (pay later)
and pass some to others. Anyone who received books pays back the person they got them from,
either with the same books or their price. Everyone must see what they hold, what they owe and what is owed to them.

## Parties

- People, plus the Temple as one party (no login; volunteers enter their own Temple pickups, returns and payments).

## Entries

All entries: kind, from, to, date, books (title + language, count).

| Kind | Meaning | Limit |
| --- | --- | --- |
| Lend | from gives books to to, to be repaid | from's books in hand (Temple: any) |
| Return | from gives lent books back to to | books still on loan, and in from's hand |
| Pay | from pays to for lent books; no books move | books still on loan |
| Add | own books, owed to nobody | any |
| Out | books leave: distributed, gifted, lost | books in hand |

No money-only payments: a payment always names the books it pays for, at their price.

## Calculations (per title and language)

    on loan from A to B = lent A→B − returned B→A − paid B→A
    in hand            = received (lend, return, add) − sent (lend, return, out)

- Each direction is separate; A owing B and B owing A never cancel.
- Amount owed = value of books still on loan.

## Screens

- **Stock**: I owe ₹, owed to me ₹, books in hand. Bar: Books gone, Add own books.
- **People**: each person with "You have N · ₹" and "Has N of yours · ₹", or Settled.
- **Person**: "You have from A" (Return, Pay), "A has from you" (Got back, Got paid), history. Bar: Take books, Give books.
- **History**: my entries, newest first.

## Open points

- Price: today's price (mock) or the price on the lend day (parked design).
- Who may enter: either side, counts at once (parked design), or the other side confirms.
- Link to the Book Distribution app: should its sales count as "Out" automatically?
- Edit/delete entries, duplicate warning.
