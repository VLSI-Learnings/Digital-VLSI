# Sequential circuits
* The circuit whose output depends not only on the present state inputs and also on the previous state of the storage elements.

## Asynchronous Sequential circuits
* sequential circuit depends upon the input signals at any instant of time and the order in which the inputs change.

## Synchronous Sequential circuits
* a system whose behavior can be defined from the knowledge of its signals at discrete instants of time.

* **Latches**
	* Storage elements that are operated with signal levels
	* sr latch, d latch(transparent latch)
	
* Problem with the latch is when the control signal is active level according to the input the output changes, but as long as the input changes when the control signal is active, the output will be changed.
* One way is to employ two latches in a special configuration that isolates the output of the flip-flop and prevents it from being affected while the input to the flip-flop is changing.
* Another way is to produce a flip-flop that triggers only during a signal transition (from 0 to I or from 1 100) of the synchronizing signal(clock) and is disabled during the rest of the clock pulse. 
* **Flipflop**
	* Storage elements that are operated with signal transitions are referred as Flipflops
* Characteristic table defines the logical properties of a flip-flop by describing its operation in tabular form.
* Characteristic equation - the algebric equation derived from the logical properties described in the characteristic table.
* Asynchronous inputs(direct inputs)
	* used to force the flip-flop to a particular state independently of the clock.(The direct input are useful for bringing all the flip-flops in the system to a starting known state prior to clock operation)
	* The input that sets the flip-flop to 1 is called preset or direct set.
	* The input that sets the flip-flop to 0 is called clear or direct reset.

* **Input equations**(excitation equations): The part of the circuit that generates the inputs to flip-flops is described algebraically by a set of Boolean functions.
* **Output equations**: The part of the combinational circuit that generates external outputs is described algebraically by a set of Boolean functions.

* ANALYSIS OF CLOCKED SEQUENTIAL CIRCUITS
	* Analysis describes what a given circuit will do under certain operating conditions
	* A state table and state diagram are then presented to describe the behavior of the sequential circuit
* State equation:
	* (also called as transition equation)specifies the next state as a function of the present state and inputs.
* State table:
	* (transition table) gives the information about the next state and the output determined from the present state and inputs.
	* a sequential circuit with m flip-flops and n inputs need 2^(m+n) rows in the state table.
* State diagram:
	* The information available in a state table can be represented graphically in the form of a state diagram.
	
* Finite State Machines
	* Mealy model: the output is a function of both the present state and the input.
	* Moore model: the output is a function of only the present state.

* State reduction:
	* The reduction in the number of flip-flops in a sequential circuit.
	* State-reduction algorithms are concerned with procedures for reducing the number of states in a state table, while keeping the external input-output requirements unchanged.
	* Two states are said to be equivalent if, for each member of the set of inputs, they give exactly the same output and send the circuit either to the same state or to an equivalent state.
	
	|Present State | Next State| Output|
	|--|--|--|
	||x = 0 x = 1 |x = 0 x= 1|
	|a a b 0 0|
	|b c d 0 0|
	|c a d 0 0|
	|d e f 0 1|
	|e a f 0 1|
	|f s f 0 1|
	|g a f 0 1|
	* here e and g states are equivalent as next state and outputs are same.
	* However, the fact that a state table has been reduced to fewer states does not guarantee a saving in the number of flip-flops or the number of gates.
	* One-hot encoding usually leads to simpler decoding logic for the next state and output.
	* One-hot machines can he faster than machines with sequential binary encoding. and the silicon area required by the extra flip-flops can be offset by the area saved by using simpler decoding logic.
	* This trade-off is not guaranteed, so it must be evaluated for a given design.

* The procedure for designing synchronous sequential circuits can be summarized by the following recommended steps:
	1. From the word description and specifications of the desired operation, derive a state diagram for the circuit.
	2. Reduce the number of states if necessary.
	3. Assign binary values to the states.
	4. Obtain the binary-coded state table .
	S. Choose the type of flip-flops to be used.
	6. Derive the simplified flip-flop input equations and output equations.
	7. Draw the logic diagram.