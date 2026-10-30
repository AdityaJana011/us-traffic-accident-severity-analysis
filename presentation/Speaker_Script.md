# Presentation Script — US Accidents 2016–2022
**DS612 IDV · Group 7 · 30 minutes · 4 speakers**

---

## Speaker Allocation at a Glance

| # | Speaker | Slides | Topic | Time |
|---|---------|--------|-------|------|
| 1 | **Dhruv Parmar** | 1, 2, 3 | Opening, Severity definition, Data trust | ~7 min |
| 2 | **Aditya Jana** | 4, 5 | Hypothesis 1 — Weather | ~7 min |
| 3 | **Mahak Khurdia** | 6, 7 | Hypothesis 2 — Time of Day | ~7 min |
| 4 | **Falak Parmar** | 8, 9, 10, 11 | Key Insights, Verdict, Close, Q&A | ~8 min |

**Story arc:** Set up the puzzle → test two "obvious" hypotheses → both partially fail → reveal what *actually* drives severity → end on a decisive call to action.

---

# 🎤 SPEAKER 1 — DHRUV (~7 min)

## Slide 1 — Title (~1 min)

> *[Walk to center. Pause. Make eye contact.]*

"Good morning, everyone. I'd like to start with a number.

**Seven point seven million.**

That's how many traffic accidents were recorded across the United States between 2016 and 2022 — nearly forty-nine states, seven years of data, captured from traffic APIs, police reports, and road sensors. One of the largest publicly available accident datasets in the world.

Our group spent several weeks with this data, and we came in with two very common assumptions — that bad weather makes accidents worse, and that rush hours are the most dangerous time to be on the road.

Today, we're going to show you what the data actually says. And I'll warn you now — the answers surprised us.

I'm Dhruv, and along with Aditya, Mahak, and Falak, we're Group 7."

> *[Transition]* "Before we touch a single chart, we have to settle one question — and it's the most important question of this entire project."

---

## Slide 2 — Severity ≠ Physical Injury (~3 min)

> *[Point to the "Severity ≠ Physical Injury" text.]*

"When you hear the word **severity** in an accident dataset, what do you picture?

Most people picture the same thing — injuries. Crashes. Fatalities. Something dramatic.

**That's not what severity means in this dataset.**

Severity here is scored from 1 to 4, and it measures **how long traffic was disrupted**. How long the road was blocked. How long cars had to wait.

A **Severity 1** is a minor fender-bender cleared in minutes. A **Severity 4** is a major, prolonged blockage — think of a jackknifed truck that shuts a highway for hours.

> *[Pause. Let this land.]*

This distinction changes everything. A high-severity accident in this data isn't necessarily the one with the worst injuries — it's the one that takes the longest to clear.

So when we ask 'does bad weather increase severity?', we're actually asking: *does bad weather lead to accidents that take longer to clear the road?*

And when we ask 'are rush hours more severe?', we're asking: *do rush hour crashes tie up traffic longer than crashes at other times?*

Keep this definition in your head for the next 25 minutes. Every single chart you see today is built on top of it. If you forget this, the results will look counterintuitive. If you remember it, they start to make a lot of sense.

> *[Transition]* "Now — before we trust any conclusions, we need to talk about whether we can even trust the data itself."

---

## Slide 3 — Data Integrity (~3 min)

"Any honest data analysis has to confront its own limitations. We identified six structural faults in this dataset, but two matter most for what you'll see today.

**First — California is over-represented.**

California alone accounts for about **1.39 million records** — more than Florida and Texas combined. At first glance, that looks like California has a crash epidemic. It doesn't. California simply has one of the densest traffic sensor networks in the country. We're not measuring where accidents *happen* most — we're measuring where accidents get *reported* most. That's a sampling bias we have to respect.

**Second — the data is severely imbalanced.**

Roughly **eighty percent of all accidents are Severity 2.** Severity 1 is almost nonexistent. Severity 3 and 4 together make up less than a quarter of records. This class imbalance means we can't just look at raw counts — we have to look at **proportions**, or *high-severity rates*, to see meaningful patterns.

To clean the data, we used a two-times IQR outlier removal on accident duration and distance — this removed physically implausible records like a **sixteen-hour road closure in Maine**, which was almost certainly a timestamp error, and an **eight-and-a-half-mile single-accident blockage in Wyoming**. We also dropped 2023 because we only had three months of data — it would have skewed every year-over-year comparison.

After cleaning: **6.16 million rows**. That's our analysis foundation.

> *[Pause]*

So — we know what severity means. We know the data's biases. We know we're working with proportions, not counts.

Now we can ask our first hypothesis. And I'll hand over to Aditya."

> *[Step aside. Aditya walks forward.]*

---

# 🎤 SPEAKER 2 — ADITYA (~7 min)

## Slide 4 — H1 Setup (~3 min)

"Thanks, Dhruv.

Here's the first question we wanted to answer — and be honest, every one of you has felt this:

> ***Does bad weather actually cause worse accidents?***

It feels intuitive. Rain reduces visibility. Snow reduces traction. Fog hides the road. It seems **obvious** that these conditions should produce longer, messier accidents that take longer to clear.

So that was our **Hypothesis 1**:

> *Adverse weather conditions cause higher accident severity compared to normal conditions.*

To test this, we had a problem. The raw `Weather_Condition` column in the dataset has **hundreds** of distinct strings — things like 'Light Drizzle / Windy', 'Heavy T-Storm', 'Mostly Cloudy', 'Blowing Snow'. You can't plot hundreds of categories meaningfully.

So we engineered a cleaner feature called **Weather_Group**, using keyword matching to collapse everything into seven clean categories:

- **Clear** — fair, sunny
- **Cloudy** — overcast, partly cloudy
- **Rain** — drizzle, showers, thunderstorms
- **Snow/Ice** — snow, sleet, blizzard
- **Fog/Haze** — fog, mist, haze, smoke
- **Wind/Dust** — high winds, dust, sand
- **Other** — anything unmatched

Then, for each weather group, we calculated the **high-severity rate** — the percentage of accidents in that weather that were Severity 3 or 4.

If the hypothesis is right, we should see adverse weather groups — Rain, Snow, Fog — dominating the top of the chart. Clear weather should be at the bottom.

> *[Pause for emphasis]*

That's not quite what we found. Let me show you."

---

## Slide 5 — H1 Results (~4 min)

> *[Point to the chart.]*

"Here's the ranked chart of high-severity rates by weather group, sorted from highest to lowest.

At the top — **Rain, at 24.38%.** That matches the intuition. Good.

In second place — **Cloudy, at 22.43%.** Also reasonable — cloudy conditions often come with reduced light.

In third — **Snow and Ice, at 21.45%.** Again, fits expectations.

But then watch what happens.

**Clear weather comes in at 17.85%.** That's fourth place. Already surprising — clear conditions account for a real share of high-severity accidents.

And here's the finding that genuinely stopped us in our tracks:

> ***Fog and Haze sits at 16.93% — LOWER than Clear weather.***

> *[Pause. Let the audience react.]*

Fog — the condition we associate with pile-ups, zero visibility, genuine danger — has a *lower* high-severity rate than driving on a perfectly clear day.

How is that even possible?

> *[Pause, then answer]*

Remember what Dhruv said. **Severity is disruption time, not injury severity.**

Fog accidents tend to happen on local roads, at lower speeds, with fewer vehicles involved. They get cleared quickly. A two-car fender-bender in fog resolves in thirty minutes.

But clear weather? Clear weather is highway weather. **High speed. Heavy traffic. Longer emergency response times. More vehicles involved when something goes wrong.** When an accident happens on a clear day on an open highway — the disruption is massive.

We also looked at a related angle — visibility distance — and the pattern confirmed itself. Visibility above 10 miles had the *highest* high-severity rate of any bin, at **31.65%.** High visibility is a proxy for wide, fast, highway roads.

So **what's our verdict on H1?**

**Partially supported.** Yes, Rain adds about 6.5 percentage points over Clear — that's real. But the story is much more nuanced than 'bad weather is dangerous'. The dataset is telling us that *road type and speed* matter more than *weather itself*.

> *[Transition]* "Alright — weather wasn't quite what we thought. Let's see if the clock is. Mahak, over to you."

> *[Step aside. Mahak walks forward.]*

---

# 🎤 SPEAKER 3 — MAHAK (~7 min)

## Slide 6 — H2 Setup (~3 min)

"Thank you, Aditya.

So if weather isn't the straightforward villain, maybe time of day is. Here's our second hypothesis, and it's probably even more intuitive than the first:

> ***Rush hours cause higher accident severity.***

This feels like common sense. The morning commute — **7 to 10 AM.** The evening commute — **4 to 8 PM.** Everybody on the road at once. Stop-and-go traffic. Distracted, tired drivers. Rear-endings. Cascading backups.

Of course these should be the most severe hours.

To test this, we broke every accident down by its **hour of the day**, from 0 to 23. We also cross-tabbed that against the **day of the week**, because weekend traffic patterns differ completely from weekday patterns.

For each hour — and for each hour-day combination — we calculated the high-severity rate. Same metric as before: what percent of accidents in that hour were Severity 3 or 4?

Then we plotted it two ways. First, as a line chart across the 24 hours. Second, as a heatmap — seven rows for days of the week, twenty-four columns for hours.

If the hypothesis holds, we should see two clear spikes — one around 7-9 AM, another around 5-7 PM. The line should peak at the commutes.

> *[Pause]*

The actual chart... does not do that. At all."

---

## Slide 7 — H2 Results (~4 min)

> *[Point to the line chart]*

"This is the high-severity rate plotted across all 24 hours. The dashed horizontal line is the daily average.

Notice where the highest points are.

**Hour 4 AM: 21.05%.**
**Hour 5 AM: 21.49%.**
**Hour 6 AM: 21.73%.**

That's a **sustained plateau** between 4 and 6 AM. Not a single spike — a full three-hour window where severity is structurally elevated.

Now look at 7 and 8 AM. The heart of morning rush hour.

**Hour 7: 18.83%.**
**Hour 8: 18.99%.**

> *[Pause]*

**They're below the daily average.** Rush hour — the time we all believed was the most dangerous — is actually one of the *calmer* windows when you measure severity rate.

> *[Move to heatmap]*

The heatmap confirms it even more dramatically. The single darkest cell — meaning the highest severity rate in the entire 24x7 grid — sits at **Saturday and Sunday, between 7 and 9 AM**, with a high-severity rate around **31.76%.**

Weekend mornings. Not weekday rush hours.

So what's happening?

The mechanism becomes clear once you think about it. At 4-6 AM, the roads are empty. **Empty roads mean high speeds.** A single crash on an empty highway means no one else around to call it in quickly, slower emergency response, and a collision at highway speeds that does serious damage and takes a long time to clear.

Weekend mornings follow the same logic. Low traffic, open roads, possibly fatigued drivers heading home from a late night — and when an accident happens, it's a big one.

Rush hour crashes, by contrast, are usually **low-speed fender-benders** in congestion. Lots of them, absolutely — but each one clears quickly. High count, low severity rate.

Now, there's one important nuance. When we look at **absolute volume** — just counts, not rates — rush hours *do* produce the most high-severity accidents in total. 7-8 AM and 4-5 PM each account for about 7.5% of daily accident volume. So if you're planning emergency dispatch resources, rush hours still matter enormously.

But if you're asking 'when is a given crash most likely to be severe?' — it's **not** rush hour.

**Verdict on H2: Partially supported.** True for volume, false for per-incident severity rate.

> *[Transition]* "So now we have a puzzle. Weather matters, but not how we thought. Rush hour matters, but not how we thought. The obvious answers both failed. So what *actually* drives severity? Falak is going to show you."

> *[Step aside. Falak walks forward.]*

---

# 🎤 SPEAKER 4 — FALAK (~8 min)

## Slide 8 — Key Insights: O1 & O2 (~4 min)

"Thank you, Mahak.

We came into this project expecting weather and time to be the main story. When they both turned out to be partial, we went back to the data looking for what we'd missed. Two findings emerged — and these are the insights we believe actually matter.

### Observation 1: Infrastructure beats weather.

> *[Point to O1 section]*

We looked at four road features in the dataset — **traffic signals, pedestrian crossings, stop signs, and junctions** — and asked a simple question: does the presence of this feature change severity?

The numbers are striking.

- **Traffic signals present: 9.51% high-severity. Absent: 21.19%.** That's an **eleven-point-seven percentage-point reduction**.
- **Pedestrian crossings present: 7.04%. Absent: 21.04%.** A **fourteen-point drop**.
- **Stop signs present: 6.22%. Absent: 19.84%.** Nearly **fourteen points** again.
- And the one exception — **junctions *increase* severity by about eight points**, because they're uncontrolled conflict points where vehicles have to yield without stopping.

Now compare that to our best weather finding. **Rain vs. Clear was only a 6.5-point difference.**

Infrastructure is roughly **1.8 times as powerful as weather** when it comes to reducing high-severity outcomes. It's not even close.

The mechanism is straightforward: controlled stops force lower speeds at conflict points. Lower speeds mean shorter collision chains. Shorter collisions mean less road disruption, less clearance time, lower severity. A signal doesn't prevent accidents — but it prevents the kind of accidents that shut down a road for hours.

### Observation 2: High severity happens in low traffic.

Everything we saw points to the same hidden pattern.

- Pre-dawn hours — peak severity.
- Weekend mornings — peak severity.
- Very high visibility (highway conditions) — peak severity at **31.65%**.

The common thread is **open roads and high speeds.** When roads are empty, speeds are high, and response times are slow, any accident that occurs becomes a bigger event. It takes longer to clear. It blocks more road.

Rush hour protects you. Congestion protects you. Low traffic — counterintuitively — is when the severe accidents happen.

> *[Pause]*

That's not what anyone would tell you at a first glance at a road safety problem. But the data is very clear about it."

---

## Slide 9 — The Verdict (~2 min)

"So if you came in asking, 'what matters most for traffic accident severity?', here's the ranking our analysis gives you.

**Number one — Infrastructure.** Signals, crossings, stop signs. Up to 14 percentage points of severity reduction. Nothing else in the dataset comes close.

**Number two — Weather.** Real, but modest. Rain adds about 6 percentage points over clear conditions. Worth planning for, not worth fixating on.

**Number three — Time of Day.** Mostly a volume driver. Per-accident severity rates don't vary as much across hours as volume does. The 4–6 AM plateau is real but narrow.

The one-sentence takeaway:

> ***Infrastructure investment has roughly 2.1 times the effect on severity reduction as weather.***

If you're a city planner — this matters. If you're a transportation department — this matters. You cannot change the weather. You cannot change the clock. You **can** change the junction."

---

## Slide 10 — Closing (~1 min)

> *[Slow down. Deliberate.]*

"Which brings us to our closing thought.

> ***Infrastructure over intervention.***

You cannot stop the rain. You cannot stop the commute. But you can add a signal. You can mark a crossing. You can install a stop sign.

The weather and the clock will always be factors.

**The junction is in your control.**"

---

## Slide 11 — Thank You & Q&A (~1 min)

"Thank you so much for listening. On behalf of Dhruv, Aditya, Mahak, and myself —

We'd love to take your questions."

---

# 📋 Q&A PREP — ANTICIPATED QUESTIONS

| Q | Short Answer |
|---|--------------|
| *"Doesn't California bias all your conclusions?"* | State-level counts are biased, yes — but our ratio-based analyses (severity rate, weather groups, hour patterns) are largely stable when stratified by state. We call this out as a limitation in the report. |
| *"How do you know it's not reverse causation? Maybe signals are installed where accidents are minor."* | Fair point. Signals tend to be installed at high-traffic junctions — if anything, that biases *against* our finding. The fact that we still see huge reductions strengthens the case. |
| *"What about exposure? More people drive at rush hour."* | Correct — this is Fault 4 in our report. We can't normalize by vehicle-miles-traveled, which is why we distinguish between *rate* and *volume* throughout the analysis. |
| *"Why did you exclude 2023?"* | Only three months of 2023 data were in the dataset when we obtained it. A partial year creates artificial temporal skew in any year-over-year analysis. |
| *"Severity declined from 34% in 2016 to 3% in 2022 — is the country getting safer?"* | Almost certainly no. This is Fault 5 — data drift. The dataset expanded its API coverage dramatically, adding lots of Severity 2 records and diluting the high-severity proportion. It's a measurement artifact, not a safety improvement. |
| *"What would you add next?"* | Speed limit data per road segment, and a logistic regression model that can separate interaction effects across weather, time, road feature, and visibility simultaneously. |

---

# ⏱️ TIMING GUIDE (Rehearsal Checklist)

- **0:00–7:00** — Dhruv (setup, severity, data trust)
- **7:00–14:00** — Aditya (H1 weather)
- **14:00–21:00** — Mahak (H2 time)
- **21:00–29:00** — Falak (insights, verdict, close)
- **29:00–30:00+** — Q&A buffer

**Rehearsal tips:**
1. **Practice the handoffs.** "Over to Aditya" / "Mahak, take it away" — these should feel confident, not awkward.
2. **Pause on the surprises.** Slide 2 (severity ≠ injury), Slide 5 (Fog < Clear), Slide 7 (rush hour below average) — these are your key "aha" moments. Let silence do the work.
3. **Trust the data points.** Don't dilute specific numbers into vague claims. "11.7 percentage points" lands harder than "a big reduction."
4. **Keep gestures minimal but intentional.** Point at the chart once when you introduce it, then return your focus to the audience.
5. **Time yourselves individually.** Each speaker should hit ~7 min. If anyone runs long, they eat into Q&A.
