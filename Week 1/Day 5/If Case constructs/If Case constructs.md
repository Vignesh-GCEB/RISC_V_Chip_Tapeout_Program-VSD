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

⚠️ *Inferred latches are almost always undesirable for combinational logic.*

---
case (sel)
2'b00: y =
; 2'b01: y
b; 2'b10:
= c; default: y = 0; //

---

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

### 2.4 Caveat 3: If vs Case & Overlapping Cases
- **if-else if**: Priority based → once a condition passes, no further checks.  
- **case**: Parallel MUX-like structure.  
- **Danger**: *Overlapping case items*.  
- If multiple match → unpredictable results.  
- Simulator checks sequentially, but synthesis may optimise differently.  
- ✅ **Rule**: Case items must be **mutually exclusive**.

---

## ✅ Best Practices

- For **combinational logic**:  
- Always complete `if`/`case` → no missing branch.  
- Always assign all outputs.  
- For **sequential logic**:  
- Use **non-blocking (`<=`)** for registers.  
- Incomplete `if` allowed to model “hold previous state”.  
- Always include a `default` case in `case` statements.  
- Be careful with **overlapping `case` options**.  

