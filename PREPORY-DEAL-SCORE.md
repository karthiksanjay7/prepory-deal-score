# Prepory Deal Score — How It Works

The Prepory Deal Score is a **Combined Score** in HubSpot that rates every deal on two dimensions: how well the lead **fits** our ideal customer profile, and how much they're **engaging** with us. Each dimension scores out of 100, combining to a max of 200.

---

## The Three Scores

### Fit Score (property-based) | Max: 100
> "How good is this lead on paper?"

Points come from **deal and contact properties** — things we know about the lead before they do anything. A deal picks exactly ONE lead channel and ONE product tier, so the formula is designed around that constraint.

**Formula**: Lead Channel (30%) + Product Tier (40%) + Lead Signals (20%) + Priority (10%) = 100 max

### Engagement Score (event-based) | Max: 100
> "How active is this lead?"

Points come from **actions the contact takes** — opening emails, clicking links, visiting pages. All events use "Score every time" so points stack with each occurrence. Per-event values are deliberately low because they accumulate.

### Total Score (combined) | Max: 200
> Fit + Engagement = Total

This is the number you see on the deal record as **Prepory Deal Score**. Higher = hotter deal.

---

## What Earns Points

### FIT SCORE — Property Groups

#### 1. Lead Channel & Source (30 pts max)
Where the lead came from. Scored on the associated contact's **Lead Channel** property. A deal gets exactly one of these.

| Lead Channel | Points | % of Max |
|---|---|---|
| Forbes | +30 | 30% |
| Referral - WOM | +25 | 25% |
| Webinar | +22 | 22% |
| Paid Social | +17 | 17% |
| AI Lead | +12 | 12% |
| Organic Search and Social | +7 | 7% |
| Other | +3 | 3% |

#### 2. Product Tier (40 pts max)
Higher-priced programs = higher priority. Scored on the deal's **Product** property. Each tier is 8 points apart — a consistent step size so the gap between tiers is always proportional.

| Tier | Programs | Points | % of Max |
|---|---|---|---|
| Tier 5 | Synthesis, Marathon US, Marathon UK, Finish Line US, Finish Line UK | +40 | 40% |
| Tier 4 | Momentum, Headstart US, Headstart UK | +32 | 32% |
| Tier 3 | Homestretch, Fast Track US, Fast Track UK | +24 | 24% |
| Tier 2 | Launchpad, A La Carte Career Coaching, A La Carte College Coaching | +16 | 16% |
| Tier 1 | Spark, Ignite | +8 | 8% |

#### 3. Lead Signals (20 pts max)
Behavioral signals that indicate buying intent.

| Signal | Condition | Points |
|---|---|---|
| Re-engaged After Inactivity | Property = "Yes" (set by workflow after 10+ days of no activity then re-engagement) | +15 |
| Competitor Awareness | First conversation = "No, we've doing our research" | +5 |

#### 4. Priority (10 pts max)
Manual override for enrollment officers to flag high-priority deals.

| Signal | Condition | Points |
|---|---|---|
| Priority Deal | Deal Priority = "High" | +10 |

---

### ENGAGEMENT SCORE — Event Groups

All engagement events use **"Score every time"** — meaning points stack with each occurrence. Values are deliberately low because a single contact may trigger dozens of events over a 2–4 week deal cycle.

**Event hierarchy**: Sales open (7) > Sales click (5) > Marketing click (4) > Page visit (3) > Marketing open (2)

#### Sales Emails 1:1 — strongest signal
Personal emails sent by enrollment officers from HubSpot (post-IC follow-ups, profile plans, etc.)

| Event | Points |
|---|---|
| Sales email open | +7 |
| Sales email click | +5 |

> Sales email open is worth the most because when a contact opens the enrollment officer's personal email, it means they're actively reading communication from the person who will close the deal. The open itself proves they're engaged with the EO relationship.

#### Marketing Emails — medium signal
Bulk campaigns, newsletters, and automated workflow emails.

| Event | Points |
|---|---|
| Clicked link in email | +4 |
| Opened email | +2 |

#### Page Visits

| Event | Points |
|---|---|
| Page visited | +3 |

---

## Thresholds (A1 to C3 Grid)

The threshold takes the fit score and engagement score and assigns a **tier label** like A1, B2, or C3.

### How to read the tier:
- **Letter** (A/B/C) = Fit level (A = best fit, C = lowest fit)
- **Number** (1/2/3) = Engagement level (1 = most engaged, 3 = least engaged)

### Threshold settings:

| Tier | Fit Score Range | Engagement Score Range |
|---|---|---|
| A / 1 | 50 to 100 | 50 to 100 |
| B / 2 | 25 to 49 | 25 to 49 |
| C / 3 | 0 to 24 | 0 to 24 |

> **Why 50/25 instead of 75/50**: With thresholds at 75, a Forbes + Tier 5 deal (our best possible lead) scored only 70 — that's a B tier. The new thresholds are calibrated to the actual scoring range: 9 out of 35 channel×tier combinations naturally land above 50, making A tier meaningful but achievable.

### The Grid:

```
              Level of Engagement
              3 (low)    2 (med)    1 (high)
Fit A (best)  [  A3  ]   [  A2  ]   [  A1  ]  <-- hottest
Fit B (mid)   [  B3  ]   [  B2  ]   [  B1  ]
Fit C (low)   [  C3  ]   [  C2  ]   [  C1  ]  <-- coldest
```

| Tier | What it means | Action |
|---|---|---|
| **A1** | Best fit + highest engagement | Call them NOW |
| **A2** | Best fit + medium engagement | Prioritize follow-up |
| **A3** | Best fit + quiet | Needs a nudge |
| **B1** | Decent fit + very engaged | Worth pursuing |
| **B2** | Decent fit + medium engagement | Nurture |
| **C3** | Low fit + low engagement | Low priority |

---

## HubSpot Properties Created

| Property | Internal Name | What It Shows |
|---|---|---|
| Prepory Deal Score | `prepory_deal_score` | Combined total (fit + engagement) |
| Prepory Deal Score fit | `prepory_deal_score_fit` | Fit score only |
| Prepory Deal Score engagement | `prepory_deal_score_engagement` | Engagement score only |
| Prepory Deal Score threshold | `prepory_deal_score_threshold` | Tier label (A1, B2, C3, etc.) |

---

## Supporting Workflows

Three workflows power the Lead Signals scoring:

1. **WF1 — Auto-Populate Lead Channel**: Sets the Lead Channel property on deals based on the contact's original source. Must be ON for the Lead Channel & Source scoring to work.

2. **WF2 — Re-engagement Tracker**: Monitors deals for 10+ days of inactivity, then flags `Re-engaged After Inactivity = Yes` when the contact re-engages. Powers the +15 re-engagement bonus.

3. **WF3 — Auto-Populate First Conversation**: Sets the First Conversation property on deals based on the contact's initial response. Must be ON for the Competitor Awareness +5 signal to work.

---

## Quick Reference

**Max possible fit score**: 100 points (Forbes +30, Tier 5 +40, Re-engaged +15, Competitor +5, Priority +10)

**Engagement score**: Uncapped (stacks every time) but limited to 100 by HubSpot's score limit

**Score is ON** and recalculates automatically whenever properties change or new events occur.
