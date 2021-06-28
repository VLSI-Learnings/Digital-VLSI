# Verilog HDL

* [Y's](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#ys)
* [Chapter 1](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-1) - Introduction
* [Chapter 2](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-2) - Data types
* [Chapter 3](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-3) - Modelling
* [Chapter 4](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-4) - Syntax
* [Chapter 5](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-5) - Gate level modelling

## Y's

**What is HDL?**

* HDL is a specialized computer language used to describe the structure and behavior of electronic circuits, and most commonly, digital logic circuits.

**Why HDL?**

1. Concurrent execution of statements.
2. The language that converts the program(textual description) to a Netlist(Gate level netlist)
3. Some characteristics like propagation delay, interconnection of parts can’t be captured with traditional languages.
   * **Why need of concurrent processng?**
   1. Due to the complexity of digital circuits, we need to process all the instructions at the same time.
   2. To reduce the delay in output.

## Chapter 1

**What is Verilog HDL?**

* Verilog HDL is a hardware description language that can be used to model a digital systema many levels of abstraction ranging from the algorithmic level to the gate-level to the switch-level.
* The Verilog HDL language was first developed by Gateway Design Automation(acquired by Cadence Design Systems) in 1983.
  **Verilog is case-sensitive**

**Capabilities:**

1. Primitive logic gates, such as and, or and nand, are built-in into the language.
2. Flexibility of creating a user-defined primitive (UDP). Such as primitive could either be a combinational logic primitive or a sequential logic primitive.
3. Switch-level modeling primitive gates, such as pmos and nmos, are also built-in into the language.
4. Hierarchical designs can be described, up to any level, using the module instantiation construct.
5. Notion of concurrency and time can be explicitly modeled.
6. The language is non-deterministic under certain situations,that is, a model may produce different results on different simulators; for example, the ordering of events on an event queue is not defined by the standard.
7. Explicit language constructs are provided for specifying pin-to-pin delays, path delays and timing checks of a design.
8. The capabilities of the Verilog HDL language can be further extended by using the programming language interface (PLI) mechanism.
   **PLI is a collection of routines that allow foreign functions to access information within a Verilog module and allows for designer interaction with the simulator.**

## Chapter 2

**Vaule Set:**

* The 4 basic values in Verilog HDL are
  * 0 :logic-0orfalse
  * 1: logic-1 or true
  * x: unknown
  * z: high-impedance
* x and z are case insensitive.
* Constants are 3 types - Integer, Real, String.
  * Integer - represented in two formats
    * Simple decimal
      * 30, -15
    * Base format
      * [size]'base value
        * size - specifies the size of the constant in no of bits.
        ex: 4'b0101
      * Not legal formats
        * Space between ' character and base
        * Size can't be expression - (2+1)'b00000
        * value can't be negative
  * Real
    * Decimal format
      * 1.5, 2,670
    * Scientific format
      * 14_3.1e2 (=14310)(underscore is ignored)
      * 4e-2 (0.04)
  * String - A sequence of characters within the double quotes.
    * A character is represented by an 8-bit ASCII value which is treated as an unsigned integer.
    * The backslash (\) character can be used to escape certain special characters.
    * \n - newline
    * \t - tab
    * \\ - \ character

    ```verilog
    message = "Internal Error";
    reg [8*14:0] message;
    ```

* An underscore (_) character can be used in an integer or a real constant freely; they are ignored in the number itself. They can be used to improve readability; the only restriction is that the underscore character cannot be the first character.
* A number in base format notation is always an unsigned number.
* If the size specified is larger than the no of bits for the constant, the number is padded to the left with O's except for the case where the leftmost bit is a x or a z, in which a x or a z respectively is used to pad to the left.
  * For example,
10'b10 Padded with 0 to the left, 0000000010
10'bx0x1 Paddedwith x to the left, xxxxxxx0x1
* If the size specified is smaller, then the leftmost bits are appropriately truncated.

**Data Types:**

* Verilog HDL has two groups of data types.

* **Net**
  * Represents the physical connection between the elements.
  * Its value is determined from the value of its drivers such as a continuous assignment or a gate output.
  * If no driver is connected to net, default value is z.
  * Net default size is 1 bit.
  * wire, tri, wor, trior, wand, triand, trireg, tril, tri0, supply0, supply 1
    Syntax

    ```verilog
    net_type [msb:lsb] var
    ```

  * wire and tri are similar types but tri is used when multiple drivers drive a net.

    | wire(or tri) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | x | x | 0 |
    | 1 | x | 1 | x | 1 |
    | x | x | x | x | x |
    | z | 0 | 1 | x | z |
  * wor and trior

    | wor(or trior) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | 1 | x | 0 |
    | 1 | 1 | 1 | 1 | 1 |
    | x | x | 1 | x | x |
    | z | 0 | 1 | x | z |
  * wand and triand

    | wand(or triand) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | 0 | 0 | 0 |
    | 1 | 0 | 1 | x | 1 |
    | x | 0 | x | x | x |
    | z | 0 | 1 | x | z |
  * Trireg net
    * This net stores value and is used to model a capacitive node.
    * Default value of trireg net is an x.
  * Tri0 & Tri1
    * Thses nets also model wired logic nets.
    * The particular characteristic of a tri0 (or 1) is, if no driver is driving this net, its value is 0(or 1).

    | tri0(or tri1) | 0 | 1 | x | z |
    | :-: | - | - | - | - |
    | 0 | 0 | x | x | 0 |
    | 1 | x | 1 | x | 1 |
    | x | x | x | x | x |
    | z | 0 | 1 | x | 0(1) |
  * Supply0 & Supply1
    * The supplyO net is used to model ground, that is, the value 0, and the supplyl net is used to model a power net, that is, the value 1.
  * If there are any undeclared nets that can set to default net type by using complier directive `default_nettype.

  ```verilog
    * `default_nettype wand
  ```

  * Vectored & Scalared nets
    * If a net is declared with the keyword vectored, then bit-selects and part-selects of this vector net are not allowed.
    * For scalar those are allowed.

* **Register** - represents an abstract data  storage element.
  * It is assigned values only within an always statement or an initial statement, and its value is saved from one assignment to the next.
  * A register type has a default value of x.
  * The value in a register is always interpreted as an unsigned number.
  * There are five different kinds of register types:
    * reg
      * Default size is 1 bit.
      * Memory is an array of registers.

        ```verilog
        reg [msb:lsb]mem1[upperlimit:lowerlimit]
        ```

        ```verilog
        parameter ADDR_SIZE =16, WORD_SIZE = 8;
        reg [1: WORD_SIZE] RamPar [ADDR_SIZE-1 : 0] , DataReg;
        ```

        * RamPar is a memory, an array of sixteen 8-bit registers, while DataRegis a 8-bit register.
      * A memory cannot be assigned a value in one assignment(means value should be assigned to each location), but a register can.
      * To assign values to memory is b using the system tasks:
        1. $readmemb (loads binary values)
        2. $readmemh (loads hexadecimalvalues)
      * Thesesystem tasks read and load data from a specifiedtext file into a memory.
      * The text file must contain the appropriate form of numbers, either binary or hexadecimal.

        ```verilog
        reg [4:0]mem[15:0];
        $readmemb("file.txt",mem)
        ```

    * integer
      * default size of the integer is 32bits.
      * An integer register holds signed quantities and arithmetic operations provide 2's complement arithmetic results.
      * converting an integer to a bit-vector can be accomplished by simply using an assignment.
      * Type conversionis automatic. No specific functions are necessary.
    * time
      * is used to store and manipulate time values.
      * default size is 64  bits
      * A tme register holds only an unsigned quantity.
    * real
    * realtime
      * real and realtime registers are identical.
      * The default value of a real register is 0. No range,bit range or word range, is allowed for declaring a real register.
      * When assigning values x and z to a realregister,these values are treated as O.
* Parameter
  * Paramter is a constant, used to specify delays and widths of variables.
  * The value of the parameter can be overridden by
    * using a defparam statement
    * or by specifying the parameter value in the module instantiation statement.

## Chapter 3

**Module:**

* The basic unit of description in HDL is the module.
* A module describes the functionality or structure of a design and also describes the ports through which it communicates externally with other modules.
  Syntax of the module:

  ```verilog

    module module_name ( port_list ) ;
    //Declarations:
    input i1;
    output o1;
    inout x1;
    reg, wire, parameter,
    .
    //statements
    .
    endmodule
  ```

* Primitives
  * Switch level primitives
  * Gate level primitives
  * User defined primitive

**Assignment Statements(AS):**

* Continous AS
  * The logic derived from the RHS drives the net in LHS.
  * The target of the CAS is always a net driven by combo logic.
* Procedural AS
  * The logic derived from the RHS drives the variable on the LHS.
  * These appear in always block or initial block.
  * There are two kinds of PAS :
    * Blocking
      * The execution of the statements is seqential(The assignment to LHS is before the execution of the next assignment).
      * This is used in combinational logic.
    * Non Blocking
      * The execution of the statements is parallel(The assignment to LHS is end of the cycle).
      * This is used in Sequential logic.

**Delay:**

* Interdelay - The time by which the execution of the statement is delayed.

  ```verilog
  assign #10 a=b^c;
  ```

* Intradelay - The time by which the assignment of the RHS to the LHS is delayed.

  ```verilog
  assign a=#10 b^c;
  ```

**Modelling Styles:**

A design can be modeled in three different styles or in a mixed style. These styles are:

* **Dataflow modelling**
  * The basic mechanism used to modela design in the dataflow style is the continuous assignment.
  * In a continuous assignment, a value is assigned to a net.

    ```text
    assign [delay] LHS=RHS;
    ```

* **Behavioral modelling**
  * Design is modelled using procedural assignments.
  * This has two statements.
    * Initial statement - Executes only once.

        ```verilog
        initial begin
        clk=1'b0;
        end
        ```

    * Always statement - Executes in a loop based on the sensitivity list(**sentivity list** - variables that are required to execute the block when they change).

        ```verilog
        always @ ("sentivity list") begin
        //statements
        end
        ```

* **Structural modelling**
  * This is modelled using primitives.
    * Built-in gate primitives (at the gate-level
    * Switch-levelprimitives (at the transistor-level)
    * User-defined primitives (at the gate-level
    * Module instances (to create hierarchy)

    ```verilog
    and a1(out, in1, in2)
    fulladder f1(sum, carry, a, b, c)
    ```

* **Mixed modelling**
  * The design is made using of all the above styles.

* **Switch level modelling**
  * used in the simulation of MOS transistor circuit

* **Simulating a Design**

  * Verilog HDL provides capabilities not only to escribe a design but also to model stimulus, control, storing responses and verification, all using the same language. Stimulus and control can be generated using initial statements.

  * **Test Bench**
    * The stimulus (i.e.. the logic values of the inputs to a circuit) that tests the functionality of the design is called a Test bench

## Chapter 4

* Expressions
  * An expression is formed using operands and operators.An expression can be used wherever a value is expected.
  * An operand can be one of the following:
    1. Constant
       * The negative value of an integer with no base specifier is treated as a signed value, while an integer with a base specifier is treated as an unsigned value.
    2. Parameter
    3. Net
       * A value in a net is interpreted as an unsigned value, in the continuous assignment.
    4. Register
       * A value in an integer register is interpreted as a signed 2's complement number,
       * while a value in a reg register or a time register is interpreted as an unsigned number.
       * Values in real and realtime registersare interpreted as signed floating point values.
    5. Bit-select
       * A bit-select extracts a particular bit from a vector.
    6. Part-select
       * In a part-select, a continuous sequence of bits of a vector is selected
    7. Memory element
       * A memory element selects one word of a memory
       * No part-select or bit-select of a memory is allowed.
       * to read a bit-select or a part-select of a word in memory is to assign the memory element to a register and then use a part-select or a bit-select of this register.
    8. Functional call
       * A function call can be used in an expression. It can either be a system function call (starts with the $ character) or a user-defined function call.
* **Operators**
  * Operatorsin Verilog HDL are classified into the following categories:
    1. **Arithmetic operators**

        |Symbol|operation|
        |-|-|
        |+|Unary plus|
        |-|Unary minus|
        |+|Binary plus|
        |-|Binary minus|
        |*|Multiply|
        |/|Divide|
        |%|Modulus|
       * The % (modulus) operator gives the remainder with the sign of the first operand.
       * If any bit of an operand in an arithmetic operation is an x or a z, the entire result is an x.
       * The size of the result of an arithmetic expression is determined by the size of the largest operand.
       * In case of an assignment, it is determined by the size of the left-hand side target as well.
       * Unsigned & signed
         * When performing arithmetic operations and assignments, it is important to note which operands are being treated as unsigned values and which are being treated as signed values. * An unsigned value is stored in:
           * a net
           * a reg register
           * an integer in base format notation
         * A signed value is stored in:
           * an integer register
           * an integer in decimal form
    2. **Relational operators**

        |Symbol|operation|
        |-|-|
        |<|Less than|
        |<=|Less than or equal to|
        |>|Greater than|
        |>=|Greater than or equal to|
        * The result of a relational operator is true(the value 1) or false (the value 0).
        * Result is an x if any bit in either of the operands is an x or a z
    3. **Equality operators**

        |Symbol|operation|
        |-|-|
        |==|Logical equality|
        |!=|Logical inequality|
        |===|Case equality|
        |!==|Case inequality|
        * In case comparisons, values x and z are compared strictly as values, that is, with no interpretations, and the result can never be an unknown,
        * while in logical comparisons, values x and z have their usual meaning and the result may be unknown; that is, for logical comparisons if either operand contains an x or a z, the result is the unknown value (x).

          ```verilog
          a=4'b11x0
          b=4'b11x0
          a==b   //gives x
          a===b  //gives 1
          ```

        * If the operands are of unequal lengths, the smaller operand is zero-filled on the most significant side.
    4. Logical operators

        |Symbol|operation|
        |-|-|
        |&&|Logical and|
        |\|\||Logical or|
        |!|Unary logical negation|
        * For vector operands, a non-zero vector is treated as a 1.
        * If a bit in any of the operands is an x, the result is also an x.
    5. Bitwise operators

        |Symbol|operation|
        |-|-|
        |&|Bit-wise and|
        |^|Bit-wise xor|
        |^~ or ~^|Bit-wise xnor|
        |\||Bit-wise or|
        |~|Unary bit-wise negation|
        * These operators operate bit-by-bit, on corresponding bits of the input operands and produce a vector result.
    6. Reduction operators

        |Symbol|operation|
        |-|-|
        |&|Reduction and|
        |~&|Reduction nand|
        |^|Reduction xor|
        |^~ or ~^|Reduction xnor|
        |\||Reduction or|
        |~\||Reduction nor|
        * The reduction operators operate on all bits of a single operand and produce a 1-bit  result.
        * The operators are:
          * & (reduction and): If any bit is 0, the result is 0,else if any bit is an x or a z, the result is an x, else the result is a 1.
          * ~& (reduction nand): Invert of & reduction operator.
          * | (reduction or): If any bit is a 1,the result is 1,else if any bit is an x or a z, the result is an x, else the result is 0.
          * ~| (reduction nor): Invert of | reduction operator.
          * ^ (reduction xor): If any bit is an x or a z, the result is an x, else if there are even number of l's in the operand,the result is 0, else the result is 1.
          * ~^ (reduction xnor): Invert of ^ reduction operator.
    7. Shift operators

        |Symbol|operation|
        |-|-|
        |<<|Left shift|
        |>>|Right shift|
        * The shift operation shifts the left operand by the right operand number of times. It is a logical shift. The vacated bits are filled with 0. If the right operand evaluates to an x or a z, the result of the shift operation is an x.
    8. Conditional operators

        |?:|Conditional operator|
        |-|-|
        * cond_expr ? exprl : expr2
        * If cond_expr is true (that is, has value 1), exprl is selected,if cond_expr is false (value 0), exprl is selected.If cond_expr is an x or a z, the result is a bitwise operation on exprl and exprl.
    9. Concatenation and replication operator

        |{}|concatenation|
        |-|-|
        * Concatenation of unsized constant numbers is not allowed as the size of these numbers are not known.
  * All operators associate left to right except for the conditional operator that associates right to left.
* Kinds of expressions
  * A **constant expression** is an expression that evaluates to a constant value at compile time. More specifically, a constant expression can be made up of:
    1. constant literals, such as 'b10 and 326
    2. parameter name
  * A scalar expression is an expression that evaluates to a 1-bit result. If a scalar result is expected but the expression produces a vector result, the least significant bit of the vector is used(the rightmost bit).

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
