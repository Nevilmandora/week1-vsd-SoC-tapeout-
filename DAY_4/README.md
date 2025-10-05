# DAY4 : Introduction to Gate level Simulation (GLS) and Synthesis-Simulation Mismatches

## What is GLS ?

* Gate-level simulation (GLS) is the process in which post-synthesis gate-level netlist is simulated with actual timing information from a Standard Delay Format (SDF) file to verify functionality and timing violations

* Gate Level Simulation (GLS) is a crucial step in the digital design verification process, occurring after the synthesis of the Register Transfer Level (RTL) code.

* Netlist is logically same as the RTL Code.

* GLS is important for verify the logical correctness of design after synthesis.Ensuring the timing of design is met.

* So here we are using iverilog for the Perform GLS and below diagram represents the GLS flow using iverilog.

  ![WhatsApp Image 2025-10-05 at 10 27 44 PM](https://github.com/user-attachments/assets/882ac058-0676-446b-ad08-c7b968f28fa1)


* In this GLS process we can give the same testbench that we were made for RTL Simulation. and after GLS the waveform should be same as RTL Simulation.

## Synthesis Simulation Mismatch :

* Missing Sensitivity list

* Bloking vs Non-Blocking Assignments

* Non Standard Verilog Coding


  ### Missing Sensitivity list :

* A missing sensitivity list in a Verilog can lead to a simulation-synthesis mismatch. This occurs because synthesis tools and simulators interpret incomplete sensitivity lists differently.
 
**In Simulation:**

* Simulators strictly adhere to the sensitivity list. A process or always block will only re-evaluate and update its outputs when a signal explicitly included in its sensitivity list changes.

* If an input signal to a combinational logic block is omitted from the sensitivity list, changes to that signal will not trigger the block's re-evaluation, leading to incorrect or unexpected behavior in simulation.

**In Synthesis:**

* Synthesis tools are primarily concerned with inferring hardware. For combinational logic, they typically ignore the explicit sensitivity list and automatically infer that the output depends on all inputs to the block.

* This can result in the synthesized hardware behaving as if all inputs were in the sensitivity list.


* The difference in interpretation creates the mismatch.

  ### Example :

        module mismatch_example (
           input wire a, b, c,
           output reg y
        );
        always @(a, b) begin
            y = a & b & c; // 'c' is missing from the sensitivity list
        end
        endmodule

  * Simulation: The always block only executes when a or b changes. If c changes, y won’t update, leading to incorrect simulation results.
 
  * Synthesis: The synthesis tool generates an AND gate with inputs a, b, and c, so y updates whenever any of a, b, or c changes.
 
  * Result: The simulation output differs from the hardware behavior, causing a synthesis-simulation mismatch.
 
  * **How to Avoid Missing Sensitivity Lists ?:** In Verilog, use **always @*** or **always_comb (SystemVerilog)** for combinational logic. These automatically include all signals read in the block, eliminating the need to manually list them.
 

## Blocking vs non-blocking assignments :

* In Verilog, blocking (=) and non-blocking (<=) assignments behave differently. Blocking assignments execute sequentially within a block, while non-blocking assignments schedule updates for the next simulation cycle.

*   Misusing these (e.g., using blocking assignments in sequential logic or non-blocking in combinational logic) can lead to different simulation and synthesis behaviors. Synthesis tools interpret these based on context, often inferring unintended latches or flip-flops, causing mismatches.

### Caveats With Blocking Assignments :

* **Race Conditions in Sequencial logic :**

* Using blocking assignments in sequential logic (e.g., within an always @(posedge clk) block) can introduce race conditions. Since blocking assignments execute immediately and sequentially, the order of statements matters, and this can lead to unpredictable results when multiple signals are updated in the same time step.

       always @(posedge clk) begin
         a = b;
         b = a; // Race condition: 'a' and 'b' may not update as expected
       end

* Synthesis may infer latches or incorrect flip-flops, differing from simulation.

### Best Practices to mitigate Caveats :

* Use blocking assignments (=) only for combinational logic.
 
* Use non-blocking assignments (<=) for sequential logic (e.g., flip-flops, registers).

* Employ always @* for combinational logic to avoid sensitivity list errors.


 


  
