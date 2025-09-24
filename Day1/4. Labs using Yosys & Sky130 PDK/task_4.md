# Task 4: Labs using Yosys & Sky130 PDK

In this final subdivision of Day 1, I performed hands-on labs using Yosys along with the Sky130 PDK to synthesize and realize a simple MUX design. The lab was divided into three parts.

## What I Learned
During this session, I understood:

- How to load a standard cell library in Yosys using `read_liberty`.
- How to import an RTL Verilog design using `read_verilog`.
- The synthesis flow in Yosys from RTL to intermediate representation using `synth`.
- How to write the synthesized gate-level netlist using `write_verilog`.
- Importance of organizing commands and outputs to track the synthesis process.

## Synthesis of good_mux.v
Synthesis converts the RTL (Register Transfer Level) description written in Verilog into a gate-level netlist mapped to a given standard cell library.

### Flow of Synthesis
1. Write the RTL design in Verilog.
2. Choose a target technology library (e.g., `sky130_fd_sc_hd`).
3. Run synthesis using Yosys, which translates the RTL into logic gates.
4. Analyze the synthesis reports (area, timing, gate-level netlist).

### Step 1: Launch Yosys
```bash
cd ~/Documents/Verilog/Labs  # Directory where you want to store your synthesis files
yosys
```

### Step 2: Read the Design and Library Files
```bash
read_liberty -lib ~/Documents/Verilog/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ~/Documents/Verilog/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
```
**Notes:**
- `read_liberty`: Reads the target standard cell library.
- `read_verilog`: Reads the Verilog design file.

### Step 3: Run Generic Synthesis
```bash
synth -top good_mux  # -top good_mux: Specifies the top-level module for synthesis
```

### Step 4: Map to Technology Library
```bash
dfflibmap -liberty ~/Documents/Verilog/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ~/Documents/Verilog/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
**Notes:**
- `dfflibmap`: Maps flip-flops to standard cells.
- `abc`: Optimizes logic and maps it to the technology library.

### Step 5: Visualize the Netlist
```bash
show good_mux
```
**Tip:**
- Save visualization to a PNG file:
```bash
show -format png good_mux_netlist.png good_mux.v
```
![Visualization of Netlist](tk4.jpg)


### Step 6: Write the Gate-Level Netlist
```bash
write_verilog ~/Documents/Verilog/Labs/good_mux_netlist.v
```
This generates the synthesized gate-level netlist in Verilog format.

### Step 7: Generate Reports
```bash
stat
```
**Tip:**
- Save reports to a text file:
```bash
yosys -p "read_verilog good_mux.v; synth -top good_mux; stat" > ~/Documents/Verilog/Labs/good_mux_stats.txt
```
- Make sure the target folder exists before running the command.

### Synthesis Outputs
**Netlist Dot File:**
- Dot file generated for visualization.

**Statistics:**
```
+----------Local Count, excluding submodules.
|
8 wires
8 wire bits
4 public wires
4 public wire bits
4 ports
4 port bits
1 cells
1   sky130_fd_sc_hd__mux2_1
```
**Rough Area:**
- Multiply counts × Liberty cell area
```
1 × sky130_fd_sc_hd__mux2_1
1 × 4.0 µm² = 4.0 µm²
Estimated area: 4.0 µm²
```

## Summary
In this section, we performed RTL-to-gate-level synthesis of `good_mux.v` using Yosys. The workflow included:

- Launching Yosys and reading the Verilog design along with the target standard cell library.
- Running generic synthesis (`synth`) to convert the RTL into a logic network.
- Mapping to the technology library (`dfflibmap` and `abc`) for gate-level optimization.
- Visualizing the synthesized netlist with `show` to inspect gates and connections.
- Writing the gate-level netlist (`write_verilog`) to a specified directory for later use.
- Generating synthesis reports (`stat`) to check cell count, area, and other metrics.

This flow demonstrates the complete synthesis cycle: **read design → synthesize → map → visualize → save netlist → analyze reports**, ensuring both functional correctness and readiness for further backend flows such as placement and routing.
