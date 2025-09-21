# Chapter 3: Netlist Generation and Analysis in Yosys
<img width="1920" height="1080" alt="Screenshot (312)" src="https://github.com/user-attachments/assets/e5850d85-2810-48c7-bd53-41fae30adce8" />

--------------------------------------------------------------------------------
3.1 Writing the Verilog Netlist
--------------------------------------------------------------------------------
- Description:
  Export the synthesized design into a gate-level Verilog netlist file.

- Objective:
  Generate a structural Verilog file (netlist) that describes the circuit in terms 
  of standard library cells and their interconnections.

- Primary Command:
  write_verilog <filename.v>

    Example (basic usage):
      write_verilog mux_netlist.v

    Issue:
      - Basic usage generates a netlist with a lot of extra information.
      - This makes it harder to read and analyse.

- Recommended Command (clean netlist):
  write_verilog -noattr <filename.v>

    Example:
      write_verilog -noattr goodmux_netlist.v

    Result:
      - Produces a "sweet and small netlist".
      - Simplified and easier to analyse.
<img width="1920" height="1080" alt="Screenshot (314)" src="https://github.com/user-attachments/assets/cd313f62-0933-43f8-be66-74884c209e9b" />


--------------------------------------------------------------------------------
3.2 Analysing the Generated Netlist
--------------------------------------------------------------------------------
- The netlist is a textual representation of the circuit after synthesis.

3.2.1 Core Components of the Netlist
------------------------------------
- Top Module:
  - Same as the module targeted for synthesis (set with synth -top).
  - Example: good_mux.

- Cell Instantiations:
  - Each cell in the technology library is instantiated with a unique name.
  - Examples:
      * inv   → inverter
      * nand  → NAND gate
      * R2AI  → R2 AND Invert (complex gate)

- Nets (Wires):
  - Declared internal wires connect cell ports.
  - Nets often use numerical names (e.g., net 4).
  - Primary inputs/outputs also act as nets.

3.2.2 Tracing Logic in the Netlist
----------------------------------
- Step 1: Identify Primary Inputs
    * I0 (net 0) → connected to inverter input.
    * I1 (net 1) → connected to NAND gate input.
    * sel (net 2) → connected to NAND gate + complex gate.

- Step 2: Follow Internal Connections
    * Inverter output = net 4.
    * Net 4 connects inverter → O2 AND gate (inside R2AI cell).

- Step 3: Trace Parallel Paths
    * NAND gate inputs: sel (net 2), I1 (net 1).
    * NAND gate output → input of R2AI cell.

- Step 4: Final Stage
    * R2AI cell inputs: O2 AND gate output + NAND gate output.
    * R2AI cell output → primary output Y of good_mux module.
# Chapter 3: Netlist Generation and Analysis in Yosys

--------------------------------------------------------------------------------
3.1 Writing the Verilog Netlist
--------------------------------------------------------------------------------
- Description:
  Export the synthesized design into a gate-level Verilog netlist file.

- Objective:
  Generate a structural Verilog file (netlist) that describes the circuit in terms 
  of standard library cells and their interconnections.

- Primary Command:
  write_verilog <filename.v>

    Example (basic usage):
      write_verilog mux_netlist.v

    Issue:
      - Basic usage generates a netlist with a lot of extra information.
      - This makes it harder to read and analyse.

- Recommended Command (clean netlist):
  write_verilog -noattr <filename.v>

    Example:
      write_verilog -noattr goodmux_netlist.v

    Result:
      - Produces a "sweet and small netlist".
      - Simplified and easier to analyse.


--------------------------------------------------------------------------------
3.2 Analysing the Generated Netlist
--------------------------------------------------------------------------------
- The netlist is a textual representation of the circuit after synthesis.

3.2.1 Core Components of the Netlist
------------------------------------
- Top Module:
  - Same as the module targeted for synthesis (set with synth -top).
  - Example: good_mux.

- Cell Instantiations:
  - Each cell in the technology library is instantiated with a unique name.
  - Examples:
      * inv   → inverter
      * nand  → NAND gate
      * R2AI  → R2 AND Invert (complex gate)

- Nets (Wires):
  - Declared internal wires connect cell ports.
  - Nets often use numerical names (e.g., net 4).
  - Primary inputs/outputs also act as nets.

3.2.2 Tracing Logic in the Netlist
----------------------------------
- Step 1: Identify Primary Inputs
    * I0 (net 0) → connected to inverter input.
    * I1 (net 1) → connected to NAND gate input.
    * sel (net 2) → connected to NAND gate + complex gate.

- Step 2: Follow Internal Connections
    * Inverter output = net 4.
    * Net 4 connects inverter → O2 AND gate (inside R2AI cell).

- Step 3: Trace Parallel Paths
    * NAND gate inputs: sel (net 2), I1 (net 1).
    * NAND gate output → input of R2AI cell.

- Step 4: Final Stage
    * R2AI cell inputs: O2 AND gate output + NAND gate output.
    * R2AI cell output → primary output Y of good_mux module.
