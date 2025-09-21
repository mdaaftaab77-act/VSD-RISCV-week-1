# 3. Introduction to Yosys & Logic Synthesis

In this subdivision, I learned about logic synthesis and how to use Yosys, an open-source synthesis tool, to convert RTL designs into gate-level netlists.

---

## 🎯 What I Learned
During this session, I understood:

- What a synthesizer is and its role in converting RTL to netlist.  
- The Yosys setup and basic workflow for synthesis.  
- How to verify the synthesized design using the same testbench as RTL.  
- The concepts of gate-level netlist and `.lib` files.  
- The need for different flavors of standard cells (fast, medium, slow) and how they affect performance and hold constraints.  
- How to guide the synthesizer with constraints for optimal implementation.  

---

## 🔹 What is a Synthesizer?
- A synthesizer converts an RTL design (behavioral representation) into a **gate-level netlist**, which can then be used for physical implementation.  
- The tool we are using is **Yosys**, an open-source logic synthesis tool.  
- The synthesis process ensures that **primary inputs and outputs remain the same**, so the same testbench can be used to verify both RTL and synthesized netlist.  

📌 *Screenshot: Yosys setup flowchart*  
📌 *Yosys Setup*  

---

## 🔹 Logic Synthesis
- **RTL design** represents the behavior of the design in code.  
- **Gate-level netlist** is the output of synthesis, describing the design in terms of logic gates and their connections.  
- **.lib files** contain a collection of standard cells, including basic logic gates (NAND, OR, etc.) in different flavors: slow, medium, fast.  

---

## 🔹 Basic Definition of Setup and Hold Time
- **Setup Time (T_setup):**  
  The minimum time interval before the clock edge during which the data input must remain stable so that a flip-flop can correctly capture it.  

- **Hold Time (T_hold):**  
  The minimum time interval after the clock edge during which the data input must remain stable to ensure correct capture by the flip-flop.  

Violating setup/hold times can lead to metastability or incorrect outputs.  

---

## 🔹 Why Different Flavors of Cells?

- **Fast cells** are used to increase circuit speed.  
- Maximum clock frequency depends on combinational delay:  

📌 *Circuit Diagram for Analysis*  
📌 *Formula*  

\[
T_{clock} \geq T_{CQA} + T_{comb} + T_{setup\_B}
\]

Where:  
- T_CQA = propagation delay of the first flip-flop  
- T_comb = delay of combinational logic  
- T_setup_B = setup time of the second flip-flop  

- **Slow cells** are used to prevent hold-time violations:  

\[
T_{hold\_B} < T_{CQA} + T_{comb}
\]

---

## 🔹 Observations on Fast vs Slow Cells

| **Characteristic**     | **Fast Cells**                                   | **Slow Cells**                                   |
|-------------------------|-------------------------------------------------|-------------------------------------------------|
| Delay                  | Low, charges/discharges capacitance quickly      | Higher, slower operation                        |
| Area                   | Higher                                           | Lower                                           |
| Power                  | Higher                                           | Lower                                           |
| Hold-time Violations   | More likely if overused                          | Less likely                                     |
| Performance            | High, can meet timing requirements               | Sluggish if overused                            |
| Transistor Width       | Wide transistors, source more current, faster    | Narrow transistors, source less current, slower |
| Usage Guidance         | Use selectively to meet speed requirements       | Use selectively to avoid hold-time issues       |

---
