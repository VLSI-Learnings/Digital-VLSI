# Registers & Memory

## All about the memory elements

* [Chapter 1](/Digital%20Electronics/Topics/register.md#chapter-1)

### Chapter 1

* **Binary cell** - A electronic physical device that has two stable states and capable of storing one bit.
* ***Register transfer*** operation is a basic operation that consists of a transfer of binary information from one set of registers into another set of registers.
* Counter is essentially a register that goes through a predetermined sequence of binary states.
* **Register** - A group of binary cells(flip flop)
* Synchronous digital systems have a master clock generator that supplies a continuous train of clock pulses. The pulses are applied to all flip-flops and registers in the system.
* The transfer of new information into a register is referred to as **loading**.
	* If all the bits of the register are loaded simultaneously with a common clock pulse, it is **parallel loading**.
	
* **Shift register**:
	* A register capable of shifting the binary information held in each cell to its neighboring cell, in a selected direction.
	* A register capable of shifting in one direction only is a unidirectional shift register.
	* One that can shift in both directions is a bidirectional shift register.
	* If the register has both shifts and parallel-load capabilities, it is referred to as a universal shift register.

* The most general shift register has the following capabilities:
	l. A clear control to clear the register to 0.
	2. A clock input to synchronize the operations.
	3. A shift-right control to enable the shift-right operation and the serial input and output lines associated with the shift right.
	4. A shift left control to enable the shrtt-lett operat ion and the serial input and output lines associated with the shift left.
	5. A parallel-load control to enable a parallel transfer and the n input lines associated with the parallel transfer.
	6. n parallel output lines.
	7. A control state that leaves the information in the register unchanged in response to the clock. Other shift registers may have only some of the preceding functions, with at least one shift operation.

* Counter
	* A register that goes through a prescribed sequence of states upon the application of input pulses is called a counter.
	* An n-bit binary counter consists of n flip-flops and can count in binary from 0 through 2^n-1.
	* Ripple counter:
		* a flip-flop output transition serves as a source for triggering other flip-flops.
	* Synchronous counter:
		* same clock signal is used to trigger all the flip-flops.
	* Counter with unused states
		* divide by N(modulo N)
	* A ring counter is a circular shift register with only one flip-flop being set at any particular time; all others are cleared.
	* A switch-tail ring counter is a circular shift register with the complemented output of the last flip-flop connected to the input of the first flip-flop.

* **Memory**
	* A memory unit is a device to which binary information is transferred for storage and from which information is retrieved when needed for processing.
	* Random Access Memory
		* RAM stores new information for later use.
		* The process of storing new information into memory is referred to as a memory write operation.
		* The process of transferring the stored information out of memory is referred to as a memory read operation.
		* RAM can perform both write and read operations
	* Read Only Memory
		* ROM can perfonn only the read operation.
		* ROM is an example of PLD.

* RAM
	* A memory unit stores binary information in groups of bits called Words.
	* A word in memory is an entity of bits that move in and out of storage as a unit.
	* The number of address bits needed in a memory is dependent on the total number of words that can be stored in the memory and is independent of the number of bits in each word.

* Timing waveforms:
	* The access time of memory is the time required to select a word and read it.
	* The cycle time of memory is the time required to complete a write operation.


* Types of memory
	* RAM units are available in two opereting modes: static and dynamic.
	* Static RAM:
		* (SRAM) consists essentially of internal latches that store the binary information.
		* The stored information remains valid as long as power is applied to the unit.
	* Dynamic RAM:
		* (DRAM) stores the binary information in the form of electric charges on capacitors provided inside the chip by MOS transistors.
		* The stored charge on the capacitors tends to discharge with time, and the capacitors must be periodically recharged by refreshing the dynamic memory.
	* Memory units that lose stored information when power is turned off are said to be volatile.
		* CMOS integrated circuit RAMs, both static and dynamic. are of this category, since the binary cells need external powe r to maintain the stored information.
	* Nonvolatile memory, such as magnetic disk, retains its stored information after the removal of power.
	* This type of memory is able to retain information because the data stored on magnetic components are represented by the direction of magnetization, which is retained after power is turned off.

* Memory decoding
	* In addition to requiring storage components in a memory unit, there is a need for decoding circuits to select the memory word specified by the input address.
	* coincidence decoding - instead of having a decoder with inputs as many as the no of address lines, can have 2 decoders each with half no of address lines.

* Address multliplexing
	* reducing the no of address input pins accommodates the address components.
	* Row address strobe (RAS) enables the row register, and the column address strobe (CAS) enables the column register.


* Error detection and correction:
	* The reliability of a memory unit may be improved by employing error-detecting and error-correcting codes.
	* The most common error detection scheme is the parity bit.
	* An error-correcting code generates multiple parity check bits thai are stored with the data word in memory.
	* Each check bit is a parity over a group of bits in the data word. When the word is read back from memory, the associated parity bits are also read from memory and compared with a new set of check bits generated from the data that have been read.
	* If the check bits are correct, no error has occurred.
	* If the check bits do not match the stored parity, they generate a unique pattern, called a **syndrome**, that can be used to identify the bit that is in error.
	* A single error occurs when a bit changes in value from 1 to 0 or from 0 to 1 during the write or read operation.
	
* **Hamming code**
	* 


* ROM:
	* A ROM is essentially a memory device in which permanent binary information is stored.
	* a 2^k X n ROM will have an internal k X 2^k decoder and n OR gates. Each OR gate has 2^k inputs, which are connected to each of the outputs of the decoder.
	* A programmable connection between two lines is logically equivalent to a switch that can be altered to be either closed (meaning that the two lines are connected) or open (meaning that the two lines are disconnected).
	* The programmable intersection between two lines is sometimes called a crosspoint.
	* The required paths in a ROM may be programmed in four different ways.
	* The first is called **mask programming** and is done by the semiconductor company during the last fabrication process of the unit.
	* second type of ROM called programmable read-only memory, or **PROM**.
		* PROM units contain all the fuses intact, giving all 1's in the bits of the stored words.
	* A third type of ROM is the erasable PROM or **EPROM**
		* which can be restructured to the initial state even though it has been programmed previously.
		* When the EPROM is placed under a special ultraviolet light for a given length of time, the shortwave radiation discharges the internal floating gates that serve as the programmed connections.
	* The fourth type of ROM is the electrically erasable PROM (EEPROM or E2PROM ).
		*This device is like the EPROM, except that the previously programmed connections can be erased with an electrical signal instead of ultraviolet light.
		* The advantage is that the device can be erased without removing it from its socket.

* **Combinational PLDs**
	* The PROM is a combinational programmable logic device (PLD}-an integrated circuit with programmable gates divided into an AND array and an OR array to provide an AND-OR sum of product implementation.


* PROM
	* Fixed AND array and programmable OR array
* PAL
	* Programmable AND array and fixed OR array
* PLA
	* Programmable AND array and programmable OR array

* **Sequential Programmable Devices**
	*  devices include both gates and flip-flops.
	1. Sequential(or simple) programmable logic device (SPLD)
	2. Complex programmable logic device (CPLD)
	3. Field-programmable gate array (FPGA)