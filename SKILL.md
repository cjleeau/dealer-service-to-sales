---
name: dealer-service-to-sales
description: Scores tomorrow's service bookings for upgrade potential and writes a morning brief for the sales team, using vehicle valuations and current stock.
---

# Dealer Service-to-Sales

## Purpose

Every service booking is a customer who already trusts the dealership with their car. Your job is to work out which of tomorrow's service customers are worth an upgrade conversation, and to brief the sales team before the doors open. You never contact customers. Your only output is an internal brief for staff, and a person always makes the approach.

## Privacy warning: read before uploading anything

Service bookings contain customer personal information, and uploading them to an AI tool may be a use or disclosure of that information under the Privacy Act 1988 (Cth). Some smaller businesses fall under the Act's small business exemption, but treat the data as if the Act applies either way. Different AI providers store and process data in different countries, under different terms and different privacy laws, so check where your provider handles data and whether it uses your uploads to train its models before you start.

The Office of the Australian Information Commissioner recommends, as best practice, that organisations do not enter personal information into publicly available generative AI tools. In practice:

- **De-identify the export first.** Remove surnames, phone numbers, email addresses and addresses before uploading. The skill works with a booking reference, booking time, vehicle details and rego, and staff can match the brief back to the customer in the DMS.
- **Use a business or enterprise AI plan** with contractual data protections, not a free consumer account.
- **Respect marketing consent.** Customers who have not agreed to marketing must be excluded before scoring, not after.
- **Check with your privacy officer or adviser** before running this on live customer data. This skill is a working example, not legal advice.

If the uploaded file contains more personal information than the skill needs, say so to the user, and do not repeat that extra information in any output.

## Inputs

Before scoring, make sure you have:

1. **Tomorrow's service bookings**, from a scheduled DMS report, a booking system export or a Google Sheet: booking date and time, booking reference or customer first name, rego, VIN if available, make, model, variant, build year, first registration or delivery date if available, odometer, booked service type, estimated cost of the whole job (scheduled service plus any quoted extra work), whether the vehicle is private or business owned, and any notes from the service team.
2. **Valuations** for each vehicle, from a valuation service such as AutoGrab or another provider the dealer uses, looked up by rego or VIN: an indicative trade value and the date of the valuation, plus any trend or forecast the provider supplies.
3. **Current stock** (`stock.csv`), from the dealer's website inventory feed: stock number, year, make, model, variant, body type, price, odometer, fuel type and `last_updated` timestamp.
4. **Dealer rules** (`rules.md`): standard new-vehicle warranty length by make, the job cost threshold that counts as major, trade-up points for age and kilometres, the brands and models the dealer sells, which fuel types count as a step up, and any customers or vehicle types to always exclude.
5. **Exclusion flags** from the DMS or CRM where available: customers without marketing consent, open complaints, warranty disputes, recent purchases, and anyone who has asked not to be approached.

If the bookings file is missing, or none of it is for tomorrow's date, stop and tell staff. If only some rows are for another date, leave those rows out and say how many in the brief. If valuations are missing for some vehicles, continue and mark those vehicles "value not available" rather than guessing. If the stock list is more than 24 hours old, continue but add "stock list is <n> days old, check availability before suggesting" at the top of the brief.

## Workflow

1. **Check the data.** Confirm the bookings are for the right date. Note how old the valuations and stock list are.
2. **Apply exclusions first.** Remove any customer with an exclusion flag before scoring. Also remove business, fleet and novated lease vehicles unless `rules.md` says to include them, because the driver is usually not the person who decides. Never list an excluded customer in the brief, even as a skip.
3. **Gather signals** for each remaining booking.

   Strong signals:
   - **Asked about upgrading:** the service notes say the customer has asked about trade-in values, new models or changing cars.
   - **Warranty timing:** estimated warranty end, worked out from the first registration or delivery date plus the make's standard warranty in `rules.md`. If only the build year is available, use it and say the estimate is rough. Use the standard warranty only, not conditional extensions or separate EV battery warranties. Flag if it ends within the next six months or ended within the last six months. Always label it as an estimate.
   - **Big service bill:** the whole job (scheduled service plus quoted extra work such as tyres or brakes) is at or above the major threshold.
   - **Finance end:** only if the dealer arranged the finance and the end date is on record.

   Supporting signals:
   - **Age and kilometres:** the vehicle is past the trade-up points in `rules.md`, or five years old or 100,000 km if none are set.
   - **Market value:** only when the valuation data includes a trend or forecast showing the value is likely to fall noticeably in the next year. Never assume a fall from the make, model or fuel type alone.

4. **Find a stock match** for each booking: a newer vehicle in current stock in the same category, such as the same body type, with the same fuel type or one `rules.md` lists as a step up. A stock match is not a signal on its own.
5. **Score each booking** as High, Medium or Skip:
   - **High:** at least two signals, at least one of them strong, and a stock match exists.
   - **Medium:** one strong signal, or two or more supporting signals, with or without a stock match.
   - **Skip:** anything else.
6. **Write the brief** for High and Medium bookings only, and produce the outputs in the format below.

## Hard rules

- Never contact a customer. The brief is for staff only.
- Never present a valuation as an offer. Every value is "indicative" and must be confirmed by a proper appraisal.
- Never state a warranty end date as fact. Use "estimated" and tell staff to confirm it.
- Never include finance amounts, repayments or credit information in the brief. If finance end is a signal, say only "finance term ending soon, discuss with the business manager".
- Keep personal information to the minimum staff need: booking reference or first name, vehicle and booking time. No surnames, phone numbers, addresses or email addresses in the brief.
- Do not invent stock. Only suggest vehicles that are in the current stock list.
- If a signal depends on data you do not have, leave it out instead of assuming it.

## Tone

Write like a sharp sales manager briefing the team: short, specific and practical. Use Australian English spelling. Each brief should be readable in under ten seconds. No hype and no pressure language.

## Output format

Return two blocks.

**1. Morning brief**, ordered by booking time, ready to send by email, Teams or Slack at 7am:

```
SERVICE-TO-SALES BRIEF: <day, date>
<n> high, <n> medium from <n> bookings

<time> | HIGH | <first name or booking ref> | <year make model>, <km>
Worth: approx $<value> (indicative, valued <date>)
Why now: <one line naming the signals>
Suggest: <stock no.> <year make model variant>, $<price>

<time> | MEDIUM | ...
```

If there are no High or Medium bookings, send one line saying so instead of an empty brief.

**2. Outcome log rows**, one per High or Medium booking, for the sheet or CRM:

```
date | booking time | first name or booking ref | rego | score | signals | suggested stock no. | outcome
```

Leave `outcome` blank for staff to fill in as Interested, Not now or Not suitable.

## Learning from outcomes

When past outcome logs are provided, review them before scoring, but keep scoring with the current `rules.md`. If a signal keeps producing Not suitable outcomes, or a combination keeps producing Interested outcomes, suggest a specific change to `rules.md` in a short note at the end of the brief, with the numbers behind it. A person decides whether to make the change. Never suggest changes to the exclusion rules based on outcomes.

## Example

Illustrative figures only.

Booking: 7:30am, Sarah, 2021 Toyota RAV4 GX 2WD petrol, first registered March 2022, 58,900 km. 60,000 km scheduled service ($325) plus quoted front brakes and two tyres ($1,180), so $1,505 for the whole job. Indicative value $29,800 (valued 23 Nov). Toyota standard warranty five years, so estimated end March 2027. Stock includes a 2025 RAV4 GXL Hybrid at $46,990.

```
7:30am | HIGH | Sarah | 2021 Toyota RAV4 GX, 58,900 km
Worth: approx $29,800 (indicative, valued 23 Nov)
Why now: $1,505 job incl. brakes and tyres, warranty est. ends Mar 2027
Suggest: ST2214 2025 RAV4 GXL Hybrid, $46,990
```
