# 🕒 D Flip-Flop Reset Strategies in Verilog

This document describes different reset strategies used in **D flip-flops** — asynchronous reset, asynchronous set, synchronous reset, and combined reset designs — including Verilog coding styles, behaviour, and hardware implications.

---

## 1.0 D Flip-Flop with Asynchronous Reset

- **Definition**: Output reacts immediately to reset signal, independent of clock.  

### 1.1 Verilog Code and Behaviour
- **Sensitivity List**:  

always @(posedge clock or posedge async_reset)

- Triggered by either clock edge or async reset edge.  
- **Priority**: Reset has highest priority inside `if` condition.  
- **Logic**:  
- If `async_reset = 1` → `Q = 0` immediately.  
- Else if clock edge occurs → `Q = D`.  
- **Characteristic**:  
Reset action does **not wait** for clock.  

---

## 2.0 D Flip-Flop with Asynchronous Set

- **Definition**: Similar to asynchronous reset, but forces `Q = 1` independent of clock.  

### 2.1 Verilog Code and Behaviour
- **Sensitivity List**:  

always @(posedge clock or posedge async_set)

- **Logic**:  
- If `async_set = 1` → `Q = 1` immediately.  
- Else if clock edge occurs → `Q = D`.  
- **Note**: Signal name doesn’t matter (`alpha`, `beta` etc.), behaviour defines functionality.  

---

## 3.0 D Flip-Flop with Synchronous Reset

- **Definition**: Reset occurs **only on the active clock edge**.  

### 3.1 Verilog Code and Behaviour
- **Sensitivity List**:  

- **Logic**:  
- If `async_set = 1` → `Q = 1` immediately.  
- Else if clock edge occurs → `Q = D`.  
- **Note**: Signal name doesn’t matter (`alpha`, `beta` etc.), behaviour defines functionality.  

---

## 3.0 D Flip-Flop with Synchronous Reset

- **Definition**: Reset occurs **only on the active clock edge**.  

### 3.1 Verilog Code and Behaviour
- **Sensitivity List**:  

always @(posedge clock)

- **Logic**:  
- On clock rising edge:  
  - If `sync_reset = 1` → `Q = 0`.  
  - Else → `Q = D`.  
- **Characteristic**: Reset ignored unless clock rises.  

### 3.2 Hardware Implementation
- Synchronous reset implemented using a **MUX at D-input**:  
- If `sync_reset = 1` → MUX selects `0`.  
- If `sync_reset = 0` → MUX selects `D`.  
- Value at D waits until next clock edge before being loaded.  

---

## 4.0 D Flip-Flop with Both Asynchronous and Synchronous Resets

- **Definition**: Combines immediate reset (async) with clock-dependent reset (sync).  

### 4.1 Verilog Code and Behaviour
- **Sensitivity List**:  

always @(posedge clock or posedge async_reset)

- **Logic Priority**:  
1. If `async_reset = 1` → `Q = 0` immediately.  
2. Else, triggered by clock edge:  
   - If `sync_reset = 1` → `Q = 0`.  
   - Else → `Q = D`.  

### 4.2 Hardware Implementation
- Flip-flop uses:  
- **Dedicated async reset pin**  
- **MUX on D-input** for synchronous reset functionality  

---

## 5.0 Comparison Summary and Notes

| Feature        | Asynchronous Reset                          | Synchronous Reset                           | Both Async & Sync                         |
|----------------|---------------------------------------------|---------------------------------------------|-------------------------------------------|
| Sensitivity    | `posedge clock` + `posedge reset`           | `posedge clock` only                        | `posedge clock` + `posedge async_reset`   |
| Reset Logic    | Checked **outside** clock block             | Checked **inside** clock block              | Async outside, Sync inside                |
| Timing         | **Immediate**, independent of clock         | On next active clock edge                   | Async = immediate, Sync = clock edge      |
| Hardware       | Uses direct reset pin of flop               | Uses MUX at D-input                         | Uses both flop reset pin + MUX at D-input |

### ⚠️ Important Note
- Using **both reset and set signals simultaneously** may cause **race conditions**.  
- Designers must carefully avoid undefined behaviour.  

---

## ✅ Key Takeaways
- **Asynchronous reset/set** → Immediate effect, bypasses clock.  
- **Synchronous reset** → Controlled by clock edge, safer for timing closure.  
- **Combined reset** → Offers flexibility but must be handled carefully.  
- Correct reset strategy is essential for reliable digital system design.  
