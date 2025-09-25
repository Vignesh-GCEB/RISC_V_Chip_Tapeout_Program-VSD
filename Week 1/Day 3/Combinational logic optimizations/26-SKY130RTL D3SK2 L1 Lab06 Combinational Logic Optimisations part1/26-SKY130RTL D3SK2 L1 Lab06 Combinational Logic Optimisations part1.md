# 🔧 Combinational Logic Optimisations (Part 1)

This lab explores **combinational logic optimisations** using Verilog designs and synthesis tools.  
The exercises demonstrate how synthesis tools simplify Boolean expressions and map them into efficient hardware implementations.

---

## 📖 1.0 Introduction to the Lab

- **Topic**: Combinational logic optimisation  
- **Files Used**:  
  - `opt_check.v`  
  - `opt_check2.v`  
  - (and others: opt_check3.v, etc.)  

All files are located in the Verilog files directory.

---<img width="1920" height="1080" alt="Screenshot (357)" src="https://github.com/user-attachments/assets/33c7ed71-e2cd-44ff-859c-9f4d9ca009fa" />


## 🧮 2.0 Case Study 1: `opt_check.v`

### 2.1 Module Description
- Behaviour: Output `Y` depends on input `A`:  
  - If `A = 1` → `Y = B`.  
  - If `A = 0` → `Y = 0`.  
- Equivalent to a **2x1 multiplexer (MUX)**.  

### 2.2 Boolean Logic Simplification
Logic expression:  
\[
Y = (\bar{A} \cdot 0) + (A \cdot B)
\]  
Simplifies to:  
\[
Y = A \cdot B
\]  
**Expected Result**: A simple **2-input AND gate**.  

### 2.3 Synthesis and Verification
**Workflow**:  
Invoke Yosys
yosys

Load standard cell library
read_liberty -lib .../my_lib.lib

Read Verilog design
read_verilog opt_check.v

Synthesis top-level
synth -top opt_check

Optimisation
opt_clean -purge

Technology mapping
abc -liberty .../my_lib.lib

Visualisation
show


**Result**:  
- Tool simplified design into a **2-input AND gate**.  
- Matches expected outcome. ✅  
<img width="1920" height="1080" alt="Screenshot (358)" src="https://github.com/user-attachments/assets/c8a540e7-f91f-49df-9313-df4bda13d5ec" />

---

## 🧮 3.0 Case Study 2: `opt_check2.v`

### 3.1 Module Description
- Behaviour:  
  - If `A = 1` → `Y = 1`.  
  - If `A = 0` → `Y = B`.  

### 3.2 Boolean Logic Simplification
Logic expression:  
\[
Y = (\bar{A} \cdot B) + (A \cdot 1)
\]  
Simplifies to:  
\[
Y = \bar{A}B + A
\]  
Further simplification:  
\[
Y = A + B
\]  

**Target Result**: A simple **2-input OR gate**.  

> **Note**: The reduction from \(\bar{A}B + A \to A + B\) is a Boolean simplification identity, often called the **Absorption Law** (or Adjacency Law).

### 3.3 Synthesis and Verification
**Workflow**: Same steps (read_verilog → synth → opt_clean → abc → show).  

**Result**:  
- The tool did **not directly instantiate a standard OR gate**.  
- Instead, it produced a **non-inverting realisation of OR**.  
<img width="1920" height="1080" alt="Screenshot (359)" src="https://github.com/user-attachments/assets/5010eaa6-1e78-458b-8902-a20676d6753d" />

### Reason for Discrepancy
- In CMOS logic:  
  - A typical OR implementation = **NOR + inverter**.  
  - A standard NOR gate requires **stacked PMOS transistors** (high resistance, slow performance).  
- To avoid performance issues:  
  - The synthesis tool selected an **alternative gate-level structure** (non-inverting OR).  
- **Outcome**: Equivalent logic → more **optimal CMOS performance**.  

---

## ✅ Key Takeaways

- **Case 1 (`opt_check.v`)**: Yosys optimisation shrinks design → `AND gate`.  
- **Case 2 (`opt_check2.v`)**: Yosys avoids slow NOR+inverter construction → implements OR using *non-inverting OR gate realisation*.  
- **Boolean identity (Absorption Law)** helps justify simplification.  
- **CMOS awareness**: Optimisations consider **transistor-level efficiency**, not just Boolean correctness.  
