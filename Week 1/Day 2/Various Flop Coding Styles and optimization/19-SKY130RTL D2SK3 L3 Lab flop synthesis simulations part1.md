# 🧪 Lab Notes: Simulating Flip-Flop Behaviour

This lab focuses on simulating different **D-type flip-flop (DFF)** designs and observing their functionality under asynchronous and synchronous reset conditions.  
Simulations are carried out using **iVerilog** and waveforms are analysed in **GTKWave**.

---

## 📖 Chapter 1: Introduction and Session Overview

### 1.1 Recap of Previous Concepts
- Review of flip-flop coding styles.  
- Implementations with **asynchronous reset**, **asynchronous set**, and **synchronous reset**.  
- Purpose and role of flip-flops in digital circuits.  

### 1.2 Lab Objectives and Setup
- **Objective**:  
  Simulate DFF designs to verify their theoretical behaviour.  
- **Simulation Tools**:  
  - iVerilog (logic simulation)  
  - GTKWave (waveform viewing)  
- **Design Files**:  
  - `dff_asynchronous_reset.v`  
  - `dff_asynchronous_set.v`  
  - `dff_sync_reset.v`  
- **Testbench Naming Convention**:  
  - Prefix `tb_` for testbenches.  
  - Example: Testbench for `dff_async_reset.v` → `tb_dff_async_reset.v`.  

---

## 🔄 Chapter 2: D-Flip-Flop with Asynchronous Reset

### 2.1 Simulation Analysis
- **Normal Operation**:  
  - `Q` updates to `D` only at the clock edge.  
- **Asynchronous Reset Behaviour**:  
  - On reset assertion, `Q` is **immediately forced low (0)**.  
  - No dependence on clock – reset overrides instantly.  
- **Observation Quote**:  
  *"The asynchronous reset is not awaiting the clock edge. It is immediately making Q go low. This is clearly seen in the waves."*  

### 2.2 Summary of Observations
- Data Path (`D → Q`): **Synchronous**  
- Reset Path: **Asynchronous**, bypasses the clock  

---

## 🔼 Chapter 3: D-Flip-Flop with Asynchronous Set

### 3.1 Simulation Analysis
- **Normal Operation**:  
  - `Q` follows `D` only on active clock edge.  
- **Asynchronous Set Behaviour**:  
  - On set assertion, `Q` is **immediately forced high (1)**.  
  - Independent of `D` and clock.  
  - While set is active → Q stays at 1 even if `D=0` at clock edge.  
- **Return to Normal Operation**:  
  - When set de-asserts, DFF resumes normal operation.  
- **Observation Quote**:  
  *"Till the time asynchronous set is present, the output will be 1 only."*  

### 3.2 Summary of Observations
- Data Path (`D → Q`): **Synchronous**  
- Set Path: **Asynchronous**, forces Q = 1  

---

## 🔁 Chapter 4: D-Flip-Flop with Synchronous Reset

### 4.1 Simulation Analysis
- **Synchronous Reset Behaviour**:  
  - When reset is asserted, Q does **not** change immediately.  
  - Reset takes effect only at the **next rising clock edge**.  
- **Priority over Data**:  
  - If `sync_reset=1` and `D=1` on clock edge, reset has **higher precedence**.  
  - Q = 0 (reset dominates).  
- **Coding Style Influence**:  
  - `if (sync_reset)` check comes first inside `always @(posedge clk)` block, ensuring reset priority.  
- **Observation Quote**:  
  *"The output Q is not going low immediately, it is waiting for the subsequent clock edge and then going low – meaning reset is applied upon clock edge."*  

### 4.2 Summary of Observations
- Data Path (`D → Q`): **Synchronous**  
- Reset Path: **Synchronous**, only at active clock edge  

---

## 📌 Chapter 5: Conclusion and Next Steps

- **Verification**:  
  Simulations match the **intended Verilog code behaviours**.  
- **Glitch-Free Behaviour**:  
  - Async reset/set → immediate effect.  
  - Sync reset → clock-controlled, safe for timing.  
- **Next Steps**:  
  - Extend lab to include **flip-flop initialisation** (set/reset interactions).  
  - Compare with **latch-based implementations**.  

---

## ✅ Key Takeaways
- **Asynchronous controls**: Work instantly, independent of clock.  
- **Synchronous controls**: Only active on clock edge, good for stable designs.  
- Testbenches confirm theory → simulation waveforms reflect **expected behaviour**.  
