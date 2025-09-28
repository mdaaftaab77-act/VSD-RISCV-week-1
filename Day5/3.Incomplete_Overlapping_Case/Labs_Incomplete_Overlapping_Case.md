# Day 5 - Labs on Incomplete & Overlapping Case

This document explains the third subdivision of **Day 5 - Optimization in Synthesis**.  
In this section, we studied how incomplete case statements and overlapping case statements affect simulation and synthesis.

---

## Files Used
- **incomp_case.v** – Incomplete case example  
- **comp_case.v** – Complete case example  
- **partial_case.v** – Partial assignment in case  
- **bad_case.v** – Overlapping case example  

---

## Concept: Incomplete and Overlapping Case
- **Incomplete Case**: If not all possibilities are covered and no `default` is present → synthesizer infers latches.  
- **Partial Case**: If all branches don’t assign all outputs → latches may be inferred.  
- **Overlapping Case**: If more than one case condition is true, multiple branches can execute → synthesis results may differ from simulation.  
- **Complete Case**: All conditions are covered or a default is provided → no latch inference.  

---

## Analysis of Files

### 1. Incomplete Case (**incomp_case.v**)
- Code contained missing conditions without a default branch.  
- Simulation executed but synthesis inferred latches.  


---

### 2. Complete Case (**comp_case.v**)
- All conditions covered, or a default branch was used.  
- No latch inference during synthesis.  



---

### 3. Partial Assignment Case (**partial_case.v**)
- Outputs were not fully assigned in all branches.  
- Resulted in latch inference.  
- Best practice is to assign all outputs in all case branches.  



---

### 4. Bad Case (Overlapping) (**bad_case.v**)
- Multiple case branches overlapped (more than one condition could be true).  
- RTL simulation produced outputs, but synthesis showed mismatches due to ambiguity.  
- Overlapping cases are unpredictable and must be avoided.  



---

## Observations
- Incomplete case statements lead to latch inference.  
- Partial assignment in case statements also causes latch inference.  
- Overlapping cases result in unpredictable outputs and mismatches between simulation and synthesis.  
- Complete cases with a `default` branch are the correct coding style.  

---

## What I Learned
- Always use a `default` branch in case statements to avoid latches.  
- Assign all outputs in every case branch to ensure consistent synthesis results.  
- Avoid overlapping conditions in case statements, as they may cause hardware mismatches.  
- Good coding practice in case statements leads to predictable and optimized hardware.  

---

## Summary Comparison Table

| Case Type         | Behavior in Simulation             | Behavior in Synthesis            | Risk / Issue                     | Best Practice                          |
|-------------------|------------------------------------|----------------------------------|-----------------------------------|-----------------------------------------|
| **Incomplete Case** | Produces output for given cases only | Infers latches for missing cases | Latch inference                  | Always include `default` branch         |
| **Complete Case**   | Works for all inputs               | Synthesizes cleanly, no latches  | No major issues                  | Cover all cases or add `default`        |
| **Partial Assignment** | Produces outputs for some signals only | Latch inference for unassigned ones | Incomplete hardware assignment   | Assign all outputs in all branches      |
| **Overlapping Case** | Multiple matches may be executed   | Unpredictable hardware behavior  | Simulation vs synthesis mismatch | Ensure no overlapping conditions        |

---
