# Modelling

* [Gate level modelling](/Verilog/modelling.md#Gate level modelling)

## Gate level modelling

* Built-in primitive gates
  * The following built-in primitive gates are available in Verilog HDL.
    1. Multiple-input gates - These gates have one or more inputs and one output.
       * and, nand, or,nor,xor,xnor
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
       * For a bufifO gate, the output is z if control is 1, else data is transferred to output.
       * For a bufifl gate, output is a z if control is 0.
       * For a notifO gate, output is at z if control is at 1 else output is the invert of the input data value.
       * For a notifl gate, output is at z if control is at 0.
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
