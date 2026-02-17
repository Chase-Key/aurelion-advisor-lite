# Project Investigation Template: Reusable Framework

**Purpose:** Systematic methodology for complex investigations, applicable to anomalies, data issues, and multi-phase research projects.  
**Application:** Technical troubleshooting, research projects, data analysis, process optimization  
**Last Updated:** February 16, 2026  

---

## 📋 Cross-References

**Related Documentation:**
- **Strategic Planning:** See Hub Index for navigation to related templates
- **Query Libraries:** See Reference Library for query templates applicable to investigation phases
- **Terminology:** Maintain a Glossary for project-specific terms and definitions

---

## Template Structure (4 Phases)

Use this framework for all future investigations. Phases are sequential but can overlap.

---

## Phase 1: Problem Definition & Context

### **Objective**
Clear, concise statement of what you're investigating and why.

### **Template**

```
Investigation Title: [What's the issue?]
Timeline: [When did it start? Current status?]
Collaboration: [Who else is involved?]
Status: [Discovery / Investigation / Resolution / Complete]

## Problem Statement
- **What:** [Describe the issue/anomaly in 1-2 sentences]
- **When:** [Date/timeframe discovered]
- **Impact:** [Critical? Performance issue? Data quality? Business consequence?]

## Context & Scope
- **Known Facts:** [What's certain based on current knowledge?]
- **Scope:** [What's in scope? What's explicitly out of scope?]
- **Constraints:** [Time, resource, or technical limitations?]

## Hypothesis (Initial)
- [What do you think is causing this?]
- [Why is this plausible?]
```

---

## Phase 2: Investigation Steps & Findings

### **Objective**
Systematic exploration of hypothesis; iterative refinement of understanding.

### **Template**

```
## Investigation Progress

### Step 1: [Action]
- **What:** [What did you do?]
- **Data Examined:** [What sources/systems did you check?]
- **Finding:** [What did you discover?]
- **Next Question:** [What does this raise?]

### Step 2: [Action]
- **What:** [Next investigative step]
- **Data Examined:** [Sources checked]
- **Finding:** [Result]
- **Updated Hypothesis:** [How does this change your thinking?]

### Step 3: [Action]
- [Continue pattern]

## Key Insights from Investigation
- [What does the pattern tell you?]
- [Where does evidence point?]
- [What's still uncertain?]

## Remaining Unknowns
- [What questions remain?]
- [What can't you investigate at your level?]
- [What requires escalation?]
```

---

## Phase 3: Phased Investigation Plan (For Complex Issues)

### **Objective**
Structured roadmap for extended investigations; clear phases and deliverables.

### **Template**

```
## Multi-Phase Investigation Plan

### Phase 1: [Name - e.g., "Baseline Assessment"]
- **Objective:** [What's this phase trying to determine?]
- **Steps:** 
  1. [Specific action]
  2. [Specific action]
  3. [Specific action]
- **Deliverable:** [What will you have when this phase completes?]
- **Success Criteria:** [How do you know phase 1 is done?]

### Phase 2: [Name - e.g., "Root Cause Identification"]
- **Objective:** [Moving deeper into investigation]
- **Depends On:** [Findings from Phase 1]
- **Steps:** [Actions in sequence]
- **Deliverable:** [Output/findings]

### Phase 3: [Name]
- [Continue pattern]

### Phase 4: [Name - e.g., "Validation & Documentation"]
- **Objective:** [Confirm findings; prepare for handoff or implementation]
- **Deliverable:** [Final report/documentation]
```

---

## Phase 4: Action Items & Deliverables

### **Objective**
Specific, actionable next steps with owner assignments and success criteria.

### **Template**

```
## Action Items for [Team/Person]

### Priority 1: [High-impact action]
- **Action:** [What needs to happen?]
- **Owner:** [Who's responsible?]
- **Timeline:** [By when?]
- **Success Criteria:** [How do you know it's done?]

### Priority 2: [Action]
- [Same structure]

### Priority 3: [Action]
- [Same structure]

## Deliverables for Handoff

- [ ] [Document type] – [Description] – Owner: [Team]
- [ ] [Document type] – [Description] – Owner: [Team]
- [ ] [Query/Tool] – [Description] – Owner: [Team]

## Communication Plan
- **Interim Updates:** [Who needs to know progress? How often?]
- **Final Report:** [Audience, format, timeline]
- **Escalation Points:** [Conditions that trigger escalation; who to contact]
```

---

## Technical Resources Section

### **Objective**
Production-ready queries, scripts, or tools created during investigation, formatted for reuse.

### **Template**

```
## Resource 1: [Descriptive Name]

### Purpose
[What does this resource accomplish?]

### Code
\`\`\`sql
-- Example: SQL query with comments
SELECT column1, column2, COUNT(*) as count
FROM table_name
WHERE condition = 'value'
GROUP BY column1, column2
-- Comment explaining logic
\`\`\`

### Parameters to Modify
- `[PLACEHOLDER]` – What should this be?
- `[DATE_RANGE]` – How to set date range?

### Expected Results
- What should query/script return?
- Sample output (if helpful)

### Troubleshooting
- **Error: [Common error]** → Solution
- **Empty results?** → Check [condition]

## Resource 2: [Next Query/Script]
- [Same structure]
```

---

## Why This Framework Works

### **For You (Investigator)**
✅ Prevents analysis paralysis (clear phases guide thinking)  
✅ Documents decision-making (why you took each step)  
✅ Provides replicable methodology (repeatable for similar issues)  
✅ Creates portfolio evidence (demonstrates strategic thinking)  

### **For Your Team**
✅ Clear handoff (next team knows exactly where you left off)  
✅ Reduced ramp-up time (action items are explicit)  
✅ Methodology sharing (team can replicate your approach)  
✅ Knowledge preservation (nothing lost between team members)  

### **For Stakeholders**
✅ Shows professional approach (structured thinking, not guessing)  
✅ Accountability points (each phase has deliverable)  
✅ Risk management (identifies scope boundaries early)  
✅ Strategic value (demonstrates problem-solving capability)  

---

## When to Use This Template

**Use for:**
- Data anomalies (volume drops, missing fields, inconsistent values)
- Performance issues (processing delays, system bottlenecks)
- Complex troubleshooting (multi-system, cross-team investigation)
- Strategic research projects (comprehensive analysis, cross-functional work)
- Process optimization (efficiency improvements, workflow redesign)

**Don't use for:**
- Simple, single-step issues (would be overkill)
- Well-documented standard procedures (follow existing SOPs)
- Routine maintenance tasks (operational work)

**Optional use for:**
- Training documentation (teaching methodology through example)
- Knowledge transfer (onboarding new team members)
- Process improvements (documenting methodology refinement)

---

## Example Use Cases

**Technical Investigations:**
- Database performance degradation
- API integration failures
- System architecture issues

**Data Analysis:**
- Data quality anomalies
- Pattern recognition research
- Trend analysis projects

**Process Research:**
- Workflow optimization
- Efficiency bottlenecks
- Cross-functional alignment

**Strategic Projects:**
- Market research initiatives
- Competitive analysis
- Long-term planning research

---

## Version History

**Version 1.0:** Created February 16, 2026  
**Based on:** Proven methodologies for systematic investigation and research  
**Next update:** After project completion; add learnings and improvements

---

## Notes for Implementation

### Getting Started
1. Copy this template for each new investigation
2. Name file descriptively: `[Project_Name]_Investigation.md`
3. Update cross-references to match your file structure
4. Customize phases based on project complexity

### Best Practices
- Start with Phase 1 even if problem seems simple (prevents scope creep)
- Document hypotheses before investigating (prevents confirmation bias)
- Update template as you learn (living document)
- Archive completed investigations for future reference

### Scaling the Framework
- **Simple projects:** Use Phases 1, 2, and 4 only
- **Complex projects:** Use all 4 phases with detailed sub-steps
- **Recurring issues:** Create specialized version of template for that issue type
- **Team onboarding:** Use completed examples as training materials

---

**Related Templates:** Hub Index | Reference Library | Decision Tree | Communication Standards
