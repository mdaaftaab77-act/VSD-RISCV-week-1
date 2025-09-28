# Labs on Synthesis-Simulation Mismatch for Blocking Statements

In this final subdivision of **Day 4**, we explored **synthesis-simulation mismatches** caused by **blocking assignments**.

We used the Verilog module:

- `blocking_caveat.v` – a combinational logic circuit combining AND and OR gates.

---

## 1. Lab Procedure

### RTL Simulation
- Simulated `blocking_caveat.v` at the RTL level.  
- Verified logical behavior of the combinational circuit.  
- Observed the waveform and identified how **blocking assignments** affect signal updates.  

### Synthesis & GLS Simulation
- Synthesized the design to generate the **gate-level netlist**.  
- Ran **GLS** to simulate the synthesized netlist.  
- Compared **GLS waveform** with **RTL simulation** to identify mismatches.  

---

## 2. Observations

### In RTL Simulation:
- Some signals depended on older values of other variables due to **blocking assignments (`=`)**.  
- This behavior sometimes mimics **flip-flops**, even in combinational logic.  

### In GLS Simulation:
- The synthesized design performed correctly according to hardware logic.  
- A clear **synthesis-simulation mismatch** was observed between RTL and GLS waveforms.  

**Lesson:**  
Blocking assignments in combinational logic can introduce **unexpected sequential behavior** in RTL simulation.  
GLS is necessary to confirm the **actual hardware functionality**.  

---

## 3. Screenshots

1. **Verilog Code Screenshot**  
![Screenshot](images/7.1.png)


2. **RTL Simulation Waveform**  
![Screenshot](images/7.2.png)

3. **Synthesized Printed Statistics**  
![Screenshot](images/7.3.png)

4. **Graphical Representation**  
![Screenshot](images/7.4.png)

5. **GLS Simulation Waveform**  
![Screenshot](images/7.5.png)

📌 In this waveform, the output `d` follows the combinational logic of the design.  
However, when compared to RTL simulation, the output `d` holds the value of the **previous `x`** (acting like a flop).  
This clearly demonstrates a **Synthesis-Simulation Mismatch**.
