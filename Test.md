# 🧩 Production Defect KPI – Methodology & Decision Rationale

> **A detailed account of what we tested, what we rejected, and why**

This document provides the complete methodology behind the Production Defect KPI redesign. It captures the exploratory process, statistical validation, and practical trade-offs that shaped the final solution.

---

## 📋 Table of Contents

- [Background & Problem Statement](#1-background--problem-statement)
- [Design Objective](#2-design-objective)
- [What We Tested (And Why)](#3-what-we-tested-and-why)
- [Key Insight](#4-key-insight)
- [Introducing DPMR](#5-introducing-dpmr)
- [Mean vs Median Decision](#6-mean-vs-median-decision)
- [Multi-Year Data Usage](#7-multi-year-data-usage)
- [Data Source Adjustment](#8-data-source-adjustment)
- [The 10% Improvement Adjustment](#9-the-10-improvement-adjustment)
- [Dummy Data Example](#10-dummy-data-example)
- [Final KPI Outcome](#11-final-kpi-outcome)
- [Strengths & Limitations](#12-strengths--limitations)
- [Conclusion](#13-conclusion)

---

## 1. Background & Problem Statement

### The Original System

The organisation initially assessed production performance using **SLA-based metrics** such as:
- Response time to incidents
- Resolution time for defects

### What These Metrics Became in Practice

| Intended Purpose | Actual Reality |
|-----------------|----------------|
| Measure service quality | **Highly negotiable** |
| Objective performance data | **Dependent on customer acknowledgement** |
| Indicator of software quality | **Weak correlation** with actual quality |

### The Core Problem

**Projects could appear "green" while still generating a high number of production defects.**

**Example scenario:**
- Project meets all SLA targets (response within 4 hours, resolution within 2 days)
- Customer satisfied with response speed
- **But:** Project generates 50 production defects per month
- **Question:** Is this actually good quality?

### The Governance Gap

From a P&C (Process & Control) perspective, this created a **governance gap**:

- **Management wanted:** Fewer production defects
- **Metrics measured:** How fast we responded to defects
- **Result:** Misalignment between goals and measurement

**The fundamental issue:** We were measuring speed, not quality.

---

## 2. Design Objective

### The KPI Needed To:

| Requirement | Why It Mattered |
|-------------|----------------|
| **Shift focus from speed to quality** | Align metrics with actual management priority |
| **Be resistant to negotiation bias** | Can't be gamed through SLA negotiations |
| **Work across projects of different sizes** | Fair comparison regardless of scale |
| **Remain stable and explainable** | Usable for governance and decision-making |

### Success Criteria

A successful KPI would:
- ✅ Tell us which projects have quality issues (not just slow response)
- ✅ Be defensible in governance meetings
- ✅ Drive the right behaviors (defect prevention, not SLA negotiation)
- ✅ Be simple enough for broad adoption

---

## 3. What We Tested (And Why)

This was an **iterative exploration**, not a pre-planned sequence. Each approach was tested against real data and rejected for specific reasons.

---

### 3.1 SLA-Based Metrics

#### Observation

SLA outcomes were heavily influenced by:
- Customer communication and relationship
- Agreed extensions and renegotiations
- Customer availability for testing and validation
- Project manager negotiation skill

**Example:**
- Project A: 10 defects, all resolved within SLA → Green
- Project B: 5 defects, 1 missed SLA due to customer delay → Amber
- **Question:** Is Project A really higher quality?

#### Conclusion

**SLA compliance ≠ quality performance**

SLA metrics measure responsiveness, not defect prevention.

#### Decision

**Deprioritised as a primary quality KPI.**

---

### 3.2 Raw Defect Counts

#### Observation

Larger projects naturally produced more defects:

| Project | Size | Defects | Raw Count Ranking |
|---------|------|---------|------------------|
| Small project | RM 500K | 3 defects | Best |
| Medium project | RM 2M | 8 defects | Worst |
| Large project | RM 5M | 6 defects | Middle |

**The problem:** Medium project looks worst, but it's also 4x larger than the small project.

#### Conclusion

**Raw counts penalise scale rather than quality.**

#### Decision

**Rejected.**

We needed normalization by project size/exposure.

---

### 3.3 Grouping Projects by PO Value

#### The Approach

Projects were grouped into size bands to "level the playing field":

**Example grouping:**
- Small: PO < RM 1M
- Medium: RM 1M - RM 5M  
- Large: PO > RM 5M

Each group would have its own benchmark.

#### What We Found

**Problem 1: Arbitrary boundaries**
- Why is RM 1M the cutoff?
- Project at RM 999K vs RM 1.001M treated completely differently
- No natural breakpoints in the data

**Problem 2: Small sample sizes**
- Some groups had only 5-7 projects
- Statistical reliability suffered
- One outlier could skew entire group benchmark

**Problem 3: Results changed with reclassification**
- Minor adjustment to boundaries → completely different rankings
- Not stable enough for governance use

#### Statistical Testing

We performed:
- **Correlation analysis:** PO value vs defect count
- **ANOVA testing:** Comparing defect rates across groups

**Purpose of these tests:** Sanity checks to see if grouping made statistical sense

**Important note:** These were performed **as sanity checks**, not as modeling techniques.

#### What the Tests Showed

**No consistent relationship** between PO value and defect behavior:
- Some large projects had low defects
- Some small projects had high defects
- No clear pattern that justified grouping

**ANOVA results:**
- High within-group variation
- Overlap between groups
- Grouping didn't explain defect variation well

#### Conclusion

**Grouping added complexity without improving fairness.**

#### Decision

**Rejected grouping approach.**

---

### 3.4 Percentile-Based Scoring

#### Observation

Percentiles ranked projects relative to each other but:

**Problem 1: Shifted every year**
- Percentile cutoffs changed every year as new projects added
- What was "Score 4" last year might be "Score 3" this year
- No stable target to aim for

**Problem 2: Penalised teams even when absolute performance improved**
- If ALL projects improve, distribution still forces 20% into bottom tier
- Team improves from 10 defects to 5 defects but still gets Score 1
- Demotivating and unfair

**Problem 3: Created confusion when results clustered closely**
- Many projects with similar DPMR (e.g., 8.5, 8.7, 9.0, 9.2)
- Percentile ranking created artificial distinctions
- Hard to explain why 8.7 is Score 4 but 9.0 is Score 3

#### Decision

**Removed to preserve stability and interpretability.**

---

## 4. Key Insight

After testing multiple approaches, we reached a critical realization:

> **PO value does NOT explain defect behavior, but project size still affects exposure.**

### What This Means

**PO value does NOT explain:**
- ❌ "Large projects will have X more defects"
- ❌ Can't predict defect count from PO value
- ❌ Statistical tests (correlation, ANOVA) showed no consistent pattern

**Project size DOES affect:**
- ✅ Exposure: More functionality = more defect opportunities
- ✅ Need for normalization to compare fairly
- ✅ Context for interpreting defect counts

### Therefore

**The right approach:**
- **PO should NOT be used** to predict defects
- **PO SHOULD be used** to normalise defect counts

**This distinction guided the final design.**

---

## 5. Introducing DPMR

### Definition

```
DPMR = (Number of Production Defects / PO Value in MYR) × 1,000,000
```

**DPMR = Defects Per Million Ringgit**

### Why Multiply by 1,000,000?

Makes numbers interpretable:
- Without: 0.000008 defects per Ringgit
- With: 8 DPMR

**Same reason "per million" is used in manufacturing and quality metrics (e.g., PPM - parts per million).**

### Example Calculation

**Project A:**
- PO Value: RM 2,000,000
- Production Defects: 6

```
DPMR = (6 / 2,000,000) × 1,000,000
     = 0.000003 × 1,000,000
     = 3 DPMR
```

### Why DPMR Works

| Feature | Benefit |
|---------|---------|
| **Adjusts for scale** | Fair comparison between RM 500K and RM 5M projects |
| **Focuses directly on defect density** | Quality metric, not volume metric |
| **Aligns with management's quality priority** | Fewer defects per unit of investment |
| **Simple to calculate and explain** | No complex formulas or groupings |
| **Stable** | Not dependent on relative rankings or negotiations |

### Interpretation

**Lower DPMR = Better quality**

- Project with 3 DPMR has better quality density than project with 10 DPMR
- Regardless of absolute project size
- Clear, intuitive direction for improvement

---

## 6. Mean vs Median Decision

### The Trade-Off

When establishing the benchmark DPMR, we had to choose:

| Approach | Advantages | Disadvantages |
|----------|-----------|---------------|
| **Median** | Robust to outliers | Harder to explain to stakeholders |
| **Mean** | Intuitive, commonly understood | Sensitive to extreme values |

### What Was Considered

#### Data Quality Review
- Reviewed all data points for obvious errors
- Checked for extreme outliers
- Found data quality acceptable
- No systematic data issues identified

#### Sample Size Reality
- Only **three data points per project** existed in many cases
- Small samples meant both mean and median had limitations
- Need to balance statistical rigor with practical constraints

#### Stakeholder Communication
- Mean is more intuitive for management
- "Average DPMR across projects" is easy to grasp
- Median requires more explanation ("middle value when sorted")

### Noise Risk Assessment

**Question:** Would outliers significantly distort the mean?

**Analysis:**
- Reviewed distribution of DPMR values
- Checked for extreme outliers
- Assessed potential impact on mean

**Finding:** Noise risk was assessed as **low**
- No extreme outliers that would severely skew mean
- Distribution reasonably well-behaved
- Difference between mean and median was small

### Final Decision: Mean

The **mean DPMR** was selected because:

1. ✅ **Noise risk was assessed as low** - No major outlier concerns
2. ✅ **Mean is intuitive for stakeholders** - Easier to explain and defend
3. ✅ **Mean aligns better with KPI communication** - "Average performance" is clear
4. ✅ **Simplicity outweighed marginal robustness gains** - Small benefit from median didn't justify added complexity

### Important Note

**This was a conscious trade-off, not an oversight.**

We explicitly considered median and chose mean based on:
- Data quality assessment
- Stakeholder needs
- Communication requirements
- Practical constraints

**If future data shows significant outlier issues, this decision can be revisited.**

---

## 7. Multi-Year Data Usage

### Approach

Historical data across **three years** was used in the analysis.

### Purpose

**NOT for scoring individual projects**, but to:

1. **Understand typical defect behaviour**
   - What's the normal range of DPMR?
   - Are there seasonal patterns?
   - How much variation exists?

2. **Avoid overreacting to single-year anomalies**
   - One unusual year shouldn't define benchmarks
   - Multi-year view provides stability
   - Captures different project types and conditions

3. **Establish credible baseline**
   - Larger sample size = more reliable mean
   - Better confidence in benchmark
   - Reduces impact of individual outliers

### Important Clarification

**This was contextual analysis, not a scoring method.**

- ❌ We did NOT score projects based on 3-year average
- ✅ We DID use 3 years of data to establish the benchmark
- Individual project scores based on their own DPMR vs the benchmark

### Why This Matters

Using multi-year data for the benchmark provides:
- More stable threshold that doesn't shift dramatically year-to-year
- Better representation of "typical" performance
- Reduced sensitivity to temporary anomalies

---

## 8. Data Source Adjustment

### Original Data Source

**Initially:**
- PO values were derived from **WIP data**
- Completion was inferred from **100% completion rate CRs**

### Issue Discovered

**Problem:** CR data in WIP was **inconsistently maintained**

**Specific issues:**
- Some completed projects not marked as 100%
- CR-PO mapping unclear or missing
- Timeline data unreliable
- Made it difficult to establish accurate baselines

**Impact:**
- Could not confidently identify which projects to include
- Unreliable PO values for some projects
- Benchmark credibility at risk

### Resolution

**Switched to Sales data** 

**Why this was better:**

| Aspect | WIP Data | Sales Data |
|--------|----------|------------|
| **CR-PO mapping** | Unclear | ✅ Clear |
| **Timeline data** | Inconsistent | ✅ Clear |
| **Completeness** | Gaps | ✅ More complete |
| **Reliability** | Questionable | ✅ More reliable for benchmarking |

### Impact on Analysis

- More confident in PO values
- Clearer project boundaries
- Better timeline tracking
- More reliable benchmark establishment

**This change improved data quality and thus benchmark credibility.**

---

## 9. The 10% Improvement Adjustment

### The Data-Driven Foundation

From historical data analysis, we established:
- **Mean DPMR across all projects**
- Statistical distribution of DPMR values
- Typical performance range

**Example (illustrative):**
- Historical mean DPMR: 100 defects per million MYR

### Head of CPMO's Strategic Input

After reviewing the historical analysis, the **Head of CPMO made a strategic decision:**

> "To encourage improvement, the benchmarks will account for 10% improvement—a 10% reduction in the DPMR."

### Implementation

```
Benchmark DPMR = Historical Mean DPMR × 0.9
```

**Using the example:**
- Historical mean: 100 DPMR
- **Adjusted benchmark: 90 DPMR** (10% reduction)

### Rationale for the 10% Adjustment

**Why 10%?**

1. **Achievable but challenging**
   - Not so aggressive it's demotivating (e.g., 30% would be too hard)
   - Not so mild it doesn't drive change (e.g., 2% wouldn't matter)
   - 10% represents meaningful improvement

2. **Industry practice**
   - Many quality improvement frameworks target 10-15% annual improvement
   - Familiar to quality management professionals
   - Aligns with continuous improvement philosophy

3. **Behavioral change**
   - Clear signal: "Good is not good enough"
   - Drives aspiration, not just maintenance
   - Creates improvement culture

### What This Transforms

| Aspect | Without Adjustment | With 10% Improvement |
|--------|-------------------|---------------------|
| **Benchmark** | Historical mean (100) | 90 DPMR |
| **Philosophy** | Descriptive ("where we've been") | Prescriptive ("where we should be") |
| **Message** | "Meet the average" | "Exceed past performance" |
| **Culture** | Status quo | Continuous improvement |
| **Scoring** | Score 3 at mean | Score 3 at improved target |

### Impact on Scoring

**Project with 95 DPMR:**

**Without adjustment:**
- Compared to mean of 100
- Below average → Score 4 (Good)

**With 10% adjustment:**
- Compared to benchmark of 90
- Slightly above target → Score 3 (Acceptable)
- Clear signal: "You're close, but there's room for improvement"

### Why This Matters

**This adjustment transformed the KPI from:**
- A measurement tool → A change management tool
- Describing reality → Shaping future
- Accepting baseline → Driving improvement

**The 10% factor is the bridge between analytical rigor and organizational aspiration.**

---

## 10. Dummy Data Example (Illustrative)

### Sample Projects

| Project | PO Value (RM) | Production Defects | DPMR Calculation | DPMR |
|---------|---------------|-------------------|------------------|------|
| **A** | 1,000,000 | 5 | (5 / 1,000,000) × 1M | 5.0 |
| **B** | 500,000 | 4 | (4 / 500,000) × 1M | 8.0 |
| **C** | 2,000,000 | 6 | (6 / 2,000,000) × 1M | 3.0 |

### Key Insight from This Example

**Despite Project C having more defects than Project A (6 vs 5), it demonstrates better quality density (3.0 vs 5.0 DPMR).**

**Why?**
- Project C is 2x larger than Project A
- 6 defects across RM 2M investment = better density
- 5 defects across RM 1M investment = worse density

**Without normalization:**
- Raw count ranking: A=best (5), C=worst (6)
- **Incorrect conclusion:** Project A has better quality

**With DPMR normalization:**
- DPMR ranking: C=best (3.0), A=middle (5.0), B=worst (8.0)
- **Correct conclusion:** Project C has best quality density

### Applying the 10% Improvement Adjustment

**Assume historical mean DPMR = 6.0**

**Adjusted benchmark:**
```
Benchmark = 6.0 × 0.9 = 5.4 DPMR
```

**Scoring:**

| Project | DPMR | Compared to Benchmark (5.4) | Score |
|---------|------|----------------------------|-------|
| **A** | 5.0 | Below benchmark | 4 (Good) |
| **B** | 8.0 | Well above benchmark | 2 (Needs Improvement) |
| **C** | 3.0 | Well below benchmark | 5 (Excellent) |

---

## 11. Final KPI Outcome

### The Complete KPI Framework

**Core Metric:**
```
DPMR = (Production Defects / PO Value in MYR) × 1,000,000
```

**Benchmark:**
```
Target DPMR = Historical Mean DPMR × 0.9
```

**Scoring System:**

| Score | Quality Level | DPMR vs Benchmark |
|-------|--------------|-------------------|
| **5** | Excellent | Much lower than benchmark |
| **4** | Good | Below benchmark |
| **3** | Acceptable | Around benchmark |
| **2** | Needs Improvement | Above benchmark |
| **1** | Poor | Much higher than benchmark |

### Key Characteristics

✅ **All projects measured on the same DPMR scale**
- No grouping by size
- No separate benchmarks
- Fair comparison

✅ **Fixed benchmark using mean DPMR**
- Stable over time
- Not dependent on relative rankings
- Clear target

✅ **Simple 1-5 scoring**
- Easy to understand
- Familiar format
- Clear communication

✅ **Clear improvement targets**
- 10% better than historical performance
- Aspirational but achievable
- Drives continuous improvement

✅ **Governance-friendly and auditable**
- Transparent calculation
- Defensible methodology
- Clear data sources

---

## 12. Strengths & Limitations

### Strengths

| Strength | Impact |
|----------|--------|
| **Fair across scale** | Small and large projects judged by same standard |
| **Negotiation-resistant** | Not dependent on SLA agreements |
| **Easy to explain** | Simple calculation, clear interpretation |
| **Management-aligned** | Directly measures defect prevention priority |
| **Stable** | Fixed benchmark, not relative ranking |
| **Improvement-driving** | 10% adjustment encourages better performance |

### Limitations

| Limitation | Context |
|-----------|---------|
| **Relies on PO as proxy for exposure** | Best available measure; consistently applied |
| **Assumes defect classification consistency** | P&C processes ensure standardization |
| **Not a root-cause analysis tool** | Identifies issues; separate analysis needed for causes |
| **Three data points per project** | Sufficient for stable DPMR; more data would be ideal |
| **Mean sensitive to outliers** | Data quality reviewed; risk assessed as low |

### What This KPI Is and Isn't

**This KPI IS:**
- ✅ A fair comparison tool
- ✅ A quality screening mechanism
- ✅ A conversation starter about defect prevention
- ✅ A governance tracking metric

**This KPI IS NOT:**
- ❌ A root cause diagnostic
- ❌ A replacement for all quality metrics
- ❌ A perfect statistical model
- ❌ A guarantee of zero defects

---

## 13. Conclusion

### What This Redesign Achieved

This KPI redesign prioritised **clarity, fairness, and usefulness** over theoretical perfection.

**The journey:**
1. Started with a governance gap (SLA ≠ quality)
2. Tested multiple approaches (grouping, percentiles, SLA-based)
3. Rejected complexity without clear value
4. Landed on DPMR as the fair normalizer
5. Chose mean for practical reasons
6. Added 10% improvement for behavioral impact

**The result:**
- Fair metric that works across project sizes
- Simple enough for organization-wide adoption
- Aligned with management's quality priority
- Drives continuous improvement culture

### How Analytics Actually Works in Organizations

The final solution reflects how analytics actually works in organizations:

**Not:**
- ❌ Perfect data
- ❌ Unlimited time
- ❌ Purely academic decisions
- ❌ Textbook approaches

**But:**
- ✅ Imperfect data
- ✅ Competing priorities
- ✅ Need to balance rigor with adoption
- ✅ Stakeholder communication requirements
- ✅ Real-world trade-offs

### The Real Value

**This KPI supports better conversations about quality—not just better charts.**

It enables:
- Governance discussions based on data
- Fair project comparisons
- Early identification of quality issues
- Strategic decisions about improvement focus

**The goal was never to build a perfect model.**  
**The goal was to build a useful tool that drives the right behaviors.**

**Mission accomplished.**

---

<div align="center">

**Questions about the methodology?**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](Add-your-LinkedIn-URL-here)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:your.email@example.com)

</div>
