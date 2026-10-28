# Speaker notes

Supply Chain Analytics: End to End. One short talk track per slide.

## 1. Supply Chain Analytics, end to end

Open with who you are and one sentence: I built a small but complete supply chain analytics stack, from the apps that create the data to the dashboard that tells the story. Everything runs in the cloud on free tiers, and all data is synthetic, so I can show every layer openly. The talk has two parts: how I built it, then the story the dashboard tells.

## 2. The 30-second pitch

Say this almost word for word. The key message: I understand the whole chain, not just the visual layer. Most dashboards start from a ready dataset; I started from the systems that create the transactions, so I know where every number comes from and can defend it.

## 3. How I built it

Transition line: Let me start with the architecture, then walk through how I built it in six steps.

## 4. The architecture

Walk left to right. Two source systems, each with its own database, like a real company with an ERP and a CRM. A Python ETL reads both through read-only logins on a 15-minute schedule and loads a Neon Postgres warehouse in layers. Power BI connects with its own read-only login in Import mode. A small web console lets me monitor runs, trigger a load and see data-quality results. Point to make: each layer has one job, and no tool writes back to the sources.

## 5. How I built it, in six steps

Tell it as a sequence. One: I built an ERP-style app so purchase orders, receipts and sales orders exist as real transactions. Two: a CRM, because the ERP has no customer, price or budget, and an analyst needs Demand vs Actual vs Budget. Three: an ETL that reads both sources without changing them. Four: a warehouse with a star schema. Five: two report schemas designed around the job description, inventory health and S and OP. Six: Power BI on top. Close with the habit: I checked every step with real numbers before moving on.

## 6. Steps 1–2: the two source apps

Key points: the ERP is the system of record for operational transactions. I never change its tables from the analytics side. The CRM fills the gap the ERP has: who the customer is, what they were forecast to buy, and what we budgeted. Mention security briefly: row-level security stays on, the browser only has a publishable key, and the CRM database password lives only in a server function.

## 7. Step 3: one ETL run, every 15 minutes

The ERP tables have no updated-at timestamps, so I could not rely on change dates. Instead the ETL reads each table, hashes every row and compares with the last load, then inserts or updates only what changed. The proof is in three runs: the first loaded 2,038 rows, a second run loaded nothing because nothing changed, and after I linked orders to customers in the CRM, the next run picked up exactly those 393 updates. That's what incremental and idempotent mean in practice.

## 8. Steps 4–5: the warehouse layers

Explain the layering in one breath: staging is a faithful copy, dw is the star schema, analytics holds the business logic as views, and the two report schemas are shaped for Power BI. I designed the report schemas around the job description: inventory health and supplier performance for the distribution side, and demand versus actual versus budget for S and OP. Why views? The calculations live in one place, they are testable, and anyone can verify a dashboard number with one SQL query.

## 9. Built like production, on a small scale

Interviewers like to hear about something that went wrong. Use the scheduler story: GitHub's free cron fired once in six hours, so instead of trusting it I added a heartbeat trigger that an external timer calls, and the console's own schedule decides whether to run. Another short one if asked: a password containing an at sign broke the connection string, so I added a guard that stops before connecting and never prints the secret.

## 10. The dashboard story

Transition line: Now the part a manager actually sees. The dashboard isn't a collection of charts; it's one argument in five steps, and each page title states its finding.

## 11. The story arc: one page per step

This is the structure to remember if you forget everything else. Situation, complication, impact, root cause, action. Each page answers one question and raises the next. Tell the audience up front: I'll take you from "are we OK?" to "here's what we do on Monday."

## 12. Customers are served well, but supply is shaky

Read the headline banner aloud: fill rate 96.7 percent, 3 SKU-locations at risk, supplier on-time 45 percent. Then the contrast: customers are mostly getting their orders, but fewer than half of our purchase orders arrive on time. That gap is the story. Use the region slicer once, for example click KY, to show it's interactive, then clear it. If asked about the trend line: inventory value moved between about 24 and 33 thousand dollars per snapshot and ended at 21 thousand on 26 October.

## 13. Five SKU-locations need action, and the list is ready

Explain the logic in one sentence, because it shows analyst thinking: each SKU at each DC gets a status from its days of supply, calculated from the last 28 days of demand, compared with the supplier's lead time plus a 7-day buffer. Under that it's reorder now, zero is a stockout, over 90 days is excess. Then point to the table: it already suggests an order quantity. Also mention excess: two items tie up cash, so the fix is rebalancing, not just buying more. Bridge: but are customers actually feeling this?

## 14. Fill rate held at 100% for three months, then fell in October

This is the "so what" page. Supply problems took three months to reach customers: July to September every order shipped complete, then October fell to 82.9 percent. No DC meets the 98 percent target. Be honest about the caveat before anyone asks: October is the current month, so some of those orders are open, not failed. That honesty builds trust. Unit fill rate is 95.7 percent if asked. Bridge: so why is this happening? Look upstream.

## 15. Only 45% of POs arrive on time, and one supplier drives a third of exceptions

Two different supplier problems, and that distinction is the insight. CleanCore has the most exceptions because it gets the most orders, 22 of 36. Pacific Workwear has fewer POs but the worst on-time rate, 23 percent. So volume and reliability are separate conversations. Northstar at 80 percent shows the target is reachable. Use the supplier slicer or click a bar to cross-filter. Bridge: what kind of exceptions are these, and what do we do?

## 16. 9 in 10 exceptions are late deliveries: $66K of $75K

Land the story here. The problem isn't wrong orders or bad data; 88 percent of exceptions are simply late deliveries. That means the fix is supplier management and buffer stock, not process redesign. Then give three actions with time frames: this week, this month, this quarter. End with a sentence like: "If we do these three things, I'd expect October's fill rate back above 98 percent, and the dashboard will show us whether it worked." Then stop and invite questions.

## 17. Questions to be ready for

Keep answers short and confident. If you don't know something, say what you would check and where: "I'd confirm that in the warehouse view." Questions about real UniFirst systems: be clear this is a demo on synthetic data, built to show how I'd approach the problem.

## 18. Thank you

Close with one line that ties both parts together: I built the apps that create the data, the pipeline that cleans it, and the dashboard that turns it into three actions. Then name the next step, the Demand vs Actual vs Budget report, which is already modelled in the warehouse. Thank them and open for questions.

