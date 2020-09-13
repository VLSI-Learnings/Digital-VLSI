# Verilog HDL Primer by J B

* [Y's](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#definitions)
* [Chapter 1](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-1) - Introduction
* [Chapter 2](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-2) - Syntax
* [Chapter 3](https://github.com/VLSI-Learnings/Digital-VLSI/blob/master/verilog_primer.md#chapter-3) - Modelling

## Y's

**What is HDL?**\
HDL is a specialized computer language used to describe the structure and behavior of electronic circuits, and most commonly, digital logic circuits.

**Why HDL?**

1. Concurrent execution of statements.
2. The language that converts the program(textual description) to a Netlist(Gate level netlist)
3. Some characteristics like propagation delay, interconnection of parts can’t be captured with traditional languages.
    **Why need of concurrent processng?**
      1. Due to the complexity of digital circuits, we need to process all the instructions at the same time.
      2. To reduce the delay in output.

## Chapter 1

**What is Verilog HDL?**

* Verilog HDL is a hardware description language that can be used to model a digital systema many levels of abstraction ranging from the algorithmic level to the gate-level to the switch-level.
* The Verilog HDL language was first developed by Gateway Design Automation(acquired by Cadence Design Systems) in 1983.

**Capabilities:**

1. Primitive logic gates, such as and, or and nand, are built-in into the language.
2. Flexibility of creating a user-defined primitive (UDP). Such as primitive could either be a combinational logic primitive or a sequential logic primitive.
3. Switch-level modeling primitive gates, such as pmos and nmos, are also built-in into the language.
4. Hierarchical designs can be described, up to any level, using the module instantiation construct.
5. Notion of concurrency and time can be explicitly modeled.
6. The language is non-deterministic under certain situations,that is, a model may produce different resultson different simulators; for example, the ordering of events on an event queue is not defined by the standard.
7. Explicit language constructs are provided for specifyingpin-topin delays, path delays and timing checks of a design.
8. The capabilities of the Verilog HDL language can be further extended by using the programming language interface (PLI) mechanism. **PLI is a collection of routines that allow foreign functions to access information within a Verilog module and allows for designer interaction with the simulator.**

## Chapter 2

**Module:**

* The basic unit of description in HDL is the module.
* A module describes the functionality or structure of a designand also describes the ports through which it communicates externally with other modules.
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

**Data Types:**

* Net -
* Reg -

## Chapter 3

**Assignment Statements(AS):**

* Continous AS
  * The logic derived from the RHS drives the net in LHS.
  * The target of the CAS is always a net driven by combo logic.

* Procedural AS
  * The logic derived from the RHS drives the variable on the LHS.
  * These always appear in always block.
  * There are two kinds of PAS :
    * Blocking
      * The execution of the statements is seqential(The assignment to LHS is before the execution of the next assignment).
      * This is used in combo logic.
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

* **Dataflow style**
  * The basic mechanism used to modela design in the dataflow style is the continuous assignment.
  * In a continuous assignment, a value is assigned to a net.

    ```text
    assign [delay] LHS=RHS;
    ```

* **Behavioral style**
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

* **Structural style**
  * This is modelled using primitives.
    * Built-in gate primitives (at the gate-level
    * Switch-levelprimitives (at the transistor-level)
    * User-defined primitives (at the gate-level
    * Module instances (to create hierarchy)

    ```verilog
    and a1(out, in1, in2)
    fulladder f1(sum, carry, a, b, c)
    ```

* **Mixed style**
  * The design is made using of all the above styles.
