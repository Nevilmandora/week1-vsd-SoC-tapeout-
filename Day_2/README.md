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


In the name of the .lib file we can see some notations like tt, 025C, 1v80, etc...

   * __tt: typical process corner__

   * __025C: Temprature__

   * __1v80: Voltage__

   So these things represents PVT Corners, process , voltage and temprature corners.




## 2) Flattend and hierarchical synthesis

* Basically why flattend or submodule level synthesis is needed ?

  In the larger designs if we put directly whole design in tool so its probability that tool can not efficiently work.

  So that whenever we have larger design or we have multiple instances of modules, in that case we should go for flattend or submodule synthesis so that tool can perfectly done its work.

  



