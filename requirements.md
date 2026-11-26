# Bookstock requirements

Stage: clickable mock only (`index.html`, data in memory, starts empty). No backend yet.
Live at https://asrajavel.github.io/book-stock/ (GitHub Pages from main).
Separate from the Book Distribution app for now.

## Problem

Volunteers hold books. Some own them; some take them from the Temple on credit (pay later)
and pass some to others. Anyone who received books pays back the person they got them from,
either with the same books or their price. Everyone must see what they hold, what they owe and what is owed to them.

## Parties

- People, plus the Temple as one party (no login; volunteers enter their own Temple pickups, returns and payments).
- **Chaitanya Center**: a shelf, no login. Anyone can put their books there; they stay theirs (by owner, like piles).
  It never owes anything of its own. Any volunteer records what happens there:
  - Put books: still the owner's (and, for Temple's books, still owed to the Temple by whoever took them).
  - Take books: taking someone else's books means you owe that owner; taking your own back is a return.

## Piles (whose books)

Each person's books in hand are kept in piles by whose they are: **Own**, **Temple's** (only for whoever took
them from the Temple), or the volunteer who owns them ("Pannaga's"). Giving or distributing books always says which pile.

A book's **owner** is the volunteer who brought it in: their own book, or one they took from the Temple.
Only the person who took books from the Temple owes the Temple for them; everyone else owes that person.
Passing on someone else's books moves the debt: the new holder owes the owner directly, the passer is out of it.
Giving the owner their own books back is a return. So debts never go round in a circle.

## Entries

All entries: kind, from, to, date, books (title + language, count); Lend and Out also name the pile.

| Kind | Meaning | Limit |
| --- | --- | --- |
| Lend | from gives books from their own or Temple's pile; they go into to's "from's" pile | that pile (Temple: any) |
| Pass | from gives on another volunteer's books; to now owes that owner | that pile |
| Return | to gets the books back into the pile they were lent from | owed books still in from's pile |
| Pay | from pays to for owed books; no books move | owed books sold (lent-on books are paid after that person pays) |
| Add | own books, owed to nobody | any |
| Out | books leave a pile: distributed, gifted, lost (still owed to that pile's person) | that pile |

- No money-only payments: a payment always names the books it pays for, at their price.
- Sold or lent-on books can't be returned, only paid for (or returned by whoever has them now).
- A pile below zero (e.g. after an edit) shows in red as "Missing an entry"; the holder fixes it.
- Everyone can see everyone's stock, loans and history (volunteers are trusted).
- **Either side records**: the giver (Give, Return, Pay) or the receiver (Take, Take back, Collect cash); it counts at once.
- **Notify the other side**: the person who didn't make the entry gets a push notification
  ("Pannaga gave you 5 books", "Ravi took 5 of your books") and fixes it if wrong. Temple and Center: no one to notify.

## Calculations (per title and language)

    owed by B to A = lent A→B − returned B→A − paid B→A
    pile           = books in − books out (lend, return, out), per pile

- Each direction is separate; A owing B and B owing A never cancel.
- Amount owed = value of books still owed.
- The person page splits owed books into groups, still held (Return), sold (incl. gifted, lost), with
  the cash to pay or collect (Pay), and with others (grey, no button). Holder sees Return / Pay; lender sees Take back / Collect cash.

## Screens

- **Stock**: Need to pay ₹, Need to get ₹;
  **Books in hand** (one list per pile: Own, Temple's, Pannaga's…); **At Chaitanya Center** (my books there);
  **Cash in hand to pay others**: money from books sold that is still owed, one row per person (tap opens them).
  Money from own books isn't tracked. Bar: Sell books, Buy books.
- **People**: a who-owes-whom map on top (everyone on a circle, arrow from payer to payee with ₹,
  your arrows in blue; tap a person to open them), then each person with "You need to pay ₹ · N books", "You need to get ₹ · N books", or All clear.
- **Person** (volunteer): two tabs. **Between us**: "Need to pay A · ₹" and "Need to get from A · ₹", each only when not empty,
  buttons under each group; history (collapsed); no bar. **A's stock**: same sections as my Stock (no totals). Bar: Take books, Give books.
  Opens on Between us, or on A's stock when nothing is pending.
- **Temple page**: "Need to pay Temple · ₹" (Return / Pay), history (collapsed). Bar: Take books.
- **Pickers** for Give, Take and Sell books start with a Whose books switch (Own / Temple's / …).
- **Chaitanya Center page**: Books here (by owner, mine first), history (collapsed). Bar: Take books, Put books. In People it is the last row (square icon); it is left off the map.
- **History**: my entries, newest first; a give or distribute from someone else's pile says whose.

## Open points

- Which loan a return or payment settles when the lender lent from several piles (mock: pile id order; better: oldest first).
- Price: today's price (mock) or the price on the lend day (parked design).
- Link to the Book Distribution app: should its sales count as "Out" automatically?
- Edit/delete entries, duplicate warning.
- Selling at the Chaitanya Center (it would hold the sale cash for each owner until collected).
