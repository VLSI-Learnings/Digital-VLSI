# Task & Functions & User Defined Primitives

* [UDP](/Verilog/other_topics.md#udp)
* [Task & Functions](/Verilog/other_topics.md#task-&-functions)

## UDP

* A UDP is defined using a UDP declaration which has the following syntax.
  primitive UDP_name ( OutputName , List_of_inputs );
    Output_declaration
    List_of__input_declarations
    [ Reg_declaration ]
    [ Initial_statement ]
    table
      List_of_table_en tries
    endtable
  endprimitive
* UDP definition does not depend on a module definition and thus appears outside of a module definition. A UDP definition can also be in a separate text file.
* UDP can have only one output and may have one or more inputs. The first port must be the output port. In addition, the output can have the value 0, 1 or x (z is not allowed). If a value z appears on the input, it is treated as an x.
* The behavior of a UDP is described in the form of a table. The following two kinds of behavior can be described in an UDP.
  1. Combinational
  2. Sequential (edge-triggered and level-sensitive).

* **Combinational UDP**
  * In a combinational UDP,the table specifies the various input combinations and their corresponding output values. Any combination that is not specified is an x for the output.

      ```verilog
      primitive mux_21(
        output Z,
        input a,b,sel);
          table
            //a b sel : Z
              0 ?   1 : 0;
              1 ?   1 : 1;
              ? 0   0 : 0;
              ? 1   0 : 1;
              0 0   x : 0;
              1 1   x : 1;
          endtable
        endprimitive
      ```

  * The ? character representsa don't-care value, that is, it could either be a 0, 1 or x.
  * The order of the input ports must match the order of entries in the table.

* **Sequential UDP**
  * In a sequential UDP, the internal state is described using a 1-bit register. The value of this register is the output of the sequential UDP.
  * There are two different kinds of sequential UDP, one that models level sensitive behavior and another that models edge-sensitive behavior.
  * A sequential UDP uses the current value of the register and the input values to determine the next value of the register (and consequently the output).
  * **Initializing the state register**
    * The state of a sequential UDP can be initialized by using an initial statement that has one procedural assignment statement.
      initial reg_name = 0, 1 or x;

  * **Level-sensitive sequential UDP**

      ```verilog
      primitive d_latch(
        output Q,
        reg Q,
        input Clk,D);
        initial Q = 0;
          table
            // Clk  D : Q(state): Q(next)
                0   1 : ?       : 1 ;
                0   0 : ?       : 0 ;
                1   ? : ?       : - ;
          endtable
        endprimitive
      ```

    * The - character implies a "no change". Note that the state of the UDP is stored in register Q
  * **Edge triggered sequential UDP**

      ```verilog
      primitive d_ff(
        output Q,
        reg Q,
        input Clk,D);
        initial Q = 0;
          table
            // Clk  D : Q(state): Q(next)
                01  1 : ?       : 1 ;
                01  0 : ?       : 0 ;
                0x  1 : 1       : 1 ;
                0x  0 : 0       : 0 ;
              //ignore negative edge clock
                ?0  ? : ?       : - ;
              //ignore on steady clock
                ?   ? : ?       : - ;
          endtable
        endprimitive
      ```

    * The table entry (01) indicates a transition from 0 to 1, the entry (Ox) indicates a transition from 0 to x, the entry (?0) indicates a transition from any value (0,1, or x) to 0, and the entry (??) indicates any transition.
    * For any unspecified transition, the output defaults to an x.
  * Table entries

    |Symbol|Meaning|
    |--|--|
    |0|logic 0|
    |1|logic 1|
    |x|unknown|
    |?|any of 0, 1,or x|
    |b|any of 0 or 1|
    |-|no change|
    |(AB)|value change from A to B|
    |*|sameas(??)|
    |r|same as (01)|
    |f|same as (10)|
    |p|any of (01), (Ox), (xl)|
    |n|any of (10), (lx),(x0)|

## Task & Functions

**Identifiers:**

* An identifier in Verilog HDL is any sequence of letters,digits,the $ character, and the _ (underscore) character, with the restriction that the first character must be a letter or an underscore.

**Keywords:**

* These are predefined identifiers which has specific meaning and used in certain context.
  * initial, always, begin, case, assign, and, or, xor
  * buf, casex, default, assign, bufif0, casez
  * defparam, bufif1, cmos, disable, etc
    **Note that only lowercase are keywords**

**Comments:**

* In verilog we can comment the description in two ways.
  * Multiple lines

    ```verilog
    /*
    Description
    */
    ```

  * Single line

    ```verilog
    //Description
    //Description
    ```

**Format:** Verilog is free format, statements can be written in a single line or multiple lines.

* **Task:**
  * Task is a reusable code that can be invoked from different parts of the design.
  * A task can return zero or more values and can allows delays(mostly used in test benches).
  * A task can call a another task or a function
    task task_id ;
      [ declarations ]
      procedural_statement
    endtask
  * Task calling
    * A task is called by a task enable statement that specifies the argument values passed to the task and the variables that receive the results. A task enable statement is a procedural statement and can thus appear within an always or an initial statement. It is of the form:
      task_id [ ( exprl , expr2 , exprN) ] ;
    * The list of arguments must match the order of input, output and inout declarations in the task definition. In addition, arguments are passed by value, not by reference.
* **Function:**
  * Function is like task but returns only one value.
  * This executes at zero time, must have atleast one input and doesn't allow delays.
  * A function can call another function but not another task because task has delays.
  * A function definition can appear anywhere in a module declaration. It is of the form:
      function [ range ] function_id ;
        input_declaration
        other_declarations
        procedural_statement
      endfunction
  * An input to the function is declared using the input declaration. If no range is specified with the function definition, then a 1-bit return value is assumed.
  * The function definition implicitly declares a register internal to the function, with the same name and range as the function. A function returns a value by assigning a value to this register explicitly in the function definition.
  * Function calling
    * A function call is part of an expression. It is of the form:
      reg_name = func__id ( exprl , expr2 , exprN)
* all local registers declared within a function/task definition are static, that is, local registers within a function retain their values between multiple invocations of the function/task.

* **System Task and Functions:** - An identifier starting with **$** character is system task of function.
  * Verilog HDL provides built-in system tasks and system functions, that is, tasks and functions that are predefined in the language. These are grouped as follows:
    1. Display tasks
    2. File I/O tasks
    3. Timescale tasks
    4. Simulation control tasks
    5. Timing check tasks
    6. PLA modeling tasks
    7. Stochastic modeling tasks
    8. Conversion functions for reals
    9. Probabilistic distribution functions
  * PLA modeling tasks and stochastic modeling tasks are outside the scope of this book.
  * **Display tasks**
    * The display system tasks are used for displaying and printing information. These system tasks are further characterized into:
      * **Display and write tasks**
        * The display task prints the specified information to standard output with a end-of-line character, while the write task prints the specified information without an end-of-line character.
        * $display $displayb $displayh $displayo
        * $write $writeb $writeh $writeo
      * **Strobed monitoring**
        * $strobe $strobeb $strobeh $strobeo
        * These system tasks display the simulation data at the specified time but at the end of the time step. "End of time step" implies that all events have been processed for the specified time step.
        * The strobe task differs from the display task in that the display task is executed at the time the statement is encountered, while the execution of the strobe task is postponed to the end of the time step.
      * **Continuous monitoring**
        * $monitor $monitorb $monitorh $monitoro
        * These tasks monitor the specified arguments continuously. Whenever there is a change of value in an argument in the argument list, the entire argument list is displayed at the end of the time step.
        * Monitoring can be turned on and off by using the following two system tasks.
        * $monitoroff; // Disables all monitors.
          $monitoron; // Enables all monitors.
        * These provide a mechanism to control dumping of value changes.
        * The $monitoroff task turns off all monitoring so that no more messages are displayed. The $monitoron task is used to enable all monitoring.
  * **File I/O Tasks**
    * Opening and Closing Files.
    * A system function $fopen is available for opening a file.
      integer file_pointer = $fopen ( file_name ); // The $fopen system function returns an integer value (a pointer) to the file.
    * A system task can be used to close a file.
      $fclose (file_pointer);
    * **Writing out to a File**
      * The display, write, strobe and monitor system tasks have a corresponding counter part that can be used to write information to a file. These are:
      * $fdisplay $displayb $displayh $displayo
      * $fwrite $fwriteb $fwriteh $fwriteo
      * $fstrobe $strobeb $fstrobeh $strobeo
      * $fmonitor $monitorb $monitorh $monitoro
      * The first argument for all these tasks is a file pointer. Remaining arguments for the task is a list of pairs of format specification followed by an argument list.
    * **Reading from a File**
      * There are two system tasks available for reading data from a file. These tasks read data from a text file and load the data into memory. These system tasks are:
      * $readmemb $readmemh
        reg [0:3] Mem_A [0:63];
        initial
          $readmemb ("ones_and_zeros.vec", Mem_A);
      * Optionally an explicit address can also be specified in the system task call, such as:
        $readmemb ("rx.vec", Mem_A, 15, 30);  // The first number read from the file "rx.vec" is stored in address15, next one at 16, and so on until address30.
  * **Timescale tasks**
    * **$printtimescale** - displays the time unit and time precision for the specified module.
      * The $printtimescale task with no arguments specified prints the time unit and time precision for the module that contains this task call. If a hierarchical path name to a module is specified as its argument, this system task prints the time unit and precision for the specified module.
    * $timeformat - specifies how the %t format specification must report time information.
      * The task is of the form:
        $timeformat( units_number , precision , suffix , numeric_field_width );
        $timeformat (-4, 3, "ps", 5) ;
        $display ("Current simulation time is %t", $time);
      * where a units_number is:
        0 for 1 s
        -1 for 100ms
        -2 for 10ms
        -3 for 1 ms
        -4 for 100us
        -5 for l0us
        -6 for 1 us
        -7 for 100us
        -8 for 10ns
        -9 for 1 ns
        -10 for 100ps
        -11 for 10ps
        -12 for 1 ps
        -13 for 100 fs
        -14 for 10fs
        -15 for 1 fs
  * **Simulation Control Tasks**
    * **$finish**; - makes the simulator exit and return control back to the operating system.
    * **$stop**; - causes the simulation to suspend. At this stage, interactive commands may be issued to the simulator.
  * **Timing check tasks**
    * $setup ( data_event , reference_event , limit );
      * reports a timing violation if: ( time_of_reference_event -  time_of_data_event ) < limit
      * An example of this task call is:
        $setup{D, posedge Ck, 1.0);
    * $hold( reference_event , data_event , limit );
      * reportsa violation if: (time_of_data_event - time_of_reference_event) < limit
      * Here is an example.
        $hold (posedge Ck, D, 0.1);
    * $setuphold( reference_event , data_event , setup_limit , hold_limit );
      * The following system task is a combination of the \$setup and $hold tasks.
    * $width ( reference_event , limit , threshold );
      * ports a violation if: threshold < ( time_of_data_event - time_of_reference_event ) < limit
      * The data event is derived from the reference event: it is the reference event with the opposite edge. Here is an example.
        $width (negedge Ck, 0.0, 0);
    * $period( reference_event , limit );
      * reports a violation if: ( time_of_data_event - time_of_reference_event ) < limit
      * The reference event must be an edge-triggered event. The data event is derived from the reference event: it is the reference event with the same edge.
    * $skew( reference_event, data_event , limit );
      * reports a violation if: time_of_data_event - time_of_reference_event > limit
      * If time of data_event is equal to the time of reference_event, no violation is reported.
    * $recovery ( reference_event , data_event, limit);
      * reports a timing violation if: ( time_of_data_event - time_of_reference_event ) < limit
      * The reference event must be an edge-triggered event. This system task records the new reference event time before performing the timing check; therefore if the data event and the reference event both occur at the same simulation time, a violation is reported.
    * $nochange ( reference_event, data_event, start_edge_offset, end_edge_offset );
      * reports a timing violation if the data event occurs during the specified width of the reference event. The reference event must be an edge-triggered event. The start and stop offsets are relative to the reference event edge. For example,
        $nochange (negedge Clear, Preset, 0, 0);
      * will report a violation if Preset changes while Clear is low.
    * Each of the above system tasks may optionally have a last argument which is a notifier. A system task updates a  notifier, when there is a timing violation, by changing its value according to the following case statement.
      case ( notifier )
        'bx : notifier = 'b0;
        'b0 : notifier = 'bl;
        'bl : notifier = 'b0;
        'bz : notifier = 'bz;
      endcase
    * A notifier can be used to provide information about the violation or propagate an x to the output that reported the violation. Here is an example of a notifier.
      reg NotifyDin;
      $setuphold (negedge Clock, Din, tSETUP, tHOLD, NotifyDin);
      * In this example, NotifyDin is the notifier. If a timing violation occurs, the register NotifyDin change.
  * **Simulation time functions**
    * The following system functions return the simulation time.
      * $time : Returns the time as an integer in 64 bits scaled to the time unit of the module that invoked it.
      * $stime : Returns time in 32 bits.
      * $realtime : Returns time as a real number scaled to the time unit of the module that invokes it.
  * **Conversion Functions**
    * The following system functions are utility functions that convert between number types.
      * $rtoi (real_value) : Converts a real number to an integer by truncating the decimal value.
      * $itor (integer_value) : Converts integer to real.
      * $realtobits (real_value): Converts a real into 64-bit vector representation of the real number (IEEE 754 representation of the real number).
      * $bitstoreal (bit_value) : Converts a bit pattern to a real number.
  * **Probabilistic Distribution Functions**
    * **$random [(seed)]**
      * returns a random number as a 32-bit signed integer based on the value of the seed.
      * The seed (must be a reg, integer or a time register)controls the number that the function returns, that is, a different seed will generate a different random number. If no seed is specified, a random number is generated every time $random function is called based on a default seed.
      * If a number within a range, say -10 to +10 is desired,the modulus operator can be used to generate such a number
      * {\$random}%10 - The concatenation ({})operator interprets the signed integer returned by the $random function as an unsigned number.
    * The following functions generate pseudo-random numbers according to the probabilistic function specified in the function name.
      1. $dist_uniform ( seed , start , end )
      2. $dist_normal ( seed , mean , standard_deviation , upper)
      3. $dist_exponential ( seed , mean )
      4. $dist_poisson ( seed , mean )
      5. $dist_chi_square ( seed , degree_of_freedom )
      6. $dist_t ( seed , degree_of_freedom)
      7. $dist_erlang ( seed , k_stage , mean )
    * All parameters to these functions must be integer values.
* **Disable Statement**
  * A disable statement is a procedural statement (hence it can only appear within an always or an initial statement).
  * A disable statement can be used to terminate a task or a block (sequentialor parallel) before it completes executing all its statements. It can be used to model hardware interrupts and global resets. It is ofthe form:
      disable task_id;
      disableblock_id;
  * Disablinga task is not recommended, especially if the task returns output values. This is because the language specifies that the values of the output and inout arguments are indeterminate when a task is disabled. A better approach is to disable the sequential block, if any, within the task.
* **Named events**
  * This is a data type, used as an alternate mechanism to achieve the handshake.
  * event declaration
    event ready, done;
  * event trigger statement
    -> ready;
    -> done;
  * monitoring events
    @(done) <do_something>
  * Asynchronous state machine can also be designed using named events.
* **Heirarchical Path name**
  * Every identifier in Verilog HDL has a unique hierarchical path name. This hierarchical path name is formed by using names separated by a period (.) character.
  * A new hierarchy is defined by:
    1. Module instantiation.
    2. Task definition.
    3. Function definition.
    4. Named block.
  * The complete path name of any identifier starts with the top-level module (a module that is not instantiated by anybody else). This path name can be used in any level in a description.
  * module_instance_name. variable_name
  * For downward path referencing, the model instance must be at the same level as the lower-level module.
* **Sharing Tasks and Functions**
  * One approach to share tasks and functions among different modules is to write the definitions of the shared tasks and functions in a text file, and then include these in the required module using the `include compiler directive.
  * An alternate way to share functions and tasks isto define the shared tasks and functions within a module. And then refer to the required task or function in a different module using a hierarchical name.
* **Value Change Dump (VCD) File**
  * A value change dump (VCD) file contains information about value changes on specified variables in design. Its main purpose is to provide information for other post-processing tools.
  * The following system tasks are provided to create and direct information into a VCD file.
    1. $dumpfiJe : Thissystem task specifies the name of the dump file.
        $dumpfile("uart.dump");
    2. $dumpvars : This system task specifies the variables whose value changes are to be dumped into the dump file.
        $dumpvars; // With no arguments, it specifies to dump all variables in the design.
    3. $dumpvars [level, module_name) ; //Dumps variables in specified module and in all modules the specified no of levels below. Examples
       * $dumpvars (1, UART);  // Dumps variables only in UART module.
       * $dumpvars (2, UART);  // All variables in UART and in all modules one level below.
       * $dumpvars (0, UART);  // Level 0 causes all variables in UART and all variables in all module instances below UART.
       * $dumpvars (0, P_State, N_State) ;  // Dumps info about P_State and N_State variables. The level number is not  relevant in this case, but must be given.
       * $dumpvars (3, Div.Clk, UART); // The level number applies only to modules, in this case, only to UART, that is, all variables in UART and two levels below. Also dumps value changes on variable Div.Clk.
    4. $dumpoff : This system task causes the dumping tasks to be suspended.
    5. $dumpon : This system task causes all dumping tasks to resume.
    6. $dumpall : This system tasks dumps the values of all specified variables at that time, that is at the time it is executed.
    7. $dumplimit : This system task specifies the maximum size (in bytes) for a VCD file. Dumping stops when this limit is reached.
        $dumplimit (1024) ; // VCD file is of maximum 1024 bytes.
    8. $dumpflush : This system task flushes data in the operating system VCD file buffer to be stored in the VCD file. After the execution of the system task, dumping resumes as before.
  * **Format of VCD File**
    * The VCD file is an ASCII file. It has the following information:
    * Header information: Gives date, simulator version and timescale unit.
    * Node information: Definition of the scope and type of variables being dumped.
    * Value changes: Actual value changes with time. Absolute simulation times are recorded.
* **Specify block** to be read
* **Stregths** to be read

* **Compiler Directives:**
  * Identifiers that start with `(backquote) and when compiled, has its effect throughout the compilation process, until a different compiler directive specifies otherwise.
  * `define - Used for text substitution.

    ```verilog
    `define MAX_BUS_SIZE 32
    reg [`MAX_BUS_SIZE - 1:0] var;
    ```

  * `undef - Removes the previously defined.

    ```verilog
    `define WORD 16
    wire [`WORD : 1] var;
    `undef WORD
    ```

  * `ifdef,
  * `else,
  * `endif - These directives are used in conditional compilation.

    ```verilog
    `ifdef signal
    parameter WORD_SIZE =16;
    `else
    parameter WORD_SIZE =32;
    `endif
    ```

    During the compilation, if the text macro signal is defined the corresponding statement executes, and `else is optional.
  * `default_nettype - This is used to specify the **net** type for implicit net declarations.

    ```verilog
    `default_nettype wand
    ```

  * `include - This directive is used to replace the contents of the file in line

    ```verilog
    `include "full_path_of_file"
    ```

  * `resetall - This directive resets all the compiler directives to default. Exanple: causes net type to be wire.

    ```verilog
    `resetall
    ```

  * `timescale - The association of time units is done using this directive, specifies the time units and and time precision.

    ```verilog
    `timescale timeunit/time_precision
    `timescale 10ns/10ps
    ```

    If every is having its own timescale then the least precision is taken into account.
  * `unconnected_drive
  * `nounconnected_drive - All unconnected input ports that appear between these directives are connected to 0 or 1.

    ```verilog
    `unconnected_drive pull1
    `nounconnected_drive
    ```

  * `celldefine
  * `endcelldefine - These will mark the module as cell module(these modules are used by PLI routines for timing analysis etc).

    ```verilog
    `celldefine
    module FD1S3AX {D, CK, Z) ;
    endmodule
    `endcelldefine
    ```
