# GLS, Synthesis-Simulation Mismatch & Blocking/Non-Blocking Mismatches

This document explains the importance of **Gate-Level Simulation (GLS)**, how **synthesis-simulation mismatches** occur, and the role of **blocking vs non-blocking assignments** in Verilog coding practices.

---

## 1. Gate-Level Simulation (GLS)

**Definition:**  
GLS is the process of running a testbench using the **netlist** as the Design Under Test (DUT). The netlist is logically equivalent to the RTL in terms of ports.

**DUT (Design Under Test):**  
The DUT is the component or design being tested in the simulation. In GLS, the DUT is the **synthesized netlist**.

**Why GLS is needed:**
- To verify the logical correctness of the design after synthesis.
- To ensure the timing of the design is met.

**Delay Annotation:**  
GLS often requires delay annotation, which adds timing information from the synthesis tool to the gate-level netlist. This allows the simulation to reflect real timing delays in hardware.

> **Note:** If gate-level Verilog models are delay-annotated, GLS can also be used for timing validation.

**High-Level Block Diagram:**

screenshot

---

## 2. Gate-Level Verilog Model

A **gate-level Verilog model** represents the synthesized cells of a design.

- Example: An AND gate in RTL might be mapped to a synthesized cell named `ANDX1` with corresponding input/output ports.
- The model defines the functionality of the synthesized cell, though the actual cell name may not directly reflect the logic function.

**Types of Gate-Level Models:**
1. **Timing-Aware:** Includes both functionality and timing (not covered here).
2. **Functionality Only:** Focuses only on logical behavior (used in most verification).

---

## 3. Synthesis-Simulation Mismatch

**Definition:**  
A mismatch occurs when the **RTL simulation behavior** does not match the **post-synthesis GLS behavior**.

### Common Causes
- Missing or incomplete sensitivity lists
- Incorrect use of **blocking (`=`)** vs **non-blocking (`<=`)** assignments
- Non-standard Verilog coding styles

---

### 3.1 Missing Sensitivity List

**Example: 2-to-1 MUX**
```verilog
always @(sel or a or b) begin
    if (sel)
        y = a;
    else
        y = b;
end
```

- If the sensitivity list is incomplete, some input changes will not trigger the always block, leading to incorrect behavior.
- **Fix:** Use `@(*)` event-driven sensitivity list.
```verilog
always @(*) begin
    if (sel)
        y = a;
    else
        y = b;
end
```

---

### 3.2 Blocking vs Non-Blocking Assignments

#### Blocking (`=`)
- Executes statements **in order**, one after another.
- Can cause simulation-only behavior that does not match hardware.

#### Non-Blocking (`<=`)
- RHS expressions are evaluated **in parallel** and assigned at the end of the time step.
- Matches hardware behavior more accurately.

---

#### Example – Sequential Logic (Shift Register)

**Incorrect (Blocking):**
```verilog
always @(posedge clk or posedge rst) begin
    if (rst) begin
        q0 = 0;
        q  = 0;
    end else begin
        q  = q0;
        q0 = d; 
    end
end
```
- Needs **2 flip-flops** but behaves as **1 flip-flop** because of blocking order.

**Correct (Non-Blocking):**
```verilog
always @(posedge clk or posedge rst) begin
    if (rst) begin
        q0 <= 0;
        q  <= 0;
    end else begin
        q0 <= d;
        q  <= q0; 
    end
end
```
- Produces correct **2-bit shift register** logic.

---

#### Example – Combinational Logic

**Incorrect (Blocking order issue):**
```verilog
always @(*) begin
    y  = q0 & c;
    q0 = a | b;
end
```
- `y` depends on the **old value of q0** → behaves sequentially.

**Fix:** Reorder statements or use non-blocking where needed.
```verilog
always @(*) begin
    q0 = a | b;
    y  = q0 & c;
end
```

---

## Key Takeaways
- GLS is essential for **post-synthesis verification**.
- Missing sensitivity lists cause simulation-only behavior.
- Blocking vs non-blocking assignments are **critical** for accurate hardware modeling.
- Always use **non-blocking (`<=`)** in sequential logic.
- Always run GLS to detect potential synthesis-simulation mismatches.
