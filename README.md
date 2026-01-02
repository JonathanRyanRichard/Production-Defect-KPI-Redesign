# 📊 Redesigning a Production Defect KPI (P&C Case Study)

> **Creating a fair, stable, and management-aligned quality metric**

**Context:** Process & Control (P&C)  
**Challenge:** SLA-based metrics reflected negotiation outcomes, not quality  
**Solution:** DPMR-based KPI with 10% improvement adjustment

![Status](https://img.shields.io/badge/status-completed-success)
![Impact](https://img.shields.io/badge/impact-organization--wide-blue)
![Approach](https://img.shields.io/badge/approach-iterative-orange)

---

## 📋 Table of Contents

- [Why This Exists](#why-this-exists)
- [Objective](#objective)
- [How the Work Was Approached](#how-the-work-was-approached)
- [Final Outcome](#final-outcome)
- [Repository Contents](#repository-contents)
- [What This Demonstrates](#what-this-demonstrates)

---

## 🎯 Why This Exists

This case study documents a **practical KPI redesign** carried out within a P&C (Process & Control) context.

### The Goal

The goal was not to build a perfect statistical model, but to create a **fair, stable, and management-aligned quality metric** that could actually be used.

### The Problem

The organisation historically relied on **SLA-based metrics** to evaluate project team's performance. Over time, it became clear that these metrics reflected **negotiation outcomes rather than true delivery quality**.

**What this meant:**
- Projects with well-negotiated SLAs scored better, regardless of actual defect quality
- Metrics rewarded negotiation skill, not defect prevention
- Management couldn't reliably compare quality across projects

### What This Work Captures

How we moved from that reality to a clearer, defect-focused KPI.

---

## 🎯 Objective

### To Design a Production Defect KPI That:

| Requirement | Why It Matters |
|-------------|----------------|
| ✅ **Reflects quality, not negotiation skill** | Measure actual defect performance |
| ✅ **Allows comparison across projects of different sizes** | Fair evaluation regardless of project scale |
| ✅ **Simple enough for broad adoption** | Must be understandable and usable |
| ✅ **Aligns with management's priority: fewer production defects** | Support organizational quality goals |
| ✅ **Can be explained and defended confidently** | Must make sense to all stakeholders |

---

## 🧭 How the Work Was Approached

This was an **exploratory and iterative** process, not a pre-defined statistical exercise.

### Our Process

```
Challenge existing assumptions
         ↓
Test ideas against real project data
         ↓
Reject approaches that added complexity without value
         ↓
Prioritise clarity, stability, and fairness
         ↓
Validate with statistics
```

### Key Principles

**We:**
- ✅ Challenged existing assumptions
- ✅ Tested ideas against real project data
- ✅ Rejected approaches that added complexity without value
- ✅ Prioritised clarity, stability, and fairness

**Statistical techniques were used to validate decisions, not dictate them.**

### What We Rejected

During the iterative process, we rejected several approaches:

| Approach Considered | Why We Rejected It |
|-------------------|-------------------|
| **Project grouping** | Added complexity without clear value |
| **Percentile ranking** | Difficult to explain and defend |
| **SLA dependency** | Would reintroduce negotiation bias |

---

## ✅ Final Outcome

### The Final KPI

**Core Metric:**
```
DPMR (Defects Per Million Ringgit) = (Production Defects / Project Cost in MYR) × 1,000,000
```

### Key Features

| Feature | Benefit |
|---------|---------|
| **Uses DPMR** | Normalises defect counts by project cost |
| **Mean-based threshold** | Benchmarks projects fairly |
| **Avoids grouping** | No arbitrary project categorization |
| **Avoids percentile ranking** | Clear, stable thresholds |
| **No SLA dependency** | Independent of negotiation outcomes |
| **1-5 quality score** | Simple, clear rating system |

### The Strategic Adjustment

While benchmarks were established based on historical data, the **Head of CPMO made a strategic decision**:

> "To encourage improvement, the benchmarks will account for 10% improvement—a 10% reduction in the DPMR."

**Implementation:**
```
Benchmark DPMR = Historical Mean DPMR × 0.9
```

**What this achieves:**
- Sets aspirational targets, not just historical baselines
- Drives continuous improvement culture
- Aligns with management's quality agenda
- Transforms metric from descriptive to prescriptive

**Example:**
- Historical mean DPMR: 100 defects per million MYR
- **Adjusted benchmark: 90 DPMR** (10% reduction)
- Projects evaluated against the improved target

### Quality Score Framework

| Score | Quality Level | Interpretation |
|-------|--------------|----------------|
| **5** | Excellent | Outstanding defect prevention |
| **4** | Good | Better than adjusted target |
| **3** | Acceptable | Meeting improved target |
| **2** | Needs Improvement | More defects than target |
| **1** | Poor | Significant quality issues |

### Why This Works

✅ **Directly supports management's quality agenda**  
✅ **Fair comparison across all project sizes**  
✅ **Simple enough for organization-wide adoption**  
✅ **Can be explained and defended confidently**  
✅ **Encourages continuous improvement through 10% adjustment**

---

## 📁 Repository Contents

| File/Folder | Description |
|------------|-------------|
| **`README.md`** | This overview (you are here) |
| **`Defect-KPI-Methodology.md`** | Full methodology, reasoning, and trade-offs |


### What You'll Find in the Methodology

The **`Defect-KPI-Methodology.md`** document provides:

- Complete design journey and decision-making process
- Detailed explanation of approaches tested and rejected
- Statistical validation of the final approach
- Dummy data examples demonstrating the calculations
- Trade-off discussions and reasoning
- Implementation guidance

---

## 🧠 What This Demonstrates

### Professional Skills

| Skill | Application |
|-------|-------------|
| **KPI design in imperfect data environments** | Working with real-world constraints and messy data |
| **Balancing statistical rigor with business reality** | Using statistics to validate, not dictate |
| **Translating analysis into decision-ready tools** | Creating usable metrics, not just academic exercises |
| **Saying "no" to over-engineered solutions** | Prioritizing simplicity and adoptability |

### Key Capabilities

#### 1. Working with Real Constraints

- Imperfect data
- Organizational politics (SLA negotiations)
- Need for broad adoption
- Management alignment requirements

**Result:** A metric that works in the real world, not just on paper.

#### 2. Iterative Problem-Solving

**Not a linear process:**
- Explored multiple approaches
- Tested against real data
- Rejected complexity without value
- Refined until fit-for-purpose

#### 3. Stakeholder-Focused Design

**Every decision guided by:**
- Will management understand this?
- Can projects explain their score?
- Does this drive the right behaviors?
- Is this defensible?

#### 4. Strategic Collaboration

**Data-driven foundation + Leadership vision:**
- Established benchmarks from historical analysis
- Head of CPMO provided strategic direction (10% improvement)
- **Result:** Metric that's both analytical and aspirational

---

## 🎓 Key Lessons

### 1. Simple Usually Beats Complex

**Could have:** Added sophisticated weighting, groupings, multiple tiers  
**Did instead:** Kept it simple with DPMR and clear thresholds  
**Result:** Actually adopted and used organization-wide

### 2. Statistics Validate, Don't Dictate

**Statistics helped us:**
- ✅ Validate that DPMR was stable across project sizes
- ✅ Establish a reasonable mean from historical data
- ✅ Test sensitivity to outliers

**Statistics did NOT:**
- ❌ Force us into complex models
- ❌ Override business judgment
- ❌ Dictate the final threshold

### 3. Aspirational Targets Drive Improvement

**Historical mean alone:** Describes where we've been  
**10% improvement adjustment:** Prescribes where we should be

**Impact:** Shifts culture from "meeting baseline" to "continuous improvement"

### 4. Rejection Is Part of the Process

**What we rejected mattered as much as what we kept:**
- Grouping → Added complexity without value
- Percentile ranking → Hard to explain
- SLA dependency → Reintroduced the original problem

**Lesson:** Be willing to say "no" even after investing time exploring an approach.

---

## 💼 Business Impact

### What Changed

| Before | After |
|--------|-------|
| SLA-based metrics | DPMR-based metrics |
| Negotiation skill rewarded | Quality rewarded |
| Can't compare projects fairly | Fair comparison via normalization |
| Historical baseline acceptance | Aspire to 10% improvement |
| Management frustrated | Management confident in metric |

### The Real Value

**Not just a formula.**

The real value is:
1. **Management has a metric they trust** for quality decisions
2. **Projects evaluated fairly** regardless of size or SLA negotiations
3. **Built-in improvement driver** through the 10% adjustment
4. **Organizational alignment** around defect reduction

---

## 🔮 Future Enhancements

While the current solution is fit-for-purpose, potential future work could include:

- **Trend analysis:** Track DPMR over time per project
- **Severity weighting:** Differentiate critical vs low severity defects
- **Root cause linking:** Connect high DPMR to specific practices
- **Industry benchmarking:** Compare internal DPMR to external standards

**Important:** Only pursue if clear value justifies added complexity.

---

## 🛠️ Technical Approach

### Tools Used

- **Excel:** Data exploration, initial analysis, statistical validation and testing
- **Statistical methods:** Distribution analysis, mean calculation, sensitivity testing

### Methodology

- Descriptive statistics to understand historical defect patterns
- Comparative analysis across different project sizes
- Sensitivity testing to ensure DPMR stability
- Stakeholder validation at key decision points

---

## 📞 About This Work

This case study demonstrates:

- **Practical analytics** over theoretical perfection
- **Iterative problem-solving** in real-world constraints  
- **Business-first thinking** in metric design
- **Collaboration** between analysis and leadership
- **The discipline** to keep things simple

**It's not about building the most sophisticated model.**  
**It's about building the right model that actually gets used.**

---

<div align="center">

**💡 Interested in discussing KPI design or P&C work?**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](Add-your-LinkedIn-URL-here)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:your.email@example.com)

</div>

---

## 📝 Note

**This README provides the "what" and "why."**  
**The methodology document (`Defect-KPI-Methodology.md`) provides the "how" in detail.**
