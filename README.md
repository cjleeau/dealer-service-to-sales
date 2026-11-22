# Service-to-sales: a beginner's guide

This folder turns Claude, ChatGPT or whichever AI assistant you already use into a service-to-sales assistant. Each evening you give it tomorrow's service bookings, what those cars are worth and your current stock, and it writes a short brief telling your sales team which service customers might be ready to upgrade, and which car on the lot to show them.

It never contacts a customer. It only writes a note for your team, and a person always decides whether to have the conversation.

You do not need to code anything. If you can download a report and attach a file to an email, you can run this.

---

## What is in this folder

| File | What it is | Do you need to edit it? |
|---|---|---|
| `SKILL.md` | The instructions for the AI. This is the brains of the whole thing. | No |
| `rules-template.md` | A short form describing your dealership (warranty lengths, what counts as a big service, which brands you sell). | Yes, fill it in once |
| `sample-data/` | Five made-up files for a fictional dealership, so you can practise first. | No |
| `sample-data/expected-brief.md` | What the result should roughly look like when you run the sample data. | No |

---

## Before you start: two things that matter

**1. Use a business AI plan, not a free account.** Business and team plans come with contract terms about how your data is stored and whether it is used to train the AI. Free consumer accounts usually do not. The Office of the Australian Information Commissioner recommends that organisations do not put personal information into publicly available AI tools.

**2. Practise on the sample data first.** Everything in `sample-data` is made up, so you can try the whole process without touching a single real customer record. Only move to real bookings once you are happy with how it works, and once you have read the privacy section at the bottom of this guide.

---

## Part 1: Set it up (once, about ten minutes)

The simplest way, which works in Claude, ChatGPT and most other assistants, is to create a project so the instructions are remembered every time.

1. **Download this folder.** On GitHub, click the green **Code** button and choose **Download ZIP**, then unzip it somewhere easy to find, like your desktop.
2. **Create a new project** in your AI assistant and call it something like "Service-to-sales". Both Claude and ChatGPT have Projects in the side menu. If yours does not, skip to the note below.
3. **Open `SKILL.md`** in Notepad (Windows) or TextEdit (Mac), select everything, copy it, and paste it into the project's **instructions** box. Save.

That is the setup done. From now on, every chat you start inside that project already knows how to do the job.

> **No projects in your AI assistant?** Just attach `SKILL.md` at the start of each new chat along with your other files, and type "Follow the instructions in SKILL.md". It works the same way, you just have to do it each time.
>
> **Using Claude and comfortable with settings?** You can also save the folder as a skill. Search Claude's help pages for "skills" for the current steps. The project approach above is easier if you are just getting started.

---

## Part 2: Practise with the sample data (about five minutes)

1. Start a **new chat inside your project**.
2. Attach the five files from the `sample-data` folder: `bookings.csv`, `valuations.csv`, `stock.csv`, `exclusions.csv` and `rules.md`.
3. Type this message and send it:

   > Pretend today is Monday 23 November 2026. Please run the service-to-sales brief for tomorrow's bookings using the attached files.

   The "pretend today is" part matters. The sample bookings are dated 24 November 2026, and the instructions tell the AI to stop if the bookings are not for tomorrow.

4. **Compare the result** with `sample-data/expected-brief.md`. The wording will be a little different each time, but you should see three High and two Medium bookings, and the people listed under "What to check" should not appear.

If something looks wrong, check you attached all five files and included the date line, then try again in a fresh chat.

---

## Part 3: Get it ready for your dealership (once)

**Fill in your rules.** Open `rules-template.md`, replace the examples with your own details and save it as `rules.md`. The most important lines are your warranty lengths by make and what you count as a big service bill. You can change these at any time.

**Set up your three regular files.** You need these each evening. Column names do not have to match exactly, the AI will work out what is what, but the closer you get to the sample files the better.

1. **Tomorrow's service bookings.** Most DMS and booking systems can produce a scheduled report each evening. Ask your DMS provider or whoever looks after your system to set one up with these columns: booking time, booking reference or first name, rego, make, model, variant, build year, first registration date if available, kilometres, booked service, estimated cost of the whole job (including any quoted extras like tyres or brakes), private or business owned, and any service notes. Open `sample-data/bookings.csv` to see an example. Leave out surnames, phone numbers, email addresses and home addresses.
2. **Valuations.** An indicative trade value and the date it was valued for each car, looked up by rego or VIN from whichever valuation service you already use, such as AutoGrab. See `sample-data/valuations.csv`. If a car has no valuation, the AI will say "value not available" rather than guess.
3. **Current stock.** Your stock list, usually the same feed that goes to your website. See `sample-data/stock.csv`.

**Exclusions.** If your DMS or CRM can export a list of booking references for customers with no marketing consent, open complaints, warranty disputes or recent purchases, add that too (see `sample-data/exclusions.csv`). If it cannot, put an extra column in your bookings report for marketing consent and the AI will use that instead. Do not skip this step.

---

## Part 4: Run it every day (about two minutes)

1. In the evening, or first thing in the morning, start a **new chat inside your project**.
2. Attach tomorrow's bookings, the valuations, your stock list, your exclusions and your `rules.md`.
3. Type: **"Please run the service-to-sales brief for tomorrow's bookings."**
4. Read the brief before sending it. It takes about a minute, and you know your customers better than the AI does.
5. Copy the **morning brief** into an email, Teams or Slack message for the sales team.
6. Copy the **outcome log rows** into a Google Sheet or your CRM.

After each conversation, the salesperson fills in the outcome column as **Interested**, **Not now** or **Not suitable**.

**Make it smarter over time.** Every few weeks, attach your outcome log along with the usual files. The AI will look at what has been working and suggest changes to your rules, for example "major service alone rarely leads to interest, consider requiring a second signal". It only suggests. You decide whether to update `rules.md`.

---

## Common questions

**The AI says the bookings are not for tomorrow and stops.** Check the date in your bookings report. If you are running it in the morning for the same day, say so: "Run the brief for today's bookings."

**It suggested a car we sold yesterday.** Your stock list is out of date. Export a fresh one before running the brief. The AI will warn you when the stock list is more than a day old.

**It listed someone who should not have been approached.** Check that person is in your exclusions file or marked in the consent column, then run it again. Exclusions are applied before anything else, so if the flag is there, the person will not appear.

**The warranty dates look a bit off.** They are estimates, worked out from first registration (or build year if that is all there is) plus the standard warranty in your rules. Always confirm in the DMS before mentioning it to a customer.

**Can it send messages to customers?** No, and it is designed not to. It only writes an internal brief for your team.

**Can it look up valuations itself?** No. You need to provide the valuations file. This keeps the AI working only with data you have chosen to give it.

---

## Privacy: read this before using real bookings

Service bookings contain your customers' personal information. Putting that into an AI tool may be a use or disclosure of personal information under the Privacy Act 1988. Some smaller businesses fall under the Act's small business exemption, but treat the data as if the Act applies either way.

- **De-identify the export.** Leave out surnames, phone numbers, email addresses and home addresses. The brief only needs a first name or booking reference, the car and the booking time. Your team can look the customer up in the DMS.
- **Use a business AI plan** with proper data protections, and check where your provider stores data and whether it trains on your uploads.
- **Respect marketing consent.** Customers who have not agreed to marketing must be in your exclusions, every time.
- **Check with your privacy officer or adviser** before running this on live customer data. This is a working example, not legal advice.

---

Part of the "Dealership AI, one feature at a time" series by Chris Lee. Free to use and adapt, no subscription attached.
