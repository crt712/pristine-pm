# Simulation Layer

Purpose:
This folder contains hypothetical scenario analysis used for exploration and decision support.

This is NOT a source of truth.

---

## Core Rules

1. Simulation Mode Only
All files in this folder are exploratory and must be treated as non-real.

2. No Canon Influence
Content in this folder must NOT:
- update STATUS.md
- update CONTEXT.md
- update DECISIONS.md
- update WORKING_SESSION.md

3. No Implicit Promotion
Simulation outputs do not become decisions automatically.
All promotion to canon must be explicit and manual.

4. Assumptions Required
All simulations must clearly state:
- inputs
- ranges
- unknowns

5. Directional Outputs Only
Simulations should produce directional insights, not definitive answers.

---

## Planning Assumptions vs Simulation Inputs

Planning assumptions that exist in canon or artifacts are distinct from simulation inputs.

- Planning assumptions = working directional estimates used to structure the business (e.g., MSRP targets, rough COGS, attach rates)
- Simulation inputs = exploratory variables used to test scenarios and sensitivity

Simulation inputs must:
- remain isolated within the simulations layer
- not overwrite, reinterpret, or replace planning assumptions
- not be promoted to canon without validated real-world data

Planning assumptions may inform simulation ranges, but simulation outputs must not redefine them.

---

## Labeling Requirement

Every simulation file must begin with:

SIMULATION MODE: TRUE  
DO NOT TREAT AS REAL DATA

---

## Usage

This layer is used to:
- explore tradeoffs
- test sensitivity (cost, pricing, CAC, etc.)
- evaluate decision scenarios before committing to reality

This layer is NOT used to:
- define product specs
- define pricing
- define financials
- define operational decisions

---

## Promotion Rule

Information may only move from simulation to canon when:
- inputs are validated in the real world
- assumptions are replaced with actual data
- a human explicitly promotes the decision
