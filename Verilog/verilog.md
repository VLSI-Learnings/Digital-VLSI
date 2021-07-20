# Verilog

## Introduction

**What is HDL?**

* HDL is a specialized computer language used to describe the structure and behavior of electronic circuits, and most commonly, digital logic circuits.

**Why HDL?**

1. Concurrent execution of statements.
2. The language that converts the program(textual description) to a Netlist(Gate level netlist)
3. Some characteristics like propagation delay, interconnection of parts can’t be captured with traditional languages.

* **Why need of concurrent processng?**
  1. Due to the complexity of digital circuits, we need to process all the instructions at the same time.
  2. To reduce the delay in output.

**What is Verilog HDL?**

* Verilog HDL is a hardware description language that can be used to model a digital system at many levels of abstraction ranging from the algorithmic level to the gate-level to the switch-level.
* The Verilog HDL language was first developed by Gateway Design Automation(acquired by Cadence Design Systems) in 1983.
  **Verilog is case-sensitive**

**Capabilities:**

1. Primitive logic gates, such as and, or and nand, are built-in into the language.
2. Flexibility of creating a user-defined primitive (UDP). Such as primitive could either be a combinational logic primitive or a sequential logic primitive.
3. Switch-level modeling primitive gates, such as pmos and nmos, are also built-in into the language.
4. Hierarchical designs can be described, up to any level, using the module instantiation construct.
5. Notion of concurrency and time can be explicitly modelled.
6. The language is non-deterministic under certain situations, that is, a model may produce different results on different simulators; for example, the ordering of events on an event queue is not defined by the standard.
7. Explicit language constructs are provided for specifying pin-to-pin delays, path delays and timing checks of a design.
8. The capabilities of the Verilog HDL language can be further extended by using the programming language interface (PLI) mechanism.
**PLI is a collection of routines that allow foreign functions to access information within a Verilog module and allows for designer interaction with the simulator.**

* **Design Functionality**
  * The behavioral requirements for the logic ciruit.

## Data types

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
    * The backslash (\\) character can be used to escape certain special characters.
    * \n - newline
    * \t - tab
    * \\ - \ character

    ```verilog
    message = "Internal Error";
    reg [8*14:0] message;
    ```

* An underscore (_) character can be used in an integer or a real constant freely; they are ignored in the number itself. They can be used to improve readability; the only restriction is that the underscore character cannot be the first character.
* A number in base format notation is always an unsigned number.
* If the size specified is larger than the no of bits for the constant, the number is padded to the left with 0's except for the case where the leftmost bit is a x or a z, in which a x or a z respectively is used to pad to the left.
  * For example:
    1. 10'b10 padded with 0 to the left, 0000000010
    2. 10'bx0x1 Paddedwith x to the left, xxxxxxx0x1
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

## Syntax

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

* **Desgin Abstraction Layers**
  * System level
  * RTL level
  * Gate level
* Design styles
  * Top - down
  * Down - top

## Expression and Operators

* Expressions
  * An expression is formed using operands and operators. An expression can be used wherever a value is expected.
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
         * When performing arithmetic operations and assignments, it is important to note which operands are being treated as unsigned values and which are being treated as signed values.
         * An unsigned value is stored in:
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
  * A **scalar expression** is an expression that evaluates to a 1-bit result. If a scalar result is expected but the expression produces a vector result, the least significant bit of the vector is used(the rightmost bit).

## Modelling

* [Gate level modelling](/Verilog/modelling.md#gate-level-modelling)
* [Dataflow Modelling](/Verilog/modelling.md#dataflow-modelling)
* [Behavioral Modelling](/Verilog/modelling.md#behavioral-modelling)
* [Structural Modelling](/Verilog/modelling.md#structural-modelling)

### Gate level modelling

* Built-in primitive gates
  * The following built-in primitive gates are available in Verilog HDL.
    1. Multiple-input gates - These gates have one or more inputs and one output.
       * and, nand, or, nor, xor, xnor
       * gate_type <instance_name> (out,in1,in2,in3)
       * output of these gates is never a Z since the input Z is treated as X.
    2. Multiple-output gates - These gates have only one input and one or more outputs.
       * buf, not
       * gate_type <instance_name> (out1,out2,out3,in1)
       * output of these gates is never a Z
    3. Tristate gates - These gates model three-state drivers. These gates have one output, one data input and one control input.
       * buflf0, bufif1, notif0, notif1
       * tristate_gate [instance_name] (OutputA, InputB, ControlC);
       * Depending on the control input, the output can be driven to the high-impedance state, that is, to value z.
       * For a bufif0 gate, the output is z if control is 1, else data is transferred to output.
       * For a bufif1 gate, output is a z if control is 0.
       * For a notif0 gate, output is at z if control is at 1 else output is the invert of the input data value.
       * For a notif1 gate, output is at z if control is at 0.
    4. Pull gates - These gates have only one output with no inputs.
       * pullup, pulldown
       * A pullup gate places a 1 on its output. A pulldown gate places a 0 on its output.
       * pull_gate [instance_name] ( Outputs );
    5. MOS switches - These gates model unidirectional switches, that is, data flows from input to output and the data flow can be turned off by appropriately setting the control input(s).
       * cmos, nmos, pmos, rcmos, rnmos, rpmos
       * The pmos(p-type MOS transistor), nmos (n-type MOS transistor), rnmos ('r' stands for resistive) and rpmos switches have one output, one input and one control input.
       * gate_type [instance_name] ( Outputs , InputB , ControlC );
       * If control is 0 for nmos and rnmos switches and 1 for pmos and rpmos switches, the switch is turned off, that is, output has value z.
       * If control is 1 for nmos and rnmos switches and 0 for pmos and rpmos switchesdata at input passes to output.
       * The resistive switches (rnmos and rpmos) have a higher impedance(resistance) between the input and output terminals as compared to the non-resistive switches(nmosand pmos). Thus when data passes from input to output, a reduction in strength occurs for resistive switches.
          |pmos/rpmos|||c|||
          |---|---|---|---|---|---|
          |||0|1|x|z|
          |Data|0|0|z|0/z|0/z|
          ||1|1|z|1/z|1/z|
          ||x|x|z|x|x|
          ||z|z|z|z|z|
       * The cmos(complimentary MOS) and rcmos(resistive version of cmos) switches have one data output, one data input and two control inputs.
       * (r)cmos [instance_name] ( OutputA , InputB , NControl , PControl);
    6. Bidirectional switches - data flows both ways and there is no delay when data propagates through the switches.
       * tran, tranif0, tranif1, rtran, rtranif0,rtranif1.
       * The last four switches can be turned off by setting a control signal appropriately. The tran and rtran switches cannot be turned off.
       * The syntax for instantiating a tran or a rtran (resistive version of tran) switch is:
         * (r)tran [ instance_name ] ( SignalA , SignalB);
       * For remaining switches
         * gate__type [ instance_name ] ( SignalA , SignalB , ControlC)
* **Gate Delays**
  * The signal propagation delay from any gate input to the gate output can be specified using gate delay.
    * gate_type [ delay ] [ instance_name ] ( terminal_list );
  * A gate delay can be comprised of up to three values:
    1. rise delay
    2. fall delay
    3. turn-off delay

        ||No delay|1 value(d)|2 values(d1,d2)|3 values(dA,dB,dC)|
        |---|---|---|---|---|
        |rise|0|d|d1|dA|
        |fall|0|d|d2|dB|
        |to_x|0|d|min(d1,d2)|min(dA,dB,dC)|
        |turn-off|0|d|min(d1,d2)|dC|
  * to_x - transition to x delay
  * delay for a gate (including all other delays such as in continuous assignments) can also be specified in a min:typ:max form. The form is minimum : typical : maximum
  * Multiple-input gates, such as and and or, and multiple-output gates (buf and not) can have only up to two delays specified(since output never goes to z).
  * Tristate gates can have up to three delays and the pull gates cannot have any delays.
* **Array of instances**
  * When repetitive instances are requied a range specification can optionally be specified in a gate instantiation.
  * gate_type [ delay ] instance_name [ leftbound : rightbound ] ( list __of_ terminal_names );

### Dataflow Modelling

* Continous Assignment
  * A continuous assignment assigns a value to a net.
  * assign lhs_target = rhs_expression
  * The target in a continuous assignment can be one of the following.
    1. Scalar net
    2. Vector net
    3. Constant bit-select of a vector
    4. Constant part-select of a vector
    5. Concatenation of any of the above
  * These assignments are concurrent in the sense that they are order independent.
* Net Declaration Assignment
  * A continuous assignment can appear as part of a net declaration itself. Such an assignment is called a net declaration assignment.

    ```verilog
    wire clear
    assign clear = 'bl;
    ```

    is equivalent to the net declaration assignment:

    ```verilog
    wire Clear = 'bl;
    ```

  * Multiple net declaration assignments on the same net are not allowed. If multiple assignments are necessary,continuous assignments must be used.

* **Delay**
  * What happens if the right-hand side changes before it had a chance to propagate to the left-hand side? In such a case, the latest value change is applied.
  * If the right-hand side goes from a non-zero value to a zero vector, then fall delay is used.
  * If the right hand side value goes to z, then turn-off delay is used; else rise delay is used.
* **Net delay**
  * A delay can also be specified in a net declaration, such as in the following declaration.

    ```verilog
    wire #5 Arb;
    ```

  * This delay indicates the delay between a change of value of a driver for Arb and the net Arb itself.
  * If a delay is present in a net declaration assignment, then the delay is not a net delay but an assignment delay. In the following net declaration assignment for A, 2 time units is the assignment delay,not the net delay.

    ```verilog
    wire #2 A = B-C
    ```

### Behavioral Modelling

* Procedural Constructs
  * The primary mechanisms for modeling the behavior of a design are the following two statements.
    1. Initial statement
    2. Always statement
  * A module may contain an arbitrary number of initial or always statements. These statements execute concurrently with respect to each other, that is, the order of these statements in a module is not important.
  * **Initial statement**(non synthesizable)
    * This statement executes onl once. It vefins its execution at start of simulation which is at time 0.
    * initial [timing control] procedural_statement
    * An initial statement is mainly used for initialization and waveform generation.
  * **Always statement**(synthesizable)
    * an always statement executes repeatedly, everytime the signal in the sentivity list changes.
    * Since the always statement executes repeatedly and if there is no timing control specified, the procedural assignment statement will loop indefinitely in zero time.
* **Timing controls**
  * A timing control may be associated with a procedural statement. Timing control is of two forms:
    1. **Delay control**
       * A delay control specifies the time duration from the time the statement is initially encountered to the time the statement executes.
       * If the value of the delay expression is 0, then it is called an explicit zero delay.
       * An explicit zero delay causes a wait until all other events that are waiting to be executed at the current simulation time are completed, before it resumes; the simulation time does not advance.
       * If the value of a delay expression is an x or a z, it is the same as zero delay.
       * If the delay expression evaluates to a negative value, the two's complement signed integer value is used as the delay.
    2. **Event control**
       * With an event control, a statement executes based on events. There are two kinds of event control.
         1. Edge-triggered event control
            * @(event) procedural_statement
            * Negative edge
              * 1 -> x, z, 0
              * x, z -> 0
            * Positive edge
              * 0 -> x, z, 1
              * x, z -> 1
         2. Level-sensitive event control
            * In a level-sensitive event control, the procedural statement is delayed until a condition becomes true.
            * wait (condition) procedural_statement
* **Block statement**
  * A block statement provides a mechanism to group two or more statements to act syntactically like a single statement. There are two kinds of blocks in Verilog HDL. These are:
    1. **Sequential block** (delimiters - begin...end): Statements are executed sequentially in the given order.
       * A delay value in each statement is relative to the simulation time of the execution of the previous statement.
    2. **Parallel block**(delimiters - fork...join): Statements in this block execute concurrently.
       * Delay values specified in each statement within a parallel block are relative to the time the block starts its execution.
       * all statements within the parallel block must complete execution before control passes out of the block.
  * A block can be labeled optionally. If so labeled, registers can be declared locally within the block. Blocks can also be referenced.
  * for example, a block can be disabled using a disable statement. A block label, in addition, provides a way to uniquely identify registers.
  * However, there is one word of caution. All local registers are static, that is, their values remain valid throughout the entire simulation run
* **Procedural Assignments**
  * A procedural assignment is an assignment within an initial statement or an always statement.
  * It is used to assign to only a register data type. The righthand side of the assignment can be any expression.
  * There are two kinds of procedural assignments:
    1. Blocking
    2. Non-blocking
  * **Blocking procedural assignment**
    * A procedural assignment in which the assignment operator is an "=" is a blocking procedural assignment.
  * **Non Blocking procedural assignment**
    * A procedural assignment in which the assignment operator is an "<=" is a blocking procedural assignment.

      ```verilog
      initial
      begin
      Cbn <= 0;
      Cbn <= 1;
      end
      ```

    * The value of Cbn after the initial statement executes is 1 since the Verilog HDL standard specifies that all non-blocking assignments to a variable shall occuring the order the assignment statements are executed.Thus, Cbn gets the value 0 first and then 1.
* Difference in PA and CA

  |Procedural assignment|Continuos assignent|
  |---|---|
  |Occurs inside an always statement or an initial statement.|Occurs within a module.|
  |Execution is with respect to other statements surrounding it.|Executes concurrently with other statements; executes whenever there is a change of value in an operand on its right-hand side.|
  |Drives registers.|Drives nets.|
  |Uses "=" or "<=" assignment symbol.|Uses "=" assignment symbol.|
  |No assign keyword (except in a procedural continuous assignment).|Uses **assign** keyword.|

* **Conditional statement**
  * Syntax of an if statement:
    if ( conditional )
      procedural_statement__l
    else if ( condition2 )
      procedural_statement_2
    else
    procedural_statement_3
  * For a nested if the else part belongs to the closest if part.
* **Case statement**
  * A case statement is a multi-way conditional branch.
    case ( case_expr )
    case_item_expr1 : procedural_statement1
    case_item_expr2 : procedural_statement2
    default: procedural_statement
    endcase
  * The set of statements that match the first true condition is executed. Multiple case item expressions can be specified in one branch; these values need not be mutually-exclusive.
  * The default case covers all values that are not covered by the case item expression.
  * What happens if the case expression and the case item expressions are of different lengths? In such a situation, all case expressions are made equal to the largest size of any of these expressions before any comparisons are made.
  * The values x and z are interpreted literally, that is, as x and z values.
  * There are two other forms of case statements: casex and casez,that use a different interpretation for x and z values. The syntax is exactly identical to that of a case statement except for the keywords **casex** and **casez**
  * **casez**
    * the value z that appears in the case expression and in any case item expression is considered as a don't-care, that is, that bit is ignored (not compared).
  * **casex**
    * In a casex statement, both the values x and z are considered as don't-care.
* **Loop statement**
  * There are four kinds of loop staments. They are :
    1. Forever loop
       * someform of timing controls must be used in the procedural statement, otherwise the forever-loop will loop forever in zero delay.
       * forever
          procedural_statement
    2. Repeat loop
       * repeat(loop_count) procedural statement
       * It executes the procedural statement the specified number of times. If loop count expressionis an x or a z, then the loop count is treated as a 0.
    3. While loop
       * while ( condition ) procedural_statement
       * This loop executes the procedural statement until the specified condition becomes false. If the expression is false to begin with, then the procedural statement is never executed.
       * If the condition is an x or a z, it is treated as a 0 (false).
    4. For loop
       * for ( initial_assignment ; condition ;step_assignment)
          procedural_statement
       * A for-loop statement repeats the execution of the procedural statement a certain number of times. The initial_assignment specifies the initial value of the loop index.
       * The condition specifies the condition when loop execution must stop. As long as the condition is true, the statements in the loop are executed.
       * The step_assignment specifies the assignment to modify, typically to increment or decrement, the step count.
* **Procedural Continuous Assignment**
  * A procedural continuous assignment is a procedural assignment, that is, it can appear within an always statement or an initial statement. This assignment can override all other assignments to a net or a register.
  * It allows the expression in the assignment to be driven continuously into a register or a net.
  * There are two kinds of procedural continuous assignments.
    1. assign and deassign procedural statements: these assign to registers.
       * An assign procedural statement overrides all procedural assignments to a register. The deassign procedural statement ends the continuous assignment to a register.
       * If an assign is applied to an already assigned register, it is deassigned first before making the new procedural continuous assignment.
    2. force and release procedural statements: these assign primarily to nets, though they can also be used for registers.
       * The force statement, when applied to a register,causes the current value of the register to be overridden by the value of the force expression. When a release on the register is executed, the current value is held in the register unless a procedural continuous assignment was already in effect (at the time the force statement was executed) in which case, the continuous assignment establishes the new value of the register.
  * The target of a procedural continuous assignment cannot be a part-select or a bit-select of a register.
  * Both the assignments can be used in the same procedural continuous blocks

### Structural Modelling

* Structural modeling is described using:
  * Gate instantiation
  * UDP instantiation
  * Module instantiation
* **Module instantiation**
  * Module - the basic unit in verilog hdl.
  * Port - A port can be declared as input, output or inout. A port by default is a net.
  * A module can be instantiated in another module, thus creating hierarchy. A module instantiation statement is of the form:
    module_name instance_name ( port_associations );
  * Port associations can be by position or by name; however, associations cannot be mixed.
    * port_expr // By position.
    * .PortName ( port_expr ) // By name.
    where port_expr can be any of the following:
    1. an identifier (a register or a net)
    2. a bit-select
    3. a part-select
    4. a concatenation of the above
    5. an expression (only for input ports)
  * Unconnected ports in an instantiation can be specified by leaving the port expression blank.
  * Unconnected ports or unused bits have value z.
* When a module is instantiated in another module, the higher level module can change the value of the parameters in a lower level module. This can be donein two ways.
  1. Defparam statement
  2. Module instance parameter value assignment
* **Defparam statement**
  * A defparam statement is of the form:
    defparam hier_path_namel = valuel ,
              hier_path_name2 = value2;
  * The hierarchical path names of the parameters in a lower level module can be explicitly set by using such a statement.
* **External ports**

## Task & Functions & User Defined Primitives

* [UDP](/Verilog/other_topics.md#udp)
* [Task & Functions](/Verilog/other_topics.md#task-&-functions)

### UDP

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

### Task & Functions

* **Identifiers:**
  * An identifier in Verilog HDL is any sequence of letters,digits,the $ character, and the _ (underscore) character, with the restriction that the first character must be a letter or an underscore.

* **Keywords:**
  * These are predefined identifiers which has specific meaning and used in certain context.
  * initial, always, begin, case, assign, and, or, xor
  * buf, casex, default, assign, bufif0, casez
  * defparam, bufif1, cmos, disable, etc
    **Note that only lowercase are keywords**

* **Comments:**
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

* **Format:** Verilog is free format, statements can be written in a single line or multiple lines.
* **Task:**
  * Task is a reusable code that can be invoked from different parts of the design.
  * A task can return zero or more values and can allows delays(mostly used in test benches).
  * A task can call a another task or a function

    ```text
    task task_id ;
      [ declarations ]
      procedural_statement
    endtask
    ```

  * Task calling
    * A task is called by a task enable statement that specifies the argument values passed to the task and the variables that receive the results. A task enable statement is a procedural statement and can thus appear within an always or an initial statement. It is of the form:

      ```text
      task_id [ ( exprl , expr2 , exprN) ] ;
      ```

    * The list of arguments must match the order of input, output and inout declarations in the task definition. In addition, arguments are passed by value, not by reference.
* **Function:**
  * Function is like task but returns only one value.
  * This executes at zero time, must have atleast one input and doesn't allow delays.
  * A function can call another function but not another task because task has delays.
  * A function definition can appear anywhere in a module declaration. It is of the form:

      ```text
      function [ range ] function_id ;
        input_declaration
        other_declarations
        procedural_statement
      endfunction
      ```

  * An input to the function is declared using the input declaration. If no range is specified with the function definition, then a 1-bit return value is assumed.
  * The function definition implicitly declares a register internal to the function, with the same name and range as the function. A function returns a value by assigning a value to this register explicitly in the function definition.
  * Function calling
    * A function call is part of an expression. It is of the form:

      ```text
      reg_name = func__id ( exprl , expr2 , exprN)
      ```

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
        * $monitoron; // Enables all monitors.
        * These provide a mechanism to control dumping of value changes.
        * The $monitoroff task turns off all monitoring so that no more messages are displayed. The $monitoron task is used to enable all monitoring.
  * **File I/O Tasks**
    * Opening and Closing Files.
    * A system function $fopen is available for opening a file.

      ```text
      integer file_pointer = $fopen ( file_name ); // The $fopen system function returns an integer value (a pointer) to the file.
      ```

    * A system task can be used to close a file.

      ```text
      $fclose (file_pointer);
      ```

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

        ```text
        reg [0:3] Mem_A [0:63];
        initial
          $readmemb ("ones_and_zeros.vec", Mem_A);
        ```

      * Optionally an explicit address can also be specified in the system task call, such as:

        ```text
        $readmemb ("rx.vec", Mem_A, 15, 30);  // The first number read from the file "rx.vec" is stored in address15, next one at 16, and so on until address30.
        ```

  * **Timescale tasks**
    * **$printtimescale** - displays the time unit and time precision for the specified module.
      * The $printtimescale task with no arguments specified prints the time unit and time precision for the module that contains this task call. If a hierarchical path name to a module is specified as its argument, this system task prints the time unit and precision for the specified module.
    * $timeformat - specifies how the %t format specification must report time information.
      * The task is of the form:

        ```text
        $timeformat( units_number , precision , suffix , numeric_field_width );
        $timeformat (-4, 3, "ps", 5) ;
        $display ("Current simulation time is %t", $time);
        ```

      * where a units_number is:

        ```text
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
        ```

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

      ```verilog
      case ( notifier )
        'bx : notifier = 'b0;
        'b0 : notifier = 'bl;
        'bl : notifier = 'b0;
        'bz : notifier = 'bz;
      endcase
      ```

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
