# 🧪 Gate Level Simulation (GLS) and Optimisation

This document introduces **Gate Level Simulation (GLS)** and common **synthesis-simulation mismatches**.  
GLS uses the synthesised netlist to validate both logical correctness and (optionally) timing, helping catch issues that may not appear at the RTL simulation stage.

---
<img width="1920" height="1080" alt="Screenshot (379)" src="https://github.com/user-attachments/assets/8ba262db-6781-4f63-be17-96d4844c6f73" />

## 📖 Chapter 1: Gate Level Simulation (GLS)

### 1.1 What is GLS?
- **Definition**: Simulation using the *synthesised netlist* as Design Under Test (DUT), instead of the original RTL.  
- **Key Point**: Test benches remain the same because RTL and netlist have identical I/O interfaces.  
- **Concept**: RTL DUT → replaced by gate-level netlist DUT.  

---

### 1.2 Why GLS is Necessary
1. **Verify Logical Correctness**  
   - Detect synthesis-simulation mismatches.  
   - Ensure the netlist behaves identically to the RTL.  

2. **Verify Timing**  
   - Unlike RTL simulation, netlist-level simulation considers **delays**.  
   - Setup/hold times and other constraints can be verified.  
   - Requires **timing-aware GLS** with SDF (Standard Delay Format) annotations.  
   - **Note**: Current focus = functional GLS, not timing-aware simulation.  

---<img width="1920" height="1080" alt="Screenshot (380)" src="https://github.com/user-attachments/assets/26670736-a757-4d8e-8d70-ca2f97ef69f8" />


### 1.3 GLS Setup and Flow

**RTL Simulation Flow**:
Design (RTL Code) + Test Bench
→ iVerilog Simulator
→ VCD Waveform
→ GTKWave


**GLS Flow**:
Design (Synthesised Netlist) + Test Bench + Gate-Level Verilog Models
→ iVerilog Simulator
→ VCD Waveform
→ GTKWave

- **Role of Gate-Level Models**:  
  - Netlist instantiates library cells (e.g., AND2_X1, DFF_X1).  
  - Simulator cannot interpret these directly.  
  - Requires gate-level Verilog models that define functional behaviour of standard cells.  

---<img width="1920" height="1080" alt="Screenshot (381)" src="https://github.com/user-attachments/assets/d56d14ac-d60b-40fa-995c-25467ac1ae62" />


## 📖 Chapter 2: Synthesis-Simulation Mismatches

### 2.1 What is a Mismatch?
- Occurs when RTL simulation behaviour ≠ Synthesis Netlist behaviour.  
- **Causes**:  
  1. Missing sensitivity list.  
  2. Incorrect assignment type (`=` vs `<=`).  
  3. Non-standard/verilog-specific constructs.  
- **Role of GLS**: Detect and debug these mismatches.  

---

### 2.2 Cause 1: Missing Sensitivity List

- **Simulator**: Activity-driven → executes block only if signals in sensitivity list change.  
- **Problem**: If an input is omitted, RTL simulation may “hold” old values.  

**Example: 2x1 MUX**
<img width="1920" height="1080" alt="Screenshot (382)" src="https://github.com/user-attachments/assets/daddbcd6-d732-4d5b-b043-7609fb73eb92" />


// ❌ Incorrect
always @(select) begin
if (select) Y = I1;
else Y = I0;
end

- **Simulation Behaviour**:  
  - Y updates only when `select` changes.  
  - If I0/I1 toggle while select is stable → Y is stale.  
  - Appears like a **latch**.  

- **Synthesis Behaviour**:  
  - Synth tool infers proper combinational mux (`Y = select ? I1 : I0`).  

✅ **Solution**: Use **always @(*)**  

// ✅ Correct
always @(*) begin
if (select) Y = I1;
else Y = I0;
end

---

### 2.3 Cause 2: Blocking vs Non-Blocking Assignments

- **Blocking (`=`)**  
  - Sequential behaviour, like C statements.  
  - Evaluates one line after another.  

- **Non-Blocking (`<=`)**  
  - Parallel behaviour.  
  - Evaluates all RHS first, then updates LHS.  
  - Independent of order.  
<img width="1920" height="1080" alt="Screenshot (383)" src="https://github.com/user-attachments/assets/418a79df-23b6-4298-acc9-eaba341bcc93" />

---

#### Caveat 1: Sequential Logic (Shift Register)

**Goal**: 2-stage register (D → Q0 → Q).  

// ❌ Blocking Example
always @(posedge clk) begin
Q0 = D;
Q = Q0; // Gets UPDATED Q0 => both Q and Q0 = D same cycle
end
- **Error**: Collapses to single-stage register.  

// ✅ Non-Blocking Example
always @(posedge clk) begin
Q0 <= D;
Q <= Q0; // Correct 2-stage behaviour
end

**Golden Rule**:  
👉 *Always use non-blocking (`<=`) for sequential flops.*  
<img width="1920" height="1080" alt="Screenshot (384)" src="https://github.com/user-attachments/assets/4fcbc51d-a2e6-4dd4-9612-ef671789a936" />

---

#### Caveat 2: Combinational Logic

**Goal**: `Y = (A | B) & C`.

// ❌ Incorrect use of blocking
always @(*) begin
Q0 = A | B;
Y = Q0 & C;
end

- **Simulation Issue**: Depends on old/stale value of Q0 → behaves with extra delay.  
- **Synthesis Result**: Still realises `Y = (A | B) & C`.  
- **Mismatch**: RTL sim looks like sequential, hardware is pure combinational.  

✅ Best Practice: For combinational, blocking works but must be carefully used. For sequential → **never** use blocking.  
<img width="1920" height="1080" alt="Screenshot (385)" src="https://github.com/user-attachments/assets/037c827a-dccb-46ea-828e-e6abcf5c73e2" />

---

## ✅ Key Takeaways

- **GLS** ensures RTL ≈ Hardware Netlist, catching mismatches.  
- Always use **`always @(*)`** for combinational logic.  
- Always use **non-blocking (`<=`)** for sequential logic.  
- Synthesis ignores RTL-level issues (like incomplete sensitivity lists) → GLS is critical to detect these.  
- Timing-aware GLS (with delays) is necessary for setup/hold validation, but functional GLS is sufficient to catch logical mismatches.  
