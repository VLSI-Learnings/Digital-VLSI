# Syntax

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
