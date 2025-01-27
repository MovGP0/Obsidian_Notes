## Analog Circuits

### General

| Acronym    | Meaning                           | Typical Use                                                                                  |
| ---------- | --------------------------------- | -------------------------------------------------------------------------------------------- |
| $V_{IN}$   | Input Voltage                     | Voltage applied to the input of a circuit or device.                                         |
| $V_{OUT}$  | Output Voltage                    | Voltage available at the output of a circuit or device.                                      |
| $V_{REF}$  | Reference Voltage                 | A stable voltage used as a reference in ADCs, DACs, or other circuits.                       |
| $V_{PP}$   | Peak-to-Peak Voltage              | The difference between the maximum positive and maximum negative amplitude of a signal.      |
| $V_{MAX}$  | Maximum Voltage                   | The maximum voltage a circuit or component can handle.                                       |
| $V_{RMS}$  | Root Mean Square Voltage          | The effective voltage of an AC signal, equivalent to a DC voltage delivering the same power. |
| $V_{L}$    | Voltage Load                      | Voltage measured across a load.                                                              |
| $V_{D}$    | Voltage Drop                      | Voltage drop across a component, such as a diode or resistor.                                |
| $V_{CCIO}$ | Voltage for Input/Output Circuits | Power supply voltage for the I/O circuitry in digital systems.                               |
| $V_{BAT}$  | Battery Voltage                   | Voltage supplied by a battery, often in portable devices.                                    |
| $V_{SSIO}$ | Source Voltage for I/O            | Ground or negative supply for input/output circuitry.                                        |
| $V_{CAP}$  | Capacitor Voltage                 | Voltage across a capacitor, often used in filtering or decoupling applications.              |
| $GND$      | Ground                            | Common reference point in a circuit, typically representing 0V.                              |

### Bipolar Junction Transistor (BJT)

| Acronym    | Meaning                           | Typical Use                                                                                  |
| ---------- | --------------------------------- | -------------------------------------------------------------------------------------------- |
| $V_{CC}$   | Voltage at the Common Collector   | Positive supply voltage for BJTs, often tied to the collector in NPN configurations.         |
| $V_{EE}$   | Voltage at the Common Emitter     | Negative supply voltage for BJTs, often tied to the emitter in PNP configurations.           |
| $V_{BE}$   | Voltage Base-Emitter              | The voltage difference between the base and emitter of a BJT.                                |
| $V_{CE}$   | Voltage Collector-Emitter         | The voltage difference between the collector and emitter of a BJT.                           |

### Field Effect Transistor (FET)

| Acronym    | Meaning                           | Typical Use                                                                                  |
| ---------- | --------------------------------- | -------------------------------------------------------------------------------------------- |
| $V_{DD}$   | Voltage at the Drain              | Positive supply voltage for FETs (e.g., MOSFET circuits).                                    |
| $V_{SS}$   | Voltage at the Source             | Ground or the most negative supply voltage in FET circuits.                                  |
| $V_{GS}$   | Voltage Gate-Source               | The voltage difference between the gate and source of a FET.                                 |
| $V_{DS}$   | Voltage Drain-Source              | The voltage difference between the drain and source of a FET.                                |
| $V_{TH}$   | Threshold Voltage                 | The minimum gate-to-source voltage required to turn on a FET.                                |
| $V_{DDIO}$ | Voltage for Drain I/O             | Input/output voltage supply for FET-based systems.                                           |

## Digital Circuits

| Acronym   | Meaning                     | Typical Use                                                                    |
| --------- | --------------------------- | ------------------------------------------------------------------------------ |
| $CLK$     | Clock                       | Synchronization signal used to coordinate operations in sequential circuits.   |
| $D$       | Data Input                  | Input line for data in D flip-flops or shift registers.                        |
| $Q$       | Output                      | Main output of flip-flops or latches.                                          |
| $\bar{Q}$ | Inverted Output             | Complementary output of flip-flops or latches.                                 |
| $RST$     | Reset                       | Resets a circuit to its initial state.                                         |
| $SET$     | Set                         | Forces a circuit to a predefined state (often 1).                              |
| $EN$      | Enable                      | Enables or disables a circuit's operation, e.g., in counters or registers.     |
| $LD$      | Load                        | Signal to load data into a register or shift register.                         |
| $SR$      | Shift Right                 | Shift operation moving bits to the right (in shift registers).                 |
| $SL$      | Shift Left                  | Shift operation moving bits to the left (in shift registers).                  |
| $MSB$     | Most Significant Bit        | The highest-order bit in a binary number.                                      |
| $LSB$     | Least Significant Bit       | The lowest-order bit in a binary number.                                       |
| $FF$      | Flip-Flop                   | A basic storage element in digital circuits.                                   |
| $SR\;FF$  | Set-Reset Flip-Flop         | A flip-flop with Set and Reset inputs.                                         |
| $D\;FF$   | Data Flip-Flop              | Stores the value of the Data input on a clock edge.                            |
| $T\;FF$   | Toggle Flip-Flop            | Toggles the output state on every clock edge if enabled.                       |
| $JK\;FF$  | JK Flip-Flop                | A versatile flip-flop with Set, Reset, and Toggle functionality.               |
| $MUX$     | Multiplexer                 | Selects one input from many inputs to pass to the output.                      |
| $DEMUX$   | Demultiplexer               | Routes one input signal to one of many outputs.                                |
| $ALU$     | Arithmetic Logic Unit       | Performs arithmetic and logical operations.                                    |
| $ROM$     | Read-Only Memory            | Non-volatile memory used to store fixed data or instructions.                  |
| $RAM$     | Random Access Memory        | Volatile memory used for temporary data storage.                               |
| $SRAM$    | Static RAM                  | High-speed, volatile memory used in caches.                                    |
| $DRAM$    | Dynamic RAM                 | Slower but more compact volatile memory used for main system memory.           |
| $FIFO$    | First In, First Out         | A type of memory buffer where data exits in the order it was entered.          |
| $LIFO$    | Last In, First Out          | A type of memory buffer where the most recently added data is retrieved first. |
| $ADC$     | Analog-to-Digital Converter | Converts analog signals to digital data.                                       |
| $DAC$     | Digital-to-Analog Converter | Converts digital data to analog signals.                                       |
