# ⚡ Verilog Synthesis: Inferred Latches

This document explores the **danger of incomplete conditionals (if/case statements)** in Verilog, which lead to **inferred latches**.  
The goal is to demonstrate through simulation and synthesis how missing branches in conditionals unintentionally create sequential elements.

---

## 📖 Chapter 1: Introduction to Inferred Latches

### 1.1 Session Overview
- Goal: Demonstrate how incomplete conditional statements behave in simulation and synthesis.  
- Focus: **Inferred latches** when coding mistakes occur.  
- Tools:  
  - **Simulation**: Icarus Verilog (iverilog)  
  - **Synthesis**: Yosys  

### 1.2 The Danger of Incomplete Conditionals
- **Incomplete if / case** or **partial assignments** → unintended *sequential logic inferred*.  
- Risk:  
  - Bugs in functionality  
  - Timing issues  
  - Debugging complexity  

> ⚠️ Designer intended a combinational MUX but synthesis inferred a **latch**.

---

## 🧮 Chapter 2: Case Study 1 — Simple Incomplete If (`incomplete_if.v`)

### 2.1 RTL Code and Analysis
- **Code:**  
always @(*) begin
if (I0) Y = I1;
// Missing else clause
end
- Flaw: Output `Y` undefined when `I0=0`.  
- **Expected Result**: To preserve last value → latch.

---

### 2.2 Predicted Behaviour
- **D-Latch is inferred**:
- **Enable (E):** I0  
- **Data (D):** I1  
- **Output (Q):** Y  
- Function:  
- If I0=1 → Y follows I1 (transparent).  
- If I0=0 → Y holds last value.

---

### 2.3 Simulation Verification (iverilog)
- Observations:  
- When I0=1 → Y tracks I1.  
- When I0=0 → Y **latches last value**.  
- Waveform shows Y “freezes” whenever I0 goes low.  

---

### 2.4 Synthesis Verification (Yosys)
- Yosys infers **D-latch (`$dlatch`)**.  
- Schematic matches exactly:  
- I0 → Enable  
- I1 → Data  
- Y → Q  

---

### 2.5 Conclusion
- **Result:** Intended MUX → synthesised as a latch.  
- **Lesson:** Incomplete `if` = Inferred latch.  

---

## 🧮 Chapter 3: Case Study 2 — Nested Incomplete If (`incomplete_if2.v`)

### 3.1 RTL Code and Analysis
always @(*) begin
if (I0) Y = I1;
else if (I2) Y = I3;
// Missing else clause
end

- Flaw: Y undefined if **I0=0** and **I2=0**.  
- Structure: Priority MUX → missing branch.  

---

### 3.2 Predicted Behaviour
- **Latch inferred with complex enable**:  
  - Enable = `I0 OR I2`  
  - D input = priority MUX of I1/I3 inputs  
- Latching state occurs when both I0 and I2 are 0.  

---

### 3.3 Simulation Verification (iverilog)
- Observations:  
  - I0=1 → Y=I1  
  - I0=0 & I2=1 → Y=I3  
  - I0=0 & I2=0 → Y holds previous value (latch behaviour).  

---

### 3.4 Synthesis Verification (Yosys)
- Yosys infers a **latch** driven by:  
  - **Enable** = I0 OR I2  
  - **Data Input** = MUX logic of I0/I1/I3  

---

### 3.5 Conclusion
- Both **simulation and synthesis** align with theory.  
- Rule proven: *If all conditions are not covered, synthesis infers a latch.*  

---

## ✅ Key Takeaways
- **Incomplete If/Case → Inferred Latches**
  - Simulation and synthesis confirm latching behaviour.  
- **Combinational Intentions Misinterpreted**
  - Designers often intend a MUX but accidentally infer sequential logic.  
- **Best Practices**
  - Always include a final `else` in `if` chains.  
  - Always include a `default` in `case`.  
  - Assign all outputs in all branches.  

⚠️ *Avoiding inferred latches is critical for clean, predictable, synthesizable design.*  

