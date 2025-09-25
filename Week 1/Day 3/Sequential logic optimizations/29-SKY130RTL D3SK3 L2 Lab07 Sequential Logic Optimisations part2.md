# 🔄 Sequential Logic Optimisations

This lab explores how synthesis tools optimise **sequential circuits** (flip-flops or flops) when they are tied to constant inputs.  
We cover cases where flops remain intact and cases where they are optimised away into constant wires.

---

## 📖 Chapter 1: Optimisation of Constant-Input Flops

Sequential optimisation allows tools to **remove redundant sequential elements** if they behave as constants.

---


### 1.1 Case Study 1: `DFF_const_one` (Non-Optimised)

- **Observation**: Synthesis statistics showed that a **D flip-flop (DFF)** cell was inferred.  
- **Reason**: The flop still had sequential behaviour with clock dependency.  
- **Conclusion**:  
  - The flop **remains in the design**.  
  - Key point: Always check statistics after synthesis to identify what hardware was generated.

---

### 1.2 Case Study 2: `DFF_const_two` (Optimised Away)

- **Observation**: Synthesis report showed:  
- **Reason**: The output `Q` was **always constant = 1**, independent of clock or reset.  
- **Implementation**:  
- Flop removed entirely.  
- `Q` was directly tied to `1’b1`.  
- Clock and reset inputs had **no load**.  
- **Key Takeaway**:  
- Classic example where a **sequential constant** eliminates the DFF → replaced with a constant net.  

---<img width="1920" height="1080" alt="Screenshot (368)" src="https://github.com/user-attachments/assets/307c98ae-d869-44b7-a68e-bedb5e7d7446" />



## 🧮 Chapter 2: Analysis of Interconnected Constant Flops (`DFF_const_three`)

### 2.1 Circuit Description
- **Two flops, sharing same clock/reset**:  
- **Flop A (Q)**: Set flop, reset → `Q=1`. D-input = `Q1`.  
- **Flop B (Q1)**: Reset flop, reset → `Q1=0`. D-input = `1’b1`.  

---

### 2.2 Behavioural Analysis

#### Flop B (`Q1`)
1. At reset → `Q1=0`.  
2. After reset removal → on first clock edge, `Q1` samples D=1 → `Q1=1`.  
3. Remains `1` permanently.  
👉 **Not a constant** (transitions from 0 → 1). Cannot be removed.  

#### Flop A (`Q`)
1. At reset → `Q=1`.  
2. At **first clock edge**:  
 - Samples Q1 (still 0 at clock edge).  
 - Result: Q changes 1 → 0.  
3. At **second clock edge**:  
 - Q1 is now stable = 1.  
 - Q samples this and transitions 0 → 1.  
4. Waveform → Q is high always **except for one clock cycle drop** after reset.  
👉 **Not a constant** either.  

---
![Uploading Screenshot (369).png…]()

### 2.3 Final Synthesis Prediction
- Both flops **must remain** in the final design.  
- Neither can be optimised away despite constant-like inputs:  
- ❌ Cannot remove Flop B, since it changes from 0 → 1.  
- ❌ Cannot remove Flop A, since it changes from 1 → 0 → 1 over time.  
- ✅ Synthesis result will preserve **both flops**.  

> *Quote: “Can I remove flop B? No. Can I remove flop A? No. This is a very beautiful circuit — both flops retain meaning despite constant connections.”*

---

## ✅ Key Takeaways

- Tools remove flops **only when Q is truly constant** (like `DFF_const_two`).  
- If Q changes **even once** (e.g., from reset), flop must remain.  
- **DFF_const_one** → Flop retained (constant D but sequential behaviour).  
- **DFF_const_two** → Replaced by constant wire (true sequential constant).  
- **DFF_const_three** → Both flops preserved due to transient behaviour.  
- Always confirm with **simulation + synthesis stats** to validate expectations.  
