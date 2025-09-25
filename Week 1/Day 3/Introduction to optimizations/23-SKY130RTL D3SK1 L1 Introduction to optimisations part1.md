# 🧩 Introduction to Logic Optimisation

This module explores **combinational logic optimisation** in digital design. Optimisation simplifies circuits, reducing **area usage** and **power consumption**, while maintaining functional correctness.

---

## 📖 Chapter 1: Overview of Logic Optimisation

### 1.1 Purpose
- **Primary Goal**: *"Squeeze the logic to get the most optimized design."*  
- **Benefits**: Reduced circuit **area** and lower **power consumption**.  
- **Scope**: Focuses on **combinational logic optimisation** (not sequential).  

### 1.2 Key Optimisation Techniques
1. **Constant Propagation**  
   - Simplifies logic when inputs are tied to constants (0 or 1).  

2. **Boolean Logic Optimisation**  
   - Simplifies complex Boolean expressions into efficient forms.  
   - Common methods: **Karnaugh maps (K-maps)**, **Quine-McCluskey algorithm**.  
<img width="1920" height="1080" alt="Screenshot (351)" src="https://github.com/user-attachments/assets/326a5530-fb1d-4f3d-bb67-2e8c4bb7c961" />

---

## 🔄 Chapter 2: Constant Propagation in Detail

### 2.1 Example: AND-OR-INVERT Gate Simplification

**Original Function**:  
\[
Y = (A \cdot B + C)^\prime
\]  

**CMOS Implementation**:  
- Requires **6 MOS transistors**.  
- Pull-down NMOS: A and B (series), in parallel with C.  
- Pull-up PMOS: Dual configuration.  

**Applying Constant Propagation (A = 0)**:  
1. \( Y = (0 \cdot B + C)^\prime \)  
2. \( Y = (0 + C)^\prime \)  
3. \( Y = C^\prime \)  

**Result**:  
- The complex gate reduces to a **simple inverter**.  
- Inverter requires **only 2 transistors**.  

**Optimisation Advantage**:  
- Less **area**  
- Less **power**  
- Demonstrates how constants simplify logic *“downstream”*.  
<img width="1920" height="1080" alt="Screenshot (352)" src="https://github.com/user-attachments/assets/8c67f505-6b29-410b-bb29-2896254cfdf1" />

---

## 🔀 Chapter 3: Boolean Logic Optimisation in Detail

### 3.1 Example: Simplifying a Mux-Based Expression

**Original RTL Expression** (nested ternary operators):  
assign Y = (A == 0) ? C_bar : ((B == 1) ? C : ((C == 1) ? A : 0));

**Step-by-Step Simplification**:
1. **Innermost Mux**: \(((C == 1) ? A : 0) \Rightarrow AC\)  
2. **Middle Mux**: \(((B == 1) ? C : AC) \Rightarrow B \cdot C + \bar{B}\cdot AC\)  
3. **Outermost Expression**:
   \[
   Y = \bar{A}\cdot \bar{C} + A(B \cdot C + \bar{B}\cdot AC)
   \]

**Boolean Algebra Reduction**:
- Distribute \( A \):  
  \[
  Y = \bar{A}\cdot \bar{C} + ABC + A\bar{B}C
  \]
- Factor \( AC \):  
  \[
  Y = \bar{A}\cdot \bar{C} + AC(B + \bar{B})
  \]
- Simplify \( (B + \bar{B}) = 1 \):  
  \[
  Y = \bar{A}\cdot \bar{C} + AC
  \]

**Final Result**:  
\[
Y = \bar{A}\cdot \bar{C} + AC
\]  

- This is the definition of an **XNOR gate**:  
assign Y = ~(A ^ C);


**Conclusion**:  
A highly complex nested expression reduces to a simple **2-input XNOR gate**.  

---

### 3.2 Synthesis and Automation
- This manual simplification mirrors **K-map reduction** and **Boolean algorithms**.  
- **Modern Synthesis Tools** (like Yosys, Synopsys DC, Cadence Genus, etc.) perform such optimisations automatically, mapping RTL to minimal, efficient hardware.  
<img width="1920" height="1080" alt="Screenshot (353)" src="https://github.com/user-attachments/assets/cb8aeb0f-91cf-429c-bc21-23040990fdd5" />

---

## ✅ Key Takeaways
- **Constant propagation** reduces gates if inputs are fixed.  
- **Boolean algebra optimisations** collapse complex logic into simple gates (e.g., XNOR).  
- **Tools automate optimisations**, ensuring best trade-offs in area and power.  
- Optimisation is critical for energy-efficient and space-optimised VLSI designs.  
