
# 🖥️ RISC-V Reference SoC Tapeout Program – VSD 

Welcome to the **RISC-V Reference SoC Tapeout Program (VSD)** repository.  
This repository documents the setup, learning journey, and technical tasks involved in India’s largest collaborative **RISC-V SoC design and tapeout initiative**, empowering thousands of participants to contribute to the open-source silicon ecosystem.  



## 📘 Class Summary: Digital VLSI SoC Design & Planning – Introduction

The first session provided a comprehensive overview of the SoC (System-on-Chip) design flow, emphasizing how digital VLSI systems are planned and realized from specifications to silicon.
## 🔑 Key Learnings
**1. SoC Design Flow Overview**

The session introduced the top-down flow of chip design, starting from system specifications and progressing through RTL design, integration, and physical design until final GDSII tape-out.

Highlighted the importance of synchronization between architecture, verification, and implementation teams in ensuring design accuracy.

**2. Stages of the Design Flow**

**#O1 – Chip Modeling:**

Begins with specifications written in C models, serving as the functional blueprint.

Verification testbenches are created in C language for validating design intent.

**#O2 – RTL Architecture:**

Hardware description using Verilog RTL creates a soft copy of the design.

Functional units include processor cores and peripherals/IPs, which are verified against system specifications.

**#O3 – SoC Integration:**

Integration of processor, peripherals, macros, and analog IPs into a single SoC.

Gate-level netlists are synthesized, and the design undergoes floorplanning, placement, clock tree synthesis (CTS), and routing.

**#O4 – Fabrication & Applications:**

Final GDSII layout is verified through DRC/LVS checks before manufacturing.

**3. Clocking and Performance**

The discussed design examples target clock frequencies in the range of 100 MHz to 130 MHz, suitable for real-world embedded applications.

**4. Correctness Criteria**

The design is considered functionally correct only when:

                        O0=O1=O2=O3=O4

This means the specifications, RTL design, SoC integration, and fabricated silicon must all behave identically.
## 🎯 Industry Relevance
This lecture bridged the gap between specifications and silicon realization, showing how SoC designs evolve into real-world applications. The flow ensures first-time-right silicon, which is critical for cost-effective and reliable chip production.