# VSD-RISC-V Week 1 - Day 1

## Overview

This repository contains the Day 1 exercises for the VSD-RISC-V workshop.
The main focus was on **RTL design**, **synthesis using Yosys**, and the **Sky130 PDK**.

---

## Folder Structure

```
Day1/
├─ Task4_Yosys_Sky130/
│  ├─ task_4.md
│  └─ images/
│      └─ tk4.png
├─ Verilog/
│  ├─ good_mux.v
│  └─ ...
└─ README.md
```

---

## Labs and Activities

### 1. RTL Design

* Created a simple 2-to-1 MUX (`good_mux.v`) in Verilog.
* Learned the basics of **Register Transfer Level (RTL) coding** and simulation.

### 2. Synthesis Using Yosys & Sky130 PDK

The MUX design was synthesized using the following workflow:

1. **Launch Yosys** and set working directory.
2. **Read the standard cell library and Verilog design**:

```tcl
read_liberty -lib path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog path/to/good_mux.v
```

3. **Run generic synthesis**:

```tcl
synth -top good_mux
```

4. **Map to technology library**:

```tcl
dfflibmap -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

5. **Visualize the netlist**:

```tcl
show good_mux
```

6. **Write the gate-level netlist**:

```tcl
write_verilog path/to/good_mux_netlist.v
```

7. **Generate synthesis reports**:

```tcl
stat
```

---

### 3. Key Learnings

* Loading **standard cell libraries** and RTL designs in Yosys.
* Understanding the **synthesis flow**: RTL → logic network → gate-level netlist.
* Mapping flip-flops and gates to the Sky130 library.
* Visualizing netlists and analyzing **area, timing, and cell count**.
* Organizing commands and outputs for reproducibility.

---

### 4. Summary

Day 1 emphasized **hands-on RTL design and synthesis**.
The workflow demonstrated **writing Verilog, synthesizing with Yosys, mapping to Sky130 cells, visualizing netlists, and generating reports**, providing a complete RTL-to-gate-level design experience.
