# Modelling

* [Gate level modelling](/Verilog/modelling.md#gate-level-modelling)
* [Dataflow Modelling](/Verilog/modelling.md#dataflow-modelling)
* [Behavioral Modelling](/Verilog/modelling.md#behavioral-modelling)
* [Structural Modelling](/Verilog/modelling.md#structural-modelling)

## Gate level modelling

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

## Dataflow Modelling

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

## Behavioral Modelling

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

## Structural Modelling

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
