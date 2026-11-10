# Requirements Calibration

Use when calibrating specification detail or writing a vision-and-scope document. Load only the relevant section.

## Detail level per requirement

When writing a requirement, spec, or design doc, calibrate how much detail to commit to paper.

**Write more detail when:**
- The customer is an external client.
- Development or testing is outsourced.
- The team is distributed.
- System testing will be driven from the requirements.
- Accurate estimates are needed.
- Traceability is required.

**Write less detail when:**
- The customer is internal.
- The customer is involved throughout.
- Developers are domain experts.
- Precedents exist to follow.
- A package solution supplies the behavior.

**The calibration test:** if a developer can think of several acceptable ways to satisfy a requirement and you'd be content with any of them, the detail level is right. If only one implementation would satisfy you, you've under-specified; if every plausible implementation would be wrong, you've over-specified.

## Vision-and-scope document sections

A vision-and-scope document frames *why* and *how much* before any requirements are written—it's the forward-looking complement to a Design Doc. Cover these sections; omit one only deliberately, noting why.

**1. Business requirements**
- **Background**—the history that led to building this product.
- **Business opportunity**—the problem solved or market addressed.
- **Business objectives**—quantitative, measurable benefits.
- **Success metrics**—indicators tracked during and after release.
- **Vision statement**—long-term purpose and intent.
- **Business risks**—what could go wrong (market, timing, adoption).
- **Business assumptions and dependencies.**

**2. Scope and limitations**
- **Major features**—top-level capabilities, each uniquely labeled.
- **Scope of the initial release.**
- **Scope of subsequent releases.**
- **Limitations and exclusions**—what is explicitly out.

**3. Business context**
- **Stakeholder profiles**—categories, the value each gets, attitudes, constraints.
- **Project priorities**—the five dimensions held in tension: features, quality, schedule, cost, staff.
- **Deployment considerations.**
