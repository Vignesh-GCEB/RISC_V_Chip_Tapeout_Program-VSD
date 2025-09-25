# ⚙️ Synthesis of D Flip-Flop (DFF) Circuits with Yosys

This document describes the synthesis process for **D-type flip-flops (DFFs)** using the Yosys tool.  
It demonstrates how Verilog descriptions of DFFs with **asynchronous reset**, **asynchronous set**, and **synchronous reset** are mapped to a standard cell library (Sky130), along with optimisations performed during synthesis.  

---

## 📚 1.1 The Yosys Synthesis Flow

The standard procedure for synthesising Verilog circuits with Yosys:

1. **Read Liberty File**  
   Load the technology library containing logic and flip-flop standard cells.  
\
read_liberty -lib <path_to_lib_file>


2. **Read Verilog Design**  

read_verilog <design_file.v>

3. **Set the Top Module**  

dfflibmap -liberty <path_to_lib_file>

5. **Optimise and Map Logic**

abc -liberty <path_to_lib_file>

6. **Visualise Circuit (Optional)**  
show


---

## 🔄 1.2 Case Study 1: DFF with Active-High Asynchronous Reset

- **Goal**: Create a flip-flop that resets to `0` when `reset = 1`.  
- **Library Constraint**: Sky130 library DFF cell has an **active-low reset pin**.  
- **Synthesis Result**:  
- Yosys instantiates a DFF with active-low reset.  
- Inserts an **inverter** in the reset path to match Verilog specification.  

**Logic Explanation**:  
- Verilog expects active-high reset.  
- Library DFF requires active-low.  
- Inverter ensures:  
reset (high) → inverter → reset_n (low) → DFF reset asserted

- Demonstrates **signal polarity adaptation** using available standard cells.

---

## 🔼 1.3 Case Study 2: DFF with Active-High Asynchronous Set

- **Goal**: Create a flip-flop that sets `Q = 1` when `set = 1`.  
- **Library Constraint**: DFF available with **active-low set pin**.  
- **Synthesis Result**:  
- Yosys instantiates active-low set DFF.  
- Adds an **inverter** to invert the active-high set signal.  

**Logic Explanation**:  
- RTL: `set = 1` → force Q = 1.  
- Library: Requires `set_n = 0` for set.  
- Tool fixes mismatch using inverter.  

---

## ⏳ 1.4 Case Study 3: DFF with Synchronous Reset

- **Goal**: Reset occurs **only on the clock edge**.  
- **Synthesis Behaviour**:  
- Tool instantiates a **plain DFF** (with no async pins).  
- Implements synchronous reset logic *outside* flip-flop, feeding the D-input.  
- Circuit uses an **AND gate with inverted reset input**.  

### Logic Equivalence

- **RTL Intent (The Wish)**:  
Defined by a 2-to-1 multiplexer at the D-input:  

Qnext = (sync_reset ? 0 : D)

Expression:  
\[
Qnext = \overline{sync\_reset} \cdot D
\]

- **Synthesised Result (What We Got)**:  
- AND gate with `D` and `not(sync_reset)` as inputs.  
- Expression:  
\[
Qnext = \overline{sync\_reset} \cdot D
\]

- **Conclusion**: Both RTL and gate-level netlist are logically equivalent.  
Yosys optimised the MUX logic into the simpler **AND gate implementation**.  

---

## 📊 Summary Table of Case Studies

| Case Study                     | RTL Intent                  | Library Limitation   | Tool Action                         |
|--------------------------------|------------------------------|----------------------|-------------------------------------|
| Async Reset (active-high)      | Reset = 1 → Q = 0           | Only active-low DFF  | Inserted inverter in reset path     |
| Async Set (active-high)        | Set = 1 → Q = 1             | Only active-low DFF  | Inserted inverter in set path       |
| Sync Reset (edge-triggered)    | Reset only on clock edge    | Basic DFF (no reset) | Used AND gate before D-input        |

---

## ✅ Key Takeaways
- Yosys can **adapt RTL intent** to available library cells using additional gates (inverters or logic).  
- **Asynchronous controls** (set/reset) are implemented using library DFF pins directly (with polarity adjustments if needed).  
- **Synchronous resets** are realised using **combinational logic feeding the D-input** (logic optimisation reduces MUX → AND).  
- This highlights how **synthesis tools bridge the gap between RTL intent and physical cell availability**.  
