# 🔀 Verilog Synthesis: `if` and `case` Constructs

This document explores how **if** and **case** statements are used in Verilog and how they are synthesised into hardware. It also highlights **pitfalls** like inferred latches and overlapping conditions.

---

## 📖 1. The `if` Statement

### 1.1 Purpose and Hardware Implementation
- **Use**: Creates **priority logic** in Verilog.  
- **Behaviour**:  
  - `if` condition checked first (highest priority).  
  - `else if` checked only if all previous are false.  
  - Final `else` executes if none match.  
- **Synthesis**:  
  - Synthesises into a **chain of multiplexers (MUX)**.  
  - Each `if` branch = MUX level.  
  - Priority logic structure.
<img width="1920" height="1080" alt="Screenshot (398)" src="https://github.com/user-attachments/assets/f5574f74-6463-48ac-90f1-a6907e657eb4" />

---

### 1.2 Danger: Inferred Latches
- **Definition**: A latch unintentionally created when outputs are not assigned for all conditions.  
- **Cause**: **Incomplete `if` structure** without a final `else`.  
- **Mechanism**:  
  - Example:  
    ```
    if (c1) Y = A;
    else if (c2) Y = B;
    // Missing else...
    ```
  - If both c1 and c2 fail → Y not assigned.  
  - Synth tool infers **latch** to "hold previous value".  
- **Latch Enable** = OR of all specified conditions.
<img width="1920" height="1080" alt="Screenshot (399)" src="https://github.com/user-attachments/assets/546dda32-ca8d-42b3-b6b9-b5c7e1b3e400" />

⚠️ *Inferred latches are almost always undesirable for combinational logic.*

---
case (sel)
2'b00: y =
; 2'b01: y
b; 2'b10:
= c; default: y = 0; //

---
<img width="1920" height="1080" alt="Screenshot (400)" src="https://github.com/user-attachments/assets/fa7bb04a-960c-4c8f-82d0-3974bbafd6e6" />

### 2.3 Caveat 2: Partial Assignments
- **Definition**: Assigning some outputs but not all in every case branch.  
- **Example**:
case (sel)
2'b00: x = a; // y not assigned → y latched
2'b01: y = b; // x not assigned → x latched
endcase
- **Solution**: Assign **all outputs** in **every branch**.  

> Rule of Thumb: *“Assign all signals in all case items (including `default`).”*  

---
<img width="1920" height="1080" alt="Screenshot (401)" src="https://github.com/user-attachments/assets/70d28647-1810-4ce4-846d-c7f3517edf3c" />

### 2.4 Caveat 3: If vs Case & Overlapping Cases
- **if-else if**: Priority based → once a condition passes, no further checks.  
- **case**: Parallel MUX-like structure.  
- **Danger**: *Overlapping case items*.  
- If multiple match → unpredictable results.  
- Simulator checks sequentially, but synthesis may optimise differently.  
- ✅ **Rule**: Case items must be **mutually exclusive**.
<img width="1920" height="1080" alt="Screenshot (403)" src="https://github.com/user-attachments/assets/b0390ea2-83bf-43bd-a015-8958052ef848" />

---

## ✅ Best Practices
<img width="1920" height="1080" alt="Screenshot (404)" src="https://github.com/user-attachments/assets/e2840d16-5cb3-407e-bb84-8a8f1ad14b0b" />

- For **combinational logic**:  
- Always complete `if`/`case` → no missing branch.  
- Always assign all outputs.  
- For **sequential logic**:  
- Use **non-blocking (`<=`)** for registers.  
- Incomplete `if` allowed to model “hold previous state”.  
- Always include a `default` case in `case` statements.  
- Be careful with **overlapping `case` options**.  
<img width="1920" height="1080" alt="Screenshot (405)" src="https://github.com/user-attachments/assets/ca27ade1-44f2-4a21-9c21-0e1ea9eda7c3" />

