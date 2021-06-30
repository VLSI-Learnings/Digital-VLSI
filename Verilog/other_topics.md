# Task & Functions & User Defined Primitives

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

**Task:**

* Task is a reusable code that can be invoked from different parts of the design.
* A task can return zero or more values and can allows delays(mostly used in test benches).

**Function:**

* Function is like task but returns only one value.
* This executes at zero time and doesn't allow delays.

**System Task and Functions:** - An identifier starting with **$** character is system task of function.

```verilog
$display, $monitor
```

**Compiler Directives:**

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
