# Task & Functions

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
