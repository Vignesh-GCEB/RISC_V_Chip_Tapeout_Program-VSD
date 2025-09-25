# Chapter 1: Synthesis Strategies in Yosys

This chapter covers different approaches to synthesising a design, focusing on hierarchy management and synthesis scope.

---

## 1.1 Hierarchical vs. Flattened Synthesis

When synthesising a design with multiple submodules, the hierarchy can either be preserved or flattened into a single module.

### 1.1.1 Preserving Hierarchy (Default)
- **Description**: In a standard synthesis flow, the hierarchical structure of the design is maintained in the output netlist.  
- **Example**:  
  If a top module `multiple_modules` instantiates `sub_module_1` (as U1) and `sub_module_2` (as U2), the final netlist will explicitly show the instantiations of U1 and U2.  
- **Visualisation**:  
  Using the `show` command before flattening will display the blocks for U1 and U2, not the individual gates inside them.

### 1.1.2 Flattened Synthesis
- **Description**: Flattening removes the boundaries between modules and produces a single, flat netlist containing only primitive gates.  
- **Command**:  
- **Key Outcome**:  
- Hierarchy of submodules is lost (U1 and U2 disappear).  
- Gates (AND, OR, etc.) inside submodules are pulled up into the top-level.  
- **Visualisation**:  
After `flatten`, the `show` command shows the expanded structure—direct AND/OR gates instead of U1 and U2 blocks.

### 1.1.3 Key Distinction Summary

| Feature   | Hierarchical Netlist            | Flattened Netlist                  |
|-----------|---------------------------------|------------------------------------|
| Hierarchy | Preserved (U1, U2 visible)      | Removed; no submodules             |
| Components| Submodule instances visible     | Only primitive gates (AND, OR, etc)|
| Structure | Modular, matches original RTL   | Single expanded list of gates      |

---

## 1.2 Submodule-Level Synthesis

Instead of synthesising the entire design from the top module, Yosys allows synthesis of specific submodules.

### 1.2.1 Concept and Command
- **Definition**: Synthesise a lower-level module independently of the overall design.  
- **Command**:  

synth -top <module_name>

- **Example**:  
synth -top sub_module_1

- This infers logic only for `sub_module_1` (e.g., one AND gate).  
- Other modules like `sub_module_2` are ignored.

---

### 1.2.2 Rationale for Submodule-Level Synthesis

#### 1. Reusability and Efficiency
- **Scenario**: A design instantiates the same module multiple times (e.g., a multiplier used 6 times).  
- **Advantage**: Instead of synthesising the logic six times, synthesise *once* and replicate.  
- **Process**:  
1. Synthesise the submodule (e.g., multiplier).  
2. Reuse the netlist for each instance.  
3. Stitch together at the top level.  
- **Benefit**: Saves significant synthesis time.

#### 2. Divide and Conquer for Massive Designs
- **Scenario**: Very large designs may not optimise well if synthesised as a whole.  
- **Advantage**: Break into smaller submodules for better results.  
- **Process**:  
1. Split design into smaller submodules.  
2. Synthesise each submodule individually for better optimisation.  
3. Stitch optimised results at the top level.  
- **Benefit**: Produces a high-quality netlist with better QoR (Quality of Results).

---

## ✅ Key Takeaways
- **Hierarchical synthesis** keeps module boundaries intact.  
- **Flattened synthesis** creates one flat netlist with only gates.  
- **Submodule-level synthesis** enables reuse, efficiency, and better optimisation for large designs.
