# ⚠️ Synthesis-Simulation Mismatch: The Blocking Statement Caveat

This document explains how **incorrect use of blocking statements (`=`)** in Verilog can cause **mismatches** between RTL simulation and Gate-Level Simulation (GLS) / synthesised hardware.  

File used: `blocking_caveat.v`

---

## 📖 Section 1.1: The Blocking Statement Caveat

### 1.1.1 Introduction
- Common issue in digital design: **synthesis-simulation mismatch**.  
- Mismatch occurs when RTL sim ≠ Synthesis hardware behaviour.  
- **Cause**: Using blocking statements (`=`) incorrectly inside `always` blocks.  
- **Objective**: Show how blocking assignments can mislead simulation.  
<img width="1920" height="1080" alt="Screenshot (394)" src="https://github.com/user-attachments/assets/38c76e35-4d1a-4278-aec6-ba2b06a91edc" />

---

### 1.1.2 Intended Logic Design
- **Target Boolean Expression**:  
  \[
  D = (A \; OR \; B) \; AND \; C
  \]
- Implementation:  
  - OR gate for `(A OR B)`  
  - Output fed into AND gate with `C`  
<img width="1920" height="1080" alt="Screenshot (395)" src="https://github.com/user-attachments/assets/d91901b0-be9e-4e87-94d7-33a7acdbeced" />

---

### 1.1.3 Verilog Implementation and Flaw
always @(*) begin
d = x & c; // uses x before it is upda
ed x = a | b; // blocking statements executed in

- **Flaw**: Blocking (`=`) executes sequentially inside block.  
- At evaluation, `d` uses the **old value of x** from previous cycle.  
- **Effect**: Creates unintended memory (looks like a flop).  

---
<img width="1920" height="1080" alt="Screenshot (396)" src="https://github.com/user-attachments/assets/44b2a368-9416-4667-bb04-d7da0000ff49" />

## 🧮 Section 1.2: Simulation vs Synthesis

### 1.2.1 RTL Simulation Behaviour
- **Observation**: Waveform shows incorrect sequential-like behaviour.  
- Example:  
  - When `A=0, B=0, C=1`, expected `(A|B)&C = 0`.  
  - RTL Simulation → D = 1 (incorrect).  
- **Reason**: D was computed using old/stale `x`.  
- **Conclusion**: RTL sim looked as if `x` was a register → an "imaginary flop".  

---
<img width="1920" height="1080" alt="Screenshot (397)" src="https://github.com/user-attachments/assets/4a48a563-c180-42b7-b618-fcbe02f9a3b0" />

### 1.2.2 Synthesis and Netlist Analysis
- **Synthesiser Result**:  
  - Pure combinational circuit.  
  - LHS of netlist: **1 OR gate + 1 AND gate**.  
  - ✅ No register/latch inferred.  
- **Conclusion**: Synth tool correctly interprets intent as combinational.  

---

### 1.2.3 Gate-Level Simulation (GLS) Behaviour
- **GLS Waveform**: Matches intended combinational logic.  
- Example:  
  - If `A=0, B=0, C=1` → D=0 (correct).  
  - Output reflects instantaneous inputs → no storage element.  
- ✅ Confirms mismatch cause is only in **RTL sim**.  

---

## 📝 Section 1.3: Conclusions and Best Practices

### 1.3.1 The Mismatch Explained
- **RTL Simulation**: Incorrect "sequential-like" behaviour.  
- **GLS / Hardware**: Correct combinational behaviour.  
- **Root Cause**: Misuse of blocking statements → synthesis ignores sequencing, but simulator enforces it.  

---

### 1.3.2 Dangers and Industry Impact
- Can mislead designers into thinking a **flop exists** in RTL.  
- Synthesis removes it → actual chip behaviour differs.  
- Late discovery → "huge churn" in design cycles + costly rework.  

---

### 1.3.3 Recommendations
- **Golden Rule**:  
  > "You should be very, very careful with blocking statements."  
- **Avoid** `=` in most cases, especially in sequential/RTL modelling.  
- Prefer **non-blocking (`<=`)** for sequential logic.  
- Use blocking only for carefully checked combinational logic.  
- Sketch design behaviour on paper → ensure clarity before coding.  

---

## ✅ Final Takeaway
- Blocking assignments can **mislead simulation** while synthesis infers correct hardware.  
- Always verify with **GLS** to catch such mismatches.  
- Follow best practices: `always @(*)` + non-blocking for sequential logic.  

