# DAY2 : Timing libs, hierarchical vs flat synthesis and efficient coding styles 

## key topics:

 * Understanding .lib file               
  
  * Submodule level synthesis

  * Different flip flop coding styles

  * Some optimizations in designs 


 ## 1) .lib file:

 .lib file normally contains different flavours of all logic cells.


 in library file(sky130PDK) , we see that Different types of logic cells are present like and2_0, and2_1, and2_2, or1_0, or1_2, etc.. 
 and same way all logic cells have many different flavours of cells in terms of area, power and also speed and performance. 


So by viewing the .lib file i understand that what is actually .lib file contain and why this all cells are important for designing.

<img width="1920" height="1000" alt="Screenshot (1)" src="https://github.com/user-attachments/assets/357c16a5-7cdf-45b3-a04a-a42ceef4f525" />


In the name of the .lib file we can see some notations like tt, 025C, 1v80, etc...

   * __tt: typical process corner__

   * __025C: Temprature__

   * __1v80: Voltage__

   So these things represents PVT Corners, process , voltage and temprature corners.



This file is opened using below command,

     vim sky130_fd_sc_hd__tt_025C_1v80.lib


## 2) Flattend and hierarchical synthesis:

* Basically why flattend or submodule level synthesis is needed ?

  In the larger designs if we put directly whole design in tool so its probability that tool can not efficiently work.

  So that whenever we have larger design or we have multiple instances of modules, in that case we should go for flattend or submodule synthesis so that tool can perfectly done its work.


## Good mux example :

* Here we can see that how the synthesis tool convert the multiplexer design to the gate level netlist.

       module good_mux(input i0, input i1, input sel, output reg y);
       always@(*)
       begin
            if(sel)
               y<=i1;
            else
               y<=i0;
       end
       endmodule

  * So the synthesis tool will convert above design into gates as represent below:
 
    
![WhatsApp Image 2025-10-05 at 5 28 50 PM](https://github.com/user-attachments/assets/d272bb2b-b897-4d8b-9e0e-c3f45a4751e0)

* you can also verify this by synthesis of this design using yosys.


![WhatsApp Image 2025-10-05 at 5 32 33 PM](https://github.com/user-attachments/assets/ea4efbb7-7598-490e-8ec7-30e775889350)

  
     
## 3) Flip flop coding styles:

* Normally flip flops are used for removing glitches from the circuits by using flip flop. so flip flop stores data and then passes to that data to the next circuit effectively without any glitch.

* For the efiicient use of the flip flop we can use asynchronous and synchronous behaviour of it. so here we present some effective coding styles for
   asynchronous/synchronous behaviour of flip flops.
  
### Asynchronous Reset D Flip-Flop:

    module dff_asyncres (input clk, input async_reset, input d, output reg q);
        always @ (posedge clk, posedge async_reset)
        if (async_reset)
           q <= 1'b0;
        else
           q <= d;
    endmodule

* It is asynchronous reset d flip flop that goes immediately 0 when reset is high independent from clock edge.


<img width="1920" height="1017" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/1c24fb86-1543-4713-b906-87d718ce01d9" />



### Asynchronous Set D Flip-Flop:

    module dff_async_set (input clk, input async_set, input d, output reg q);
        always @ (posedge clk, posedge async_set)
        if (async_set)
           q <= 1'b1;
        else
           q <= d;
    endmodule

* It is asynchronous set d flip flop that goes immediately 1 when set is high independent from clock edge.

### Synchronous Reset D Flip-Flop

    module dff_syncres (input clk, input sync_reset, input d, output reg q);
        always @ (posedge clk)
        if (sync_reset)
           q <= 1'b0;
        else
           q <= d;
    endmodule

 * It is synchronous reset d flip flop that goes 0 only when reset is high and also clock edge is there. means it is dependent on clock edge.


<img width="1920" height="1011" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/3010fa2f-bceb-473a-8a72-f158b6649cc9" />


### Asynchronous Reset and Synchronous Reset D Flip-Flop:

    module dff_asyncres_syncres (input clk, input async_reset, input sync_res, input d, output reg q);
         always @ (posedge clk, posedge async_reset)
         if (async_reset)
            q <= 1'b0;
         else if (sync_reset)
            q <= 1'b0;
         else
            q <= d;
    endmodule

* This way we can also combine both Asynchronous and synchronous behaviour in one design.

  <img width="1920" height="1011" alt="Screenshot (6)" src="https://github.com/user-attachments/assets/5845f4e6-4210-48e1-9f84-5fd294e81434" />


  ## Simulation and synthesis flow:

    
      iverilog dff_asyncres.v tb_dff_asyncres.v

      ./a.out

      gtkwave tb_dff_asyncres.vcd

  Using this commands i done simulation of the different coding styles of d flip flop.


 ## Synthesis with yosys:

 For the synthesis i used below commands.

    yosys

    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

    read_verilog ../verilog_files/dff_asyncres.v

    synth -top dff_asyncres

    dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

    abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

    show

<img width="1133" height="805" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/8db3c463-472f-42ad-b100-0da1181a40fb" />


## Summary:

* In This day 2 i understand .lib file, i got an overview about different types of coding styles in the design perspective and also learn about optimizations that how the synthesizer optimize the logic and give the final netlist of design. and got idea about flattend and hierarchical synthesis and importance of submodule synthesis.  

    
 


   



   
