# Expected result for the sample data

This is roughly what you should get back when you run the skill on the sample files, with the date set to Monday 23 November 2026. The wording will vary a little each time, but the scores and the people included should match. All names, regos and figures are made up.

```
SERVICE-TO-SALES BRIEF: Tuesday 24 November
3 high, 2 medium from 10 bookings
Stock list is 2 days old, check availability before suggesting.
1 booking for Wednesday 25 November left out.

7:30am | HIGH | Sarah | 2021 Toyota RAV4 GX, 58,900 km
Worth: approx $29,800 (indicative, valued 23 Nov)
Why now: $1,505 job incl. brakes and tyres, warranty est. ends Mar 2027
Suggest: ST2214 2025 RAV4 GXL Hybrid, $46,990

8:00am | HIGH | James | 2019 Hyundai Tucson Active X, 112,400 km
Worth: approx $19,500 (indicative, valued 23 Nov)
Why now: asked about trade-in last visit, $1,240 105k service, 112,000 km
Suggest: ST2231 2024 Tucson Elite Hybrid, $41,990

8:15am | MEDIUM | Priya | 2023 Tesla Model 3 RWD, 41,000 km
Worth: approx $33,000 (indicative, valued 23 Nov)
Why now: warranty est. ends Apr 2027
Suggest: no close match in stock

11:00am | HIGH | Ben | 2021 Hyundai Kona Electric Elite, 62,000 km
Worth: approx $21,500 (indicative, valued 23 Nov)
Why now: warranty est. ended May 2026, 5 years old
Suggest: ST2260 2024 Kona Electric Premium, $44,990

1:00pm | MEDIUM | Grace | 2016 Toyota Corolla Ascent, 138,000 km
Worth: approx $9,800 (indicative, valued 23 Nov)
Why now: 10 years old, 138,000 km
Suggest: ST2252 2023 Corolla Ascent Sport Hybrid, $28,990
```

Outcome log:

```
2026-11-24 | 7:30am | Sarah | 1AB2CD | High | warranty, big job | ST2214 |
2026-11-24 | 8:00am | James | 2EF3GH | High | asked to upgrade, big job, km | ST2231 |
2026-11-24 | 8:15am | Priya | 3IJ4KL | Medium | warranty | none |
2026-11-24 | 11:00am | Ben | 9GH1IJ | High | warranty ended, age | ST2260 |
2026-11-24 | 1:00pm | Grace | 1KL2MN | Medium | age, km | ST2252 |
```

## What to check

- **Daniel and Tom do not appear anywhere.** Daniel has no marketing consent and Tom has an open complaint, so both are removed before scoring.
- **Craig's HiLux does not appear.** It is a business fleet vehicle, and the sample rules exclude those.
- **Mei's CX-5 does not appear.** It has no valuation and only one supporting signal (age), so it scores Skip.
- **Aisha's 2024 i30 does not appear.** Nothing about it suggests an upgrade.
- **Liam's Sportage does not appear.** His booking is for the wrong day.
- **Every suggested car is in the stock list**, and no surnames, phone numbers or email addresses appear.

If your result includes any of the people above, or suggests a car that is not in `stock.csv`, something has gone wrong. Check you uploaded all five sample files (bookings, valuations, stock, exclusions and rules) and told the AI the date.
