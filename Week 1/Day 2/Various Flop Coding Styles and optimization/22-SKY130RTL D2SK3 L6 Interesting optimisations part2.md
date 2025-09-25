# ⚡ Special Case Optimisations in Digital Synthesis

This document highlights a unique case in digital hardware design where multiplication by a fixed constant can be optimised into a **zero-hardware, rewiring-only solution**.

---

## 🔢 Section 1.1: Multiplication by a Fixed Constant (9)

### Problem Definition
- **Input**: `A` → 3-bit number  
- **Output**: `Y` → 6-bit number  
- **Function**:  
Y = A * 9;

<img width="1920" height="1080" alt="Screenshot (346)" src="https://github.com/user-attachments/assets/a258c913-6139-4a45-a6dd-a5a61a0088b6" />

---

## 🧮 Section 1.2: Mathematical Decomposition

### Core Principle
The multiplication is simplified into smaller, hardware-friendly operations:

- \( 9 = 8 + 1 \)  
- Thus:  
\[
A \times 9 = (A \times 8) + (A \times 1)
\]

### Step 1: Implementing A × 8
- In binary, multiplying by 8 = left shift by 3.  
- Equivalent to appending three zeros at LSB.  
- Result:  
A * 8 → {A, 000}

### Step 2: Adding Back A
- The operation becomes:  

(A << 3) + A
- Since `A` aligns with trailing zeros, the addition is simply concatenation:

Y = {A, A}

**Important Note**:  
- `AA` does **not** mean multiplication.  
- It represents **bit concatenation** of A with itself.  
<img width="1920" height="1080" alt="Screenshot (347)" src="https://github.com/user-attachments/assets/c94a1529-8489-4823-ab23-bc9bbf7243d5" />

---

## 🛠️ Section 1.3: Hardware Implementation and Synthesis Results

### Zero-Hardware Solution
- No multipliers, adders, or gates are required.  
- Achieved entirely by **rewiring input bits**:
- Lower three bits of Y = A  
- Upper three bits of Y = A  

### Verification via Synthesis Tool
- **Test File**: `mult_8.v` (implements multiplication by 9).  
- **Result**:  
- Tool inferred **no standard cells needed**.  
- `abc` technology mapping **skipped** (no gates to map).  

### Final Netlist Extract
- The synthesis output showed:  
assign Y = {A, A};

- Confirms that `Y` is the concatenation of `A` with itself.

---<img width="1920" height="1080" alt="Screenshot (350)" src="https://github.com/user-attachments/assets/925e134d-c506-4a19-9b4c-f6e4627abb34" />


## 🗝️ Section 1.4: Key Concepts and Takeaways

- **Custom Optimisation**:  
Multiplication by **special fixed constants** can sometimes eliminate logic completely.  

- **Zero-Hardware**:  
Some operations are effectively implemented by **signal rewiring** instead of using expensive gates.  

- **Synthesis Intelligence**:  
Modern tools can **recognise and rewrite RTL equations** into efficient implementations automatically.  

- **Practical Example**:  
For `Y = A * 9` → the tool generates `assign Y = {A, A}`, achieving the goal with just wiring.

---

## ✅ Final Insight
This example demonstrates how synthesis tools apply **special-case optimisations**, leading to hardware that is:  
- Cheaper (no gates required),  
- Faster (no logic delay),  
- More power-efficient (pure wiring).  

