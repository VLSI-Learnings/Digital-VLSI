# System Verilog

* System verilog is an extension of verilog with many verification features that allow engineer to verify the design using complex test bench structures and random stimuli in simulation.

* **Simulation** is a technique of applying different input stimulus to the design at different times to check the rtl code behaves in the intended way.

* **Why not verilog**
  * Verilog was the primary language to verify the design which are not complex and had less features.
  * System verilog is superior to verilog because of its ability to perform constrained random stimuli use OOP features in the test bench construction, functional coverage, assertions etc.

* **Data types**
  * Verification of the hardware is getting more complex and data types in verilog are not sufficient to develop efficient test benches.

    |Data type|2/4 states|bits|type|
    |---|---|---|---|
    |reg|4|1|unsigned|
    |wire|4|1|unsigned|
    |logic|4|1|unsigned|
    |bit|2|1|unsigned|
    |byte|2|8|signed|
    |shortint|2|16|signed|
    |int|2|32|signed|
    |longint|2|64|signed|
    |integer|4|32|signed|
    |shortreal||||
    |real||||
    |time||||
    |realtime||||

* four state values that a variable in system verilog can hold
  * 0 - Logic state 0 - variable/net is at 0 volts
  * 1 - Logic state 1 - variable/net is at some value > 0.7 volts
  * x or X - Logic state X - variable/net has either 0/1 - we just don't know
  * z or Z - Logic state Z - net has high impedence - maybe the wire is not connected and is floating
