# Day3 : Combinational and Sequencial Optimization

## Key Topics: 
* Introduction to Optimization

* Combinational Logic Optimization

* Sequencial Logic Optimization

* Sequencial Optimization for Unused Outputs


  ## --->Combinational Logic Optimization

  * Squeezing the logic to get the most Optimized design (Area and Power)
 
  * Constant propagation
 
  * Boolean Logic Optimization
 

### 1) Constant Propagation:

* In this synthesizer replaces logic driven by constants with fixed values.

* if any signal in logic is constant (0 or 1), the synthesizer replace the logic with simpler form.

       assign y = a & 1'b0;  // always 0

       assign y = a | 1'b1;  // always 1

* It saves unnecessary gates and reduces delay.

* Here i have one example that how the constant propagation is done in optimization.


![WhatsApp Image 2025-10-05 at 5 45 27 PM](https://github.com/user-attachments/assets/0b00c91e-2749-4ca0-802c-a2b7f9a2b6d9)

* In this example in the boolean expression if we take A=0, then equation simplifies into c'. so this way the tool convert this type of logic optimization using constant propagation method. and before optimization it takes 6 MOS transistors and after optimization it takes only 2 MOS transistors.

* So by this example, we can understand how the optimization is done by the tool.




### 2) Boolean Logic Optimization:

* Basically in Boolean logic Optimization the synthesizer removes redundant logic.

      y = a & b | a & ~b = a

*  uses k-map, Mckluskey, etc..

* Here i have one example for boolean logic optimization as shown below.

![WhatsApp Image 2025-10-05 at 5 54 42 PM](https://github.com/user-attachments/assets/34548d43-a3a2-4c6b-b79a-430f64579321)

* So as shown in this example big boolean equation can be optimize in small equation. So the tool will convert this type of boolean equations into optimized format.

### 3) Squeezing the design:

* Squeezing the design applies aggressive sharing, don't care reduction, reconstructing, and technology mapping to get fastest and smallest circuit possible.




## ---> Sequencial Optimization:

* Basic : Sequencial Constant propagation

* Advanced: (i) state optimization,  (ii) Retiming,  (iii) Sequencial logic Clonning

## Sequencial Constant propagation:

* This is the basic technique as we show for cominational , sequencial constant propagation is also one method for optimization.


![WhatsApp Image 2025-10-05 at 6 10 43 PM](https://github.com/user-attachments/assets/641dbaa9-5426-432c-9297-32d5aae4a7cc)

* As shown in above image, when D input is permenantly connected to zero then, output y will always be 1.


  ### Here we are taking one counter example for understanding sequencial optimiation.

       module counter_opt(input clk, input reset, output q);
       reg[2:0] count;
       assign q = count[0];
       always@(posedge clk, posedge reset)
       begin
       if(reset)
              count <= 3'b000;
       else
              count <= count + 1;
       end
       endmodule

  ![WhatsApp Image 2025-10-05 at 6 37 04 PM](https://github.com/user-attachments/assets/2d7b1870-cc7d-4014-bb00-c6e6024dfbd3)

  * So here as shown in example, the output of the counter only depends on the count[0], so count[2] & count[1] are not used or we can say that output is not dependent on them. so here in this case optimization will done as shown in image that invert of the q will be given to d input. So that this way the optimization of sequential designs are done.
 
  * Also you can check it using synthesis , it that it will implement the design this way only.



### 1) State Optimization:

* This applies mainly to finite state machines. The idea is to reduce the number of states or re-encode them for efficiency.

* It optimizes area, timing and sometimes power.


### 2) Retiming: 

* This is about shifting registers across combinational logic without changing overall functionality.

* It Reduces critical path delay, so we can run at higher clock speeds.


### 3) Clonning:

* Clonning Duplicates registers to reduce the fan-out and net delay, improving performance.

