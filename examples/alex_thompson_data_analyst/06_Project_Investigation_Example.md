# Project Investigation: Customer Churn Analysis

**Project Code:** CHURN-2026-Q1  
**Analyst:** Alex Thompson  
**Duration:** January 15 - February 28, 2026 (6 weeks)  
**Status:** ✅ Complete (Delivered February 25, 2026)  
**Last Updated:** February 28, 2026

---

## 🎯 Executive Summary

**Problem:** Customer churn rate increased from 5% to 8% in Q4 2025. Leadership needs to understand why and what actions to take.

**Approach:** Analyzed 3 years of customer data (12,000 customers), segmented by behavior patterns, identified churn predictors.

**Key Findings:**
1. 68% of churned customers had unresolved support tickets in their final 60 days
2. Price sensitivity wasn't the issue—98% of churned customers were on standard pricing
3. Customers with <2 product integrations were 3x more likely to churn
4. Onboarding quality was strongest predictor (customers who completed setup guide had 90% retention)

**Recommended Actions:**
1. **Immediate (Week 1):** Prioritize support tickets from customers with 1-2 active products
2. **Short-term (30 days):** Implement proactive outreach for customers with unresolved tickets >14 days
3. **Medium-term (90 days):** Redesign onboarding process to ensure setup guide completion
4. **Long-term (6 months):** Build early warning dashboard to flag at-risk customers

**Impact:** If recommendations implemented, projected churn reduction from 8% → 5.5% (baseline + 0.5% improvement), retaining ~$400K annual revenue.

---

## 📋 Project Overview

### Background & Context

**What triggered this:**
- Q4 2025 board meeting: CEO expressed concern about rising churn
- VP Product requested data-driven analysis to inform retention strategy
- Sales team reported "customers seem less engaged lately" (anecdotal)

**Stakeholders:**
- **Primary:** VP Product (project sponsor)
- **Secondary:** VP Customer Success, Director of Sales, CFO
- **Informed:** Customer Success team (15 people), Product Managers (3)

**Business Context:**
- MidTech's SaaS platform is primary revenue source ($8M ARR)
- Customer acquisition costs have increased 40% year-over-year
- Retention is now more critical than ever for profitability
- Q1 2026 OKR: "Reduce churn to <6% by end of quarter"

### Success Criteria (Defined at Project Start)

**Must Have:**
1. Identify top 3 drivers of churn with statistical support
2. Segment customer base by churn risk
3. Deliver actionable recommendations (not just analysis)
4. Present findings to VP level with clear narrative

**Nice to Have:**
1. Predictive model to score churn risk (stretch goal)
2. Dashboard for ongoing monitoring
3. ROI estimates for recommendations

**Out of Scope:**
1. Competitive analysis (separate project)
2. Product feature gap analysis (Product team owns this)
3. Pricing strategy review (Finance team owns this)

---

## 🔍 Investigation Methodology

### Phase 1: Data Collection & Understanding (Week 1)

**Data Sources Identified:**
1. **Customer Database** (Snowflake)
   - Customer profile data (company size, industry, join date, churn date)
   - Usage metrics (logins, feature usage, integration count)
   - 12,000 total customers, 960 churned in last 3 years

2. **Support Ticketing System** (Zendesk)
   - Ticket volume per customer
   - Resolution times
   - Ticket types and severity

3. **Billing System** (Stripe via API)
   - Subscription tier
   - Payment history
   - Pricing changes

4. **Product Analytics** (Mixpanel)
   - Onboarding completion rates
   - Feature adoption metrics
   - User engagement scores

**Data Quality Assessment:**
- ✅ Customer churn dates: 100% populated (reliable)
- ✅ Usage metrics: 95% complete (acceptable)
- ⚠️ Support ticket linkage: 85% matched to customers (some tickets not linked—addressed in data cleaning)
- ⚠️ Industry data: 60% populated (used where available, noted limitation)

**Initial Hypotheses (to test):**
1. Price-sensitive customers churn more
2. Low engagement customers churn more
3. Poor support experience drives churn
4. Customers without integrations churn more

---

### Phase 2: Exploratory Analysis (Week 2)

**Approach:** Descriptive statistics, segmentation, pattern identification

**Key Analyses:**

**1. Churn Rate Over Time**
```sql
-- Calculated monthly churn rate for 36 months
SELECT 
    DATE_TRUNC('month', churn_date) AS month,
    COUNT(*) AS churned_customers,
    (SELECT COUNT(*) FROM customers WHERE status = 'active' 
     AND created_at <= month) AS active_at_start,
    (COUNT(*) * 100.0 / active_at_start) AS churn_rate_pct
FROM customers
WHERE churn_date IS NOT NULL
AND churn_date >= '2023-01-01'
GROUP BY month
ORDER BY month;
```

**Finding:** Steady 5% baseline, spiked to 8% in Q4 2025 (confirmed the concern)

**2. Customer Segmentation by Churn**
- Segment A: Active users (10,200 customers, 92% retention)
- Segment B: Churned users (960 customers, 8% churn)
- Segment C: At-risk users (840 customers, low engagement patterns)

**3. Feature Usage comparison: Active vs Churned**
- Churned customers averaged 12 logins/month (Active: 45 logins/month)
- Churned customers used an average of 1.3 features (Active: 4.7 features)
- **Key insight:** Engagement gap is massive

---

### Phase 3: Deep-Dive Analysis (Weeks 3-4)

**Testing Hypotheses:**

**Hypothesis 1: Price-sensitive customers churn more**
- **Finding:** ❌ REJECTED
- 98% of churned customers were on standard pricing
- Only 2% had negotiated discounts or were price-escalated
- **Conclusion:** Price isn't the driver

**Hypothesis 2: Low engagement customers churn more**
- **Finding:** ✅ CONFIRMED
- Churned customers had 73% lower login frequency
- 68% of churned customers never completed onboarding checklist
- Engagement metrics strongly predict churn
- **Conclusion:** Engagement is critical

**Hypothesis 3: Poor support experience drives churn**
- **Finding:** ✅ CONFIRMED (STRONGEST SIGNAL)
- 68% of churned customers had unresolved support tickets in final 60 days
- Average ticket resolution time for churned customers: 8.2 days vs 3.1 days for retained  
- Churned customers had 2.3x more support tickets than average
- **Conclusion:** Support is a major factor

**Hypothesis 4: Customers without integrations churn more**
- **Finding:** ✅ CONFIRMED
- 0 integrations: 18% churn rate
- 1 integration: 12% churn rate
- 2+ integrations: 4% churn rate
- **Conclusion:** Integration depth = stickiness

---

### Phase 4: Root Cause Deep Dive (Week 5)

**Question: WHY do customers have unresolved tickets?**

**Analysis:**
- Pulled all support tickets for churned customers (2,800 tickets)
- Categorized by type: Technical (45%), Billing (15%), Feature Request (25%), How-To (15%)
- Cross-checked ticket severity and resolution SLA compliance

**Findings:**
1. Technical tickets took 2.5x longer to resolve for churned customers
2. 40% of churned customer tickets were escalated (vs 12% for retained customers)
3. Many tickets were never properly closed (customer stopped responding)
4. Support team was unaware these customers were at-risk

**Question: WHY do customers fail onboarding?**

**Analysis:**
- Onboarding has 8-step checklist
- Average completion: 6.2 steps for retained customers, 2.8 for churned
- Identified drop-off points: Steps 4-5 (integration setup)

**Findings:**
1. Integration setup is too complex (technical documentation poor)
2. No proactive outreach if customer stalls on onboarding
3. Customers who complete onboarding have 90% retention (vs 60% overall)
4. **Onboarding completion is THE strongest predictor of retention**

---

### Phase 5: Recommendations Development (Week 6)

**Criteria for recommendations:**
- Evidence-based (from analysis)
- Actionable (team can implement)
- Prioritized (quick wins vs long-term)
- Measurable (can track impact)

**Recommendation Framework:**

**Tier 1: Immediate Actions (Week 1)**
1. **Prioritize support for at-risk customers**
   - Flag customers with: <2 integrations AND open ticket >14 days
   - Customer Success team prioritizes these 87 customers
   - Target: Resolve all tickets within 7 days
   - **Expected impact:** Prevent 5-10 imminent churns

**Tier 2: Short-term (30 days)**
2. **Proactive support outreach protocol**
   - Automated alert when ticket exceeds 14 days unresolved
   - Customer Success manager personally reaches out
   - Offer dedicated support session if needed
   - **Expected impact:** 30% reduction in unresolved ticket churn

3. **Integration support enhancement**
   - Create video tutorials for top 3 integrations (Slack, Salesforce, QuickBooks)
   - Offer live setup assistance (1-hour sessions)
   - Simplify integration documentation
   - **Expected impact:** Increase 2+ integration rate from 45% to 60%

**Tier 3: Medium-term (90 days)**
4. **Onboarding redesign**
   - Reduce checklist from 8 steps to 5 (focus on essentials)
   - Automated email sequence if customer stalls (Day 3, Day 7, Day 14)
   - Flag incomplete onboardings for Customer Success team
   - **Goal:** 80%+ onboarding completion (up from 55%)

5. **Early warning dashboard**
   - Real-time view of at-risk customers (engagement score)
   - Combine: login frequency, ticket status, integration count, onboarding completion
   - Weekly review by Customer Success leadership
   - **Expected impact:** Proactive intervention prevents 20-30 churns/quarter

---

## 📊 Detailed Findings

### Finding 1: Support Experience Correlation

**Data:**
- Churned customers with unresolved tickets: 653 of 960 (68%)
- Retained customers with unresolved tickets: 840 of 10,200 (8%)
- **Statistical significance:** p < 0.001 (chi-square test)

**What this means:**
Unresolved support tickets are the strongest red flag for churn. This isn't just correlation—when we look at timeline, tickets go unresolved in the 30-60 days BEFORE churn decision, suggesting causal relationship.

**Why it matters:**
We can intervene. Support is a controllable factor. Improving ticket resolution times and proactive outreach can materially reduce churn.

---

### Finding 2: Onboarding Quality Predicts Retention

**Data:**
- Completed onboarding (8/8 steps): 90% retention
- Partial onboarding (4-7 steps): 75% retention
- Minimal onboarding (<4 steps): 40% retention

**Onboarding Completion Rates:**
- Step 1-3: 95% completion (easy setup)
- Step 4-5: 62% completion (integration setup—DROP-OFF HERE)
- Step 6-8: 55% completion (advanced features)

**What this means:**
First 30 days determine long-term outcome. Customers who don't complete setup never get full value from product and churn predictably.

**Why it matters:**
Fixing onboarding has Highest ROI. Small investment in better integration docs + proactive outreach could shift 60% retention to 80%+.

---

### Finding 3: Integration Depth = Retention

**Data:**

| Integrations | Customer Count | Churn Rate | Retention |
|--------------|----------------|------------|-----------|
| 0 | 2,400 | 18% | 82% |
| 1 | 4,800 | 12% | 88% |
| 2 | 3,000 | 6% | 94% |
| 3+ | 1,800 | 2 % | 98% |

**Statistical Note:** Correlation coefficient: -0.72 (strong negative correlation between integrations and churn)

**What this means:**
Every additional integration dramatically improves retention. Customers with 3+ integrations almost never churn (2%). The product becomes embedded in their workflow.

**Why it matters:**
We should aggressively help customers set up integrations. Even getting 0→1 or 1→2 has massive impact.

---

### Finding 4: Price Is NOT The Issue

**Data:**
- Standard pricing customers: 98% of churned (940 of 960)
- Discounted/custom pricing: 2% of churned (20 of 960)
- No correlation between contract value and churn

**What this means:**
The narrative "we're losing customers due to pricing" is wrong. Value delivery is the issue, not price.

---

## 💡 Insights & Patterns

### Pattern 1: "Death Spiral" Customer Journey

**Observed pattern for churned customers:**
1. Sign up enthusiastically
2. Struggle with integration setup (Week 2-3)
3. Open support ticket
4. Wait 10-14 days for resolution
5. Give up, stop using product
6. Eventually cancel (Month 3-4)

**Intervention Point:** Week 2-3 when they stall on integration. If we proactively help then, we prevent spiral.

---

### Pattern 2: "Happy Path" Customer Journey

**Observed pattern for retained customers:**
1. Sign up
2. Complete all onboarding steps (Week 1-2)
3. Set up 2+ integrations
4. Product becomes daily habit
5. Few support tickets (because setup was smooth)
6. High engagement, referrals, upsell opportunities

**Replication strategy:** Make "Happy Path" the DEFAULT PATH. Better docs, proactive support, simplified onboarding.

---

### Pattern 3: At-Risk Customer Signals (Early Warning)

**Combination of signals predicts churn:**
- ⚠️ <10 logins in last 30 days
- ⚠️ Open support ticket >14 days
- ⚠️ <2 integrations connected
- ⚠️ Onboarding incomplete after 30 days

**Predictive Power:**
- 0 signals: 3% churn risk
- 1 signal: 8% churn risk  
- 2 signals: 22% churn risk
- 3+ signals: 65% churn risk

**Current state:** 87 customers meet 3+ criteria (HIGH RISK—need immediate attention)

---

## ✅ Recommendations (Detailed)

### Recommendation 1: Emergency Intervention (87 High-Risk Customers)

**What:** Immediate outreach to 87 customers meeting 3+ at-risk signals

**How:**
1. Customer Success team divides list (3-4 customers each)
2. Personal phone call or video meeting (not just email)
3. Understand their pain points, offer dedicated support
4. Resolve open tickets immediately (escalate if needed)
5. Walk through integration setup if that's the blocker

**Timeline:** This week (before end of February)

**Success Metric:** Retain 70%+ of these 87 customers (vs expected 35% if we do nothing)

**Effort:** ~40 hours total team effort (distributed)

**Expected ROI:** Save ~$45K in annual revenue

---

### Recommendation 2: Support Ticket SLA and Accountability

**What:** Tighten support resolution SLA, especially for customers with <2 integrations

**How:**
1. New SLA: 48 hours for initial response, 5 days for resolution (down from 7 days)
2. Automated escalation if ticket hits Day 10 unresolved
3. Weekly support ticket review: Flag any customer at risk
4. Customer Success has visibility into support tickets (currently siloed)

**Timeline:** Implement in March, pilot for 30 days

**Success Metric:** 
- Reduce average resolution time from 6.8 days to 4.5 days
- Reduce % of unresolved tickets (>14 days) from 18% to <8%

**Effort:** Process change + tool configuration (~20 hours)

**Expected ROI:** Prevent 15-20 churns per quarter ($180K-$240K annual revenue saved)

---

### Recommendation 3: Integration Success Program

**What:** Proactive program to help customers set up 2+ integrations

**Components:**
1. **Improved Documentation**
   - Video tutorials for top 3 integrations (Slack, Salesforce, QuickBooks)
   - Step-by-step with screenshots
   - Estimated effort: 30 hours (outsource video creation?)

2. **Live Setup Assistance**
   - Offer 30-minute integration setup session (Customer Success hosts)
   - Available upon request or proactively offered if customer stalls
   - Estimated: 2 sessions/week, 1 hour total per week

3. **Integration Milestones**
   - Email automation: "You've set up 1 integration! Here's how to add integration #2..."
   - Gamification? ("90% of successful customers use 2+ integrations")

**Timeline:** Pilot in Q2 2026

**Success Metric:** Increase % of customers with 2+ integrations from 45% to 65%

**Expected ROI:** If we shift 200 customers from 1 integration to 2+ integration retention levels, save $320K annual revenue

---

### Recommendation 4: Onboarding Redesign

**What:** Simplify onboarding to maximize completion rate

**Proposed Changes:**
1. **Reduce steps:** 8 → 5 (combine or remove non-essential)
2. **Focus on integrations:** Make integration setup Step 2 (not Step 4-5)
3. **Automated nudges:** Email if no activity for 3 days, 7 days, 14 days
4. **Success Manager assignment:** Customers who don't complete in 14 days get personal outreach

**Timeline:** Design in Q2, implement in Q3

**Success Metric:** Onboarding completion rate from 55% to 80%+

**Expected ROI:** If we improve retention for 300 customers (from 60% to 80%), save $640K annual revenue

---

### Recommendation 5: Early Warning Dashboard

**What:** Real-time dashboard showing at-risk customers

**Included Metrics:**
- Engagement score (login frequency, feature usage)
- Support ticket status (open >14 days flagged)
- Integration count
- Onboarding completion %
- Overall risk score (weighted combination)

**Users:** Customer Success team, VP Product, VP Customer Success

**Refresh:** Daily

**Timeline:** Build in Q2 2026 (estimated 40 hours data + Tableau work)

**Success Metric:** 
- 100% of high-risk customers identified within 7 days of risk emerging
- Proactive outreach to 80%+ within 14 days
- Reduce churn from at-risk segment by 30%

**Expected ROI:** Ongoing churn prevention, estimated 30-50 churns prevented annually ($360K-$600K)

---

## 📈 Impact projection

### Scenario Analysis: If All Recommendations Implemented

**Baseline (No Action):**
- Q2 2026 projected churn: 8% (continuation of Q4 2025 trend)
- 800 churns annually from 10,000 customer base
- Lost revenue: $1.6M annually (assuming $2K average customer value)

**Conservative Scenario (Partial Success):**
- Churn rate: 8% → 6.5%
- 650 churns annually (150 fewer)
- Revenue saved: $300K annually
- **ROI:** ~10x (implementation cost ~$30K in labor)

**Optimistic Scenario (Full Success):**
- Churn rate: 8% → 5.5% (back to baseline + improvement)
- 550 churns annually (250 fewer)
- Revenue saved: $500K annually
- **ROI:** ~15x

**Timeframe for Impact:**
- Immediate actions: Results visible in March
- Full program impact: 6-9 months to reach steady state

---

## 🚧 Limitations & Caveats

### Data Limitations

**Issue 1: Industry Data Sparsity**
- Only 60% of customers have industry classification
- Couldn't fully analyze industry-based patterns
- **Mitigation:** Identified pattern among customers with data; warrants follow-up analysis if we improve data capture

**Issue 2: Support Ticket Matching**
- 15% of tickets couldn't be matched to customer records
- Possible undercounting of support issues
- **Mitigation:** Manual review confirmed majority were spam/non-customer tickets

**Issue 3: Competitive Intel**
- Don't know which competitor churned customers moved to (if any)
- **Mitigation:** Out of scope for this analysis; recommended exit survey implementation

---

### Analytical Limitations

**Correlation vs Causation:**
- Analysis shows strong correlations (unresolved tickets, low engagement)
- Can't definitively prove causation without experiment
- **Mitigation:** Timeline analysis + business logic strongly suggest causal relationships; recommendations are low-risk even if wrong

**Sample Completeness:**
- Analyzed 3 years of data (12,000 customers, 960 churned)
- Recent churn spike in Q4 2025 might have unique characteristics
- **Mitigation:** 3-year trend gives confidence; will monitor Q1-Q2 2026 data for consistency

---

## 📝 Lessons Learned

### What Went Well

**1. Stakeholder Alignment Early**
- Defined success criteria with VP Product in Week 1
- Avoided scope creep by being clear about "out of scope"
- Regular check-ins every Friday prevented surprises

**2. Iterative Analysis**
- Shared preliminary findings in Week 3 (before final)
- Got feedback that shaped final recommendations
- VP Customer Success contributed ideas for intervention programs

**3. Actionable Focus**
- Resisted urge to build complex predictive model (nice-to-have)
- Focused on "what should we DO?" not just "what is happening?"
- Recommendations are concrete, not vague ("improve customer success")

---

### What I'd Do Differently

**1. Start With Qualitative Research**
- Would have interviewed 5-10 churned customers first
- Might have surfaced issues faster than data analysis alone
- **For next time:** Always combine quantitative + qualitative

**2. Build Dashboard Earlier**
- Waited until Week 5 to build visualization dashboard
- Would have helped stakeholders follow progress if built in Week 2
- **For next time:** Prototype dashboard early for buy-in and iteration

**3. Document Assumptions More**
- Made some analytical decisions without explicitly documenting why
- Caused minor confusion when presenting
- **For next time:** Keep running "Decision Log" in project file

---

## 🔄 Next Steps & Follow-Up

### Immediate (This Week)
- [x] Present findings to VP Product (Feb 25 - COMPLETE)
- [x] Share deck with VP Customer Success and Director Sales
- [ ] Customer Success team meeting to discuss 87 high-risk customers

### Short-Term (March 2026)
- [ ] Launch emergency intervention for 87 high-risk customers
- [ ] Implement new support ticket SLA
- [ ] Begin integration documentation improvements

### Medium-Term (Q2 2026)
- [ ] Design and pilot onboarding redesign
- [ ] Build early warning dashboard (I'll own this)
- [ ] Monthly churn review meetings (track if interventions working)

### Measurement Plan
- **Weekly:** Track high-risk customer interventions
- **Monthly:** Calculate churn rate, compare to 8% baseline
- **Quarterly:** Full retrospective on recommendations impact

---

## 📚 Artifacts & Deliverables

**Created During Project:**
1. **Analysis SQL Scripts** (25 queries documented in my Reference Library)
2. **Tableau Dashboard** (Churn Analysis—shared with stakeholders)
3. **Presentation Deck** (35 slides, delivered to VP Product Feb 25)
4. **Recommendation Summary** (1-page exec summary for CEO)
5. **This Investigation Document** (methodology documentation for future projects)

**Shared With:**
- VP Product, VP Customer Success, Director Sales, CFO (presentation)
- Customer Success team (recommendations + at-risk customer list)
- Data team (dashboard published to shared workspace)

---

## 💬 Stakeholder Feedback

### VP Product (Project Sponsor)
> "This is exactly what we needed—clear findings, actionable recommendations, and compelling data story. The 87 high-risk customer list is our immediate action plan. Great work, Alex."

### VP Customer Success
> "I wish I had this data 6 months ago. The support ticket insight is eye-opening—we've been flying blind. Let's implement these recommendations immediately."

### Manager (Alex's Boss)
> "Your best work to date. You owned this end-to-end, communicated effectively with executives, and delivered strategic value. This is Senior Analyst caliber work."

---

**Project Status:** ✅ COMPLETE (Delivered Feb 25, recommendations accepted, implementation in progress)

**Personal Reflection:** This project was a career milestone. First time leading a strategic, cross-functional analysis with executive visibility. Learned huge amount about stakeholder management, data storytelling, and translating analysis into action. Proud of the impact this will have on the business.

**Reusable Methodology:** This investigation template worked brilliantly. Using it for future projects.

---

**Related Files:**
- [01_Career_Master_Alex_Thompson.md](01_Career_Master_Alex_Thompson.md) - This project documented for promotion case
- [02_Skills_Inventory_Alex_Thompson.md](02_Skills_Inventory_Alex_Thompson.md) - Skills applied: SQL, Tableau, stakeholder management
- [00_Hub_Index_Simplified.md](00_Hub_Index_Simplified.md) - Navigation
