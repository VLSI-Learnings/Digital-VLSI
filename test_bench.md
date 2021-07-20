# Test bench and its components

* **Test bench**
  * The stimulus (i.e.. the logic values of the inputs to a circuit) that tests the functionality of the design is called a Test bench
  * It is a container where the design is placed and driven with different input stimulus.
  * **Purpose**
    * Generate different types of input stimulus
    * Drive the design inputs with the generated stimulus
    * Allow the design to process input and provide an output
    * Check the output with expected behavior to find functional defects
    * If a functional bug is found, then change the design to fix the bug
    * Perform the above steps until there are no more functional defects

## SV test bench

* **Components**
  * **Generator** - Generates different input stimulus to be driven to DUT
  * **Interface** - Contains design signals that can be driven or monitored
  * **Driver** - Drives the generated stimulus to the design
  * **Monitor** - Monitor the design input-output ports to capture design activity
  * **Scoreboard** - Checks output from the design with expected behavior
  * **Environment** - Contains all the verification components mentioned above
  * **Test** - Contains the environment that can be tweaked with different configuration settings
