# 🧪 Lab 07: Sequential Logic Optimisations (Part 3)

This lab focuses on the analysis of **DFF_const_3** — a design where sequential logic tied to constants is **not optimised away**.  
It demonstrates why some flops remain in the design despite constant inputs and how synthesis tools handle set/reset behaviour.

---

## 📖 1.0 Case Study: `DFF_const_3`

### 1.1 Simulation Analysis
The first step is to simulate the design and view its internal behaviour.  

**Commands:**
iverilog DFF_const_3.v dff_const_3_tb.v
./a.out

**Waveform Observations:**
1. `Q1` → transitions to `1` on the first **rising edge of clk** after reset de-asserts.  
2. On **the same clock edge** that `Q1=1`, the main output `Q` → goes **low (0)**.  
3. On the **next rising clk edge**, Q transitions back to `1`.  

**Simulation Conclusion:**
- The output `Q` generates a **1-cycle pulse** (low for exactly one clock period).  
- This pulse coincides with the exact clock cycle where `Q1` rises.  

---

### 1.2 Synthesis Analysis

**Tool:** Yosys  

**Commands:**

read_liberty -lib ./mylib_lib/sky130.lib
read_verilog DFF_const_3.v
synth -top DFF_const_3
dfflibmap -liberty ./mylib_lib/sky130.lib
abc -liberty ./mylib_lib/sky130.lib
show


**Synthesis Results:**
- **Flops inferred:** Statistics confirm that **two DFF cells** are created.  
- **Netlist structure:**  
  - Flop B → Reset flop (`RESET_B`).  
  - Flop A → Set flop (`SET_B`).  
  - Q1 (`Flop B.Q`) → feeds directly into D of Flop A.  
- **Active-Low Logic Handling:**  
  - Standard cells in sky130 use **active-low reset/set pins** (`RESET_B`, `SET_B`).  
  - Since RTL used active-high reset/set, **inverters were auto-inserted** in the reset/set paths.  
- **Confirmation:** The synthesised netlist structure and waveform behaviour **match perfectly**.  

---

## 🔑 2.0 Key Principles and Conclusion

- **Core Principle:**  
  “Not every flop with a constant D input will be optimised away.”  

- **Why?**  
  Optimisation depends on whether **output Q truly stabilises at a constant value** after reset/set.  

- **In this case (`DFF_const_3`)**:  
  - Q1 transitions from 0→1 after reset.  
  - Q toggles (1 → 0 → 1).  
  - Therefore neither is a constant, so both **flops remain** in the circuit.  

- **Critical Insight:**  
  When analysing sequential logic for optimisation:  
  - Check **D input value**  
  - Check **set/reset polarity and behaviour**  
  - Verify if Q ever changes across cycles.  
  - Only if Q is truly constant can the flop be removed.  

---

## 🧪 3.0 Lab Exercises

To reinforce learning, students should:  

1. **Analyse, simulate, synthesise:**  
   - `DFF_const_4.v`  
   - `DFF_const_5.v`  

2. **Re-run** analysis for all previous designs:  
   - `DFF_const_1.v`  
   - `DFF_const_2.v`  
   - `DFF_const_3.v`  

✅ Compare simulation vs synthesis results and confirm which flops truly collapse to constants and which remain.  

---

## 📌 Final Takeaway

- Constant inputs to flops do **not always guarantee** optimisation.  
- Carefully analyse clock/reset/set interactions to determine sequential constants.  
- Both **simulation (iverilog, gtkwave)** and **synthesis (yosys)** are essential for verifying such optimisations.  
