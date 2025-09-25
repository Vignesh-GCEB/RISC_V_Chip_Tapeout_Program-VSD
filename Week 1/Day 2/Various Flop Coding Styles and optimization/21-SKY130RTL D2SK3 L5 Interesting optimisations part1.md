# 🔢 Synthesis Optimisation: Multiplication by Powers of Two

This lab note explores an **interesting optimisation** that occurs in RTL synthesis when multiplying a signal by powers of two.  
Instead of using expensive multiplier hardware, synthesis tools optimise the operation into **bit-shift wiring**.

---

## 📖 Chapter 1: Introduction to the Case Study

- **RTL Files Used**: `malt_2.v`, `malt_8.v`  
- **Focus of Analysis**: `malt_2.v` (multiplication by 2)  
- **Inputs/Outputs**:  
  - Input: `A[2:0]` (3-bit input)  
  - Output: `Y[3:0]` (4-bit output)  
- **RTL Relationship**:  

Y = 2 * A;


---

## 🧮 1.2 Theoretical Analysis of Multiplication by Two

### 1.2.1 Truth Table (Y = 2 × A)

| A (binary) | A (decimal) | Y (binary) | Y (decimal) |
|------------|-------------|------------|-------------|
| 000        | 0           | 0000       | 0           |
| 001        | 1           | 0010       | 2           |
| 010        | 2           | 0100       | 4           |
| 011        | 3           | 0110       | 6           |
| 100        | 4           | 1000       | 8           |
| 101        | 5           | 1010       | 10          |
| 110        | 6           | 1100       | 12          |
| 111        | 7           | 1110       | 14          |

### 1.2.2 Key Observation – Bit Shifting
- Multiplication by 2 = **append one zero at the LSB end**.  
- Mathematically equivalent to a **1-bit left shift**.  

> *"Multiplying by two is nothing but the number appended with a zero."*

---

## ⚡ 1.3 Hardware Implementation and Optimisation

### 1.3.1 Expectation vs Optimised Reality
- **Expectation**: Using a hardware multiplier circuit.  
- **Actual Reality**: No multiplier or adder required.  

### 1.3.2 Implementation via Wiring
The 4-bit output `Y` is generated as simple shifted wiring:

Y[3:1] = A[2:0]
Y = 1'b0

### 1.3.3 Generalised Principle
- Multiply by 2 → append **1 zero**  
- Multiply by 4 → append **2 zeros**  
  - Example: `5 (101) × 4 = 20 (10100)`  
- Multiply by 8 → append **3 zeros**  

---

## 🛠️ 1.4 Synthesis Tool Verification

### 1.4.1 Commands Used

read_liberty <library_file.lib>
read_verilog malt_2.v
synth -top malt_2


### 1.4.2 Synthesis Results
- **Tool Output**: *“No cells inferred”*  
- No multipliers, adders, or logic gates were instantiated.  
- Netlist shows:  

assign Y = {A, 1'b0};

- Running `abc` (mapping) gave an error → not needed since no standard cells exist.

### 1.4.3 Conclusion
- Multiplication by **powers of two** is optimised to **bit-shifts** or **concatenation**, not real multipliers.  
- Saves hardware, area, and power.  
- Example of **intelligent optimisation in RTL synthesis**.

---

## ✅ Key Takeaways
- `Y = 2 * A` → equivalent to `{A, 1'b0}` (append one bit zero).  
- Multiplication by `2ⁿ` → append `n` zeros.  
- Synthesis tools replace resource-heavy multipliers with **efficient wiring logic**.  
- This is a **special-case optimisation** widely exploited in digital design.  
