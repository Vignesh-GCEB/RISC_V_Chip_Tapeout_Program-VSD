# 🔄 Introduction to Sequential Logic Optimisation

This document introduces basic and advanced **sequential logic optimisation techniques**. It focuses primarily on **Sequential Constant Propagation**, with case studies to show when optimisation is successful and when it is not.

---

## 📖 1.0 Overview of Optimisation Techniques

### Categories
- **Basic Technique**  
  - **Sequential Constant Propagation**  
  - Practically demonstrated in lab sessions.  

- **Advanced Techniques** (theoretical awareness only)  
  - State Optimisation  
  - Retiming  
  - Sequential Logic Cloning  
<img width="1920" height="1080" alt="Screenshot (354)" src="https://github.com/user-attachments/assets/2b2747c6-67d9-4ddd-badd-159ffff95a6b" />

---

## 🧮 2.0 In-Depth Analysis: Sequential Constant Propagation

### 2.1 Definition and Core Concept
- Sequential constant propagation occurs when the output (**Q**) of a flip-flop is **always constant (0 or 1)** under all conditions.  
- This constant output can be **propagated downstream** through combinational logic, enabling hardware simplification.  

---

### 2.2 Case Study 1: Optimisable Circuit (Flop with Reset)

**Scenario**:  
- DFF where:  
  - `D` input = **0** (tied low)  
  - Active-high **reset** present  
  - `Q` output feeds a NAND gate with another input `A`  
  - Final Output: `Y`  

**Analysis of Q**:
1. **Reset Applied**: Q = 0  
2. **Reset Released + Clock**: Since `D=0`, on next clock → Q = 0  
3. **Conclusion**: In all cases, **Q is constant = 0**.  

**Propagation**:  
\[
Y = (A \cdot Q)' = (A \cdot 0)' = (0)' = 1
\]  

**Optimisation Result**:
- Flip-flop and NAND gate are redundant  
- Entire circuit replaced by constant `Y = 1`.  

✅ This is a **sequential constant** case → optimisation possible.  

---

### 2.3 Case Study 2: Non-Optimisable Circuit (Flop with Set)

**Scenario**:  
- DFF where:  
  - `D` input = **0**  
  - Active-high **asynchronous set** replaces reset  

**Analysis of Q**:
1. **Set Applied**: Q = 1 (asynchronous, immediate)  
2. **Set Released + Clock**: At next clock edge, Q → 0 (follows `D`)  
3. **Conclusion**: Q takes both values (`0` and `1`), not constant  

**Timing Behaviour**:
- Q ≠ simply equal to `set`  
- Q becomes `1` immediately when `set=1`  
- Q returns to `0` **only on clock edge** after set is de-asserted → delay introduced  

**Optimisation Result**:
- Q is **not constant**  
- Flip-flop must **remain** in circuit  
- Cannot propagate constant `0` from `D`  

❌ This is **not** a sequential constant case → no optimisation possible.  

---

### 2.4 Deciding Factor for Sequential Constants

**Rule of Thumb**:
> *“For a flop to be a sequential constant, the Q pin must always take one constant value, under all conditions.”*  

- Tying `D` to a constant ≠ sufficient  
- Need to consider **asynchronous inputs** (reset, set):  
  - Reset → forces Q always = 0 → sequential constant case  
  - Set → allows Q = 1 at times → not sequential constant  

---

## 🚀 3.0 Brief Overview of Advanced Techniques

The following advanced sequential optimisation methods exist but are beyond practical lab scope:

- **State Optimisation**: Reduce FSM state count for simpler transitions.  
- **Retiming**: Relocate flip-flops across combinational logic to balance delays or improve speed.  
- **Sequential Logic Cloning**: Duplicate sequential elements to reduce fanout load or improve timing.  
<img width="1920" height="1080" alt="Screenshot (355)" src="https://github.com/user-attachments/assets/ea986691-c247-4afb-8771-71f1afe1cfba" />

---

## ✅ Key Takeaways

- **Sequential Constant Propagation**: Remove redundant FFs and gates when Q is *always constant*.  
- **Optimisable Case**: Flop with reset → Q always 0 → circuit collapses to constant.  
- **Non-Optimisable Case**: Flop with set → Q toggles between 0 and 1 → flop required.  
- **Rule**: Both `D` input and asynchronous controls must enforce a *single constant value* for sequential constant optimisation.  
- **Advanced Techniques** (State Optimisation, Retiming, Cloning) help in timing and area but require more complex analysis.  
