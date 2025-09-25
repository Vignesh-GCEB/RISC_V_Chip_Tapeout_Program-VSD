# Synthesis Lab: Hierarchical vs Flat Synthesis using Yosys

## 📖 Description
This project demonstrates the difference between **hierarchical synthesis** and **flat synthesis** using the Yosys synthesis tool. The `synth -top` option is used to preserve hierarchy during synthesis and observe how the generated netlist changes.

---

## 🎯 Objective
- Understand hierarchical vs flat synthesis in Yosys  
- Learn how hierarchy is preserved in synthesized netlists  
- Explore technology mapping with the Sky130 library  
- Visualize the hierarchy using the Yosys `show` command  

---

## 📝 Design Under Test (DUT)

**File:** `multiple_modules.v`  

### Design Modules
- `sub_module_one` → Implements an AND gate  
- `sub_module_two` → Implements an OR gate  
- `multiple_module` (Top-level) → Instantiates both submodules  

### Signal Connectivity
- Inputs: `a`, `b`, `c`  
- Output: `y`  
- Flow: `(a, b) → AND (U1) → OR (U2) with input c → y`  

---

## ⚙️ Tool Flow (Yosys Commands)

Launch Yosys
yosys

Load Liberty Technology Library
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib

Load Verilog design
read_verilog multiple_modules.v

Perform synthesis (preserve hierarchy)
synth -top multiple_module

Map logic to Sky130 technology cells
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib


---

## 📊 Synthesis Results

- `sub_module_one` → AND gate  
- `sub_module_two` → OR gate (but optimized)  
- `multiple_module` → Contains U1 and U2  
- **Total cells**: 2  

---

## 🔍 Visualization


- **Expected view:** AND feeding OR  
- **Observed view:** U1 and U2 preserved as separate blocks  
- **Insight:** Hierarchical boundaries preserved  

---

## 🗂 Netlist Output
write_verilog -noattr multiple_modules_hier.v


### Characteristics
- Netlist contains: `multiple_module`, `sub_module_one`, `sub_module_two`  
- AND gate mapped properly  
- OR gate implemented as **NAND + inverters** (optimization)  

---

## 🧮 Logic Optimisation

### De Morgan’s Theorem
\[
(A' \cdot B')' = A + B
\]  

- NAND with inverted inputs produces OR  
- Inverters + NAND bubble cancel → correct OR implementation  

---

## ⚡ CMOS Considerations
- CMOS logic is **naturally inverting**  
- OR/AND gates often realized with NAND/NOR + inverter  
- **NAND vs NOR:**  
  - NAND → stacked NMOS (efficient, faster)  
  - NOR  → stacked PMOS (slower, larger area)  

### Conclusion
The synthesis tool selects a **NAND-based OR implementation** for efficiency in CMOS design.  

---

## ✅ Key Takeaways
- Yosys synthesis **preserves hierarchy** when `synth -top` is used  
- Tools optimize logic for CMOS efficiency, not strict one-to-one mapping  
- Technology library (.lib) heavily influences the final implementation  


