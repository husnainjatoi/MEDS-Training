# Digital Design and Computer Architecture - Lectures Summaries

---

## Lecture 1: Intro: Fundamentals, Transistors, Gates

### Goals of this course
- Understand the basics
- Understand the priniciples of design
- Understand the precedents
- Learn how a modern computer works
- Learn how to implement and debug complex sysstems

### The Transformation Hierarchy
- Top to Bottom Layers: Problem -> Algorithm -> Program/language -> System Software -> Software Hardware Interface -> Micro-architecture -> Logic -> Devices -> Electrons

- Narrow View: SW/HW Interface -> Micro-architecture

- Expanded View: Algorithm -> Program/language -> System Software -> Software Hardware Interface -> Micro-architecture -> Logic -> Devices

### Computer Architecture
- Science and Art of designing computing platforms.
- Includes Hardware, Interface, System software and programming model.

### What is a Computer?
- Three key components
1. Computation
2. Communication
3. Storage/Memory

### General Purpose vs Special Purpose Systems
- CPU: Flexible but not the best performance and efficiency
- GPU, FPGAs & ASICs: Efficient & high performance but difficult to program & use (inflexible)

### Transistors
#### MOS Transistor:
- By Combining
1. Conductors (**M**etal)
2. Insulators (**O**xide)
3. **S**emiconductors
- Used to realize logic gates
- Two Types: n-type & p-type

#### CMOS Transistor:
- nMOS + pMOS = CMOS
- Modern computers use CMOS technology
- n-type good at *pulling down* voltage
- p-type good at *pulling up* voltage

---
## Lecture 2: Combinational Logic

### CMOS Gate Structure
- If both networks ON -> short circuit (incorrect operation)
- If both networks OFF -> floating output or undefined (may be acceptable)

### Power Consumption
1. Dynamic Power Consumption:
- Power used to charge capacitance as signals change (0 <-> 1)
- C * $V^2$ * f \
C = capacitance of circuit, V = supply voltage, f = charging frequency of the capacitor

2. Static Power Consumption:
- Power used when signals don't change
- V * I(leakage)

3. Energy Consumption:
- Power * Time

### Moore’s Law
- Transistor count doubles every ~2 years
- Enables advances in computing power and cost efficiency

### Logic Circuits
- Composed of:
1. Inputs
2. Outputs

- Functional Specification (describes relationship between inputs and outputs)

- Timing Specification (describes the delay between inputs changing and outputs responding)

#### Types of Logic Circuits
##### Combinational Logic
- Memoryless
- Outputs strictly dependent on inputs

##### Sequential Logic
- Has memory (can store data values)
- Outputs determined by previous and current values of inputs

### Boolean Algebra
- Algebra on 1s and 0s using AND, OR, and NOT operations.

- **Goal:** Minimize boolean expressions to reduce hardware cost, area, latency, and energy. Enables automatic hardware synthesis.

- **De Morgan's Law:** Useful for converting between logic functions (e.g., changing an OR gate to an AND gate with inverted inputs).

### Standardized Function Representations
#### Sum of Products (SOP) Form
- Function is the OR (sum) of all input combinations that result in a 1.
- **Minterm:** A product that includes all input variables. Evaluates to true for that specific row.
- Leads to a structured two-level logic implementation (AND gates followed by an OR gate).

#### Product of Sums (POS) Form
- Function is the AND (product) of all input variable combinations that result in a 0.
- **Maxterm:** A sum that includes all input variables. Evaluates to zero for that specific row.

### Combinational Building Blocks
#### Decoders
- **Input pattern detector.**
- Has N inputs and $2^N$ outputs.
- Only one output evaluates to 1 depending on the specific input pattern.
- Common uses: Address decoding in memory, Opcode decoding for processor instructions.

#### Multiplexers
- **Selector.** Selects one of the N data inputs to connect to the output based on a control/select input line.
- **Lookup Tables:** Multiplexers can be hardwired or programmed to perform arbitrary logic functions. This is the foundational principle for reconfigurable hardware like FPGAs.

#### Adders
- **Full Adder:** Computes a 1-bit binary addition (Sum and Carry Out) using inputs A, B, and Carry In.
- **Ripple Carry Adder:** Chains multiple 1-bit full adders. Simple, but has high latency because the carry must ripple sequentially through each bit.
- **Carry Lookahead Adder:** Accelerates carry generation using specialized boolean logic blocks to bypass the slow serial ripple effect.

#### Programmable Logic Array
- Enables a hardware two-level SOP implementation of any function.
- Contains an array of AND gates and an array of OR gates.
- Features programmable connections from the inputs to the AND gates (generating minterms), and from the AND gates to the OR gates.

---

## Lecture 3: Sequential Logic

### Logical Completeness
- A set of gates is **logically complete** if it can implement any truth table.
- {AND, OR, NOT} is logically complete.
- NAND by itself is logically complete.
- NOR by itself is logically complete.

### Combinational Blocks
#### Equality Checker (Comparator)
- Checks if two n-bit values are exactly the same.
- Built using XNOR gates for bitwise comparison (outputs 1 if bits match).
- An n-input AND gate evaluates the final equality (evaluates to 1 only if all bits are equal).

#### Arithmetic Logic Unit (ALU)
- Combines arithmetic (Addition, Subtraction) and logic (AND, OR) operations into a single execution module.
- Performs one function at a time, dictated by a multi-bit function input.
- Uses internal decoders and multiplexers to execute and route the selected operation to the output.

#### Tri-State Buffers
- Acts as a switch to control data flow onto shared wires.
- **States:**
  1. Enable = 1: Switch is closed. Output driven by the input value.
  2. Enable = 0: Switch is open. Output is *floating*.
- **Shared Bus Application:** Allows multiple components (e.g., CPU, Memory, Ethernet) to connect to a single shared bus. Control logic ensures only one component's tri-state buffer is enabled at any given time to prevent short circuits and collisions.
- Can also be used to build efficient multiplexers.

### Logic Simplification and Automation
- EDA tools automate logic simplification to optimize latency, area, and power.
- Uniting Theorem: If changing a variable's value does not change the function's output, that variable can be eliminated.
- Don't Cares (X): Used in truth tables to indicate that an input's value does not affect the output. Simplifies boolean expressions heavily (e.g., used to simplify Priority Circuits).

### Introduction to Sequential Logic
- **Combinational Logic:** Output depends *only* on current inputs.
- **Sequential Logic:** Output depends on current inputs AND past inputs. Contains memory/storage elements.

### Storage Elements
#### Cross-Coupled Inverters
- The most basic storage element. Two inverters wired in a loop.
- Has two stable states (storing a 0 or a 1) and one unstable (metastable) state.
- Problem: Cannot be explicitly set or rewritten because there is no control mechanism. Used internally in fast, expensive SRAM caches.

#### SR Latch
- Built using two cross-coupled NAND gates.
- States:
  - `S=1, R=1`: Quiescent state. Holds previous value ($Q = Q_{prev}$).
  - `S=0, R=1`: Sets Q to 1.
  - `S=1, R=0`: Resets Q to 0.
  - `S=0, R=0`: Forbidden State. Violates the fundamental rule that Q and Q` must be complements, and transitioning out of it can cause prolonged oscillation (metastability).

#### Gated D Latch
- Solves the SR Latch's forbidden state issue.
- Introduces a **Write Enable (WE)** and a **Data (D)** input.
- Uses extra NAND gates to guarantee S and R are never 0 at the same time.
- If `WE=1`: Q gets the value of D (it becomes "transparent").
- If `WE=0`: Q holds its previous value securely.

#### Registers and Memory Arrays
- Register: Multiple Gated D latches placed in parallel (sharing a single Write Enable) to store multi-bit values simultaneously.
- Memory Array: Built using multiple registers.
  - Address Space: The unique locations in memory. **Addressability:** The number of bits stored per location.
  - Address Decoder: Selects which register to read from or write to based on the provided address bits.
  - Multiplexer: Routes the selected register's data to the memory output.

### Finite State Machines (FSMs) and The Clock
- A discrete-time model of a stateful system.
- Consists of: Finite states, external inputs, external outputs, explicit state transitions, and output generation logic.
- Built using a **State Register** (sequential), **Next State Logic** (combinational), and **Output Logic** (combinational).
- **Asynchronous vs. Synchronous:**
  - Asynchronous: Transitions happen whenever inputs change (prone to race conditions and bugs).
  - Synchronous: State transitions are synchronized globally by a **Clock** signal.

### Edge-Triggered State Elements
- The Problem with Latches in FSMs: A D-Latch is *level-triggered*. If the clock is high, the output continuously tracks the input. This causes uncontrolled feedback loops in state machines, as the state would change randomly during a single cycle.
- D Flip-Flop:
  - Created by cascading two D Latches in a Master-Slave configuration with inverted clock inputs.
  - Edge-Triggered: Samples the input D *only* at the rising edge of the clock.
  - Provides a stable output throughout the entire clock cycle, making it the essential storage element for State Registers in modern synchronous hardware.

---

## Lecture 4: Sequential Logic II, Labs, Verilog

### Finite State Machine (FSM) Implementation
- Transition Table: It defines the current state, input conditions, and the resulting next state.
- State Encoding: Assigning binary values to represent each state.
  - *Full/Binary Encoding:* Uses the minimum number of bits ($log_2(N)$ bits for N states). 
  - *One-Hot Encoding:* Uses one bit per state. Only one bit is high at any time.
  - *Output Encoding:* Applicable primarily to Moore machines. States are encoded using the desired output bits directly. 
- **FSM Circuit Schematic:** Comprises three parts:
  1. Next State Logic: Combinational logic that calculates the next state.
  2. State Register: Sequential logic that holds the current state and updates it at the clock edge.
  3. Output Logic: Combinational logic that generates the outputs based on the current state (Moore) or current state + inputs (Mealy).

### Moore vs. Mealy FSM Trade-offs
- **Moore Machine:**
  - Output depends *only* on the current state.
  - Usually requires more states than a Mealy machine.
  - Advantage: Outputs are generally more stable and synchronous with the clock. Less prone to glitches because the output logic path is isolated from immediate input changes.
- **Mealy Machine:**
  - Output depends on the current state AND the current inputs.
  - Can often achieve the same behavior with fewer states.
  - Disadvantage: A glitch (short unstable pulse) on the input can propagate directly to the output. Additionally, it creates longer combinational logic paths from input to output, which can hurt timing performance.

### FPGAs (Field Programmable Gate Arrays)
- **Concept:** A software-reconfigurable hardware substrate.
- **Key Components:**
  - Lookup Tables (LUTs): Small programmable memories that can implement any boolean function of N inputs.
  - Switch Boxes/Interconnects: Programmable routing pathways that connect LUTs and other blocks.
  - I/O Blocks: Configurable pins to interface with external components (LEDs, switches, etc.).
- **Advantages:** High performance, high energy efficiency, low development cost compared to ASICs, short time-to-market, and highly flexible/reusable.
- **Disadvantages:** Less power efficient and slower than custom-designed ASICs. The reconfigurability introduces significant area and latency overhead.

### Hardware Description Languages (HDLs) - Verilog
- **Design Paradigms:**
  - Top-Down: Define the top-level module (e.g., CPU), break it into sub-modules (ALU, Control Unit), and continue down to basic primitives.
  - Bottom-Up: Build primitive gates, combine them into basic blocks (adders, multiplexers), and assemble those into complex systems.
- **Verilog Basics:**
  - **Module:** The fundamental building block.
  - Defined using the `module` and `endmodule` keywords.
  - Includes a port list specifying `input` and `output` signals.
  - **Multi-bit Signals:** Defined using ranges, e.g., `input [31:0] a;` creates a 32-bit input vector.

  ---
## Lecture 5: HDL, Verilog II, Timing & Verification

### Verilog Modeling Styles
1. Structural (Gate-Level) Modeling:
   - Defines a module by instantiating sub-modules, primitives (like `and`, `or`, `not`), and defining the explicit wiring between them.
   - Example primitive instantiation format: (e.g., `and G1 (Y, A, B);`).
2. Behavioral Modeling:
   - Uses functional and mathematical descriptions (equations, `if-else` blocks, `case` statements) rather than physical gate connections.
   - Utilizes keywords like `assign` for continuous assignments.
   - Synthesizers translate this high-level logic into gate-level structures automatically.

### Bit Manipulation and Operators in Verilog
- Bit Slicing: Assigning specific portions of a larger bus to a smaller bus (e.g., `assign short_bus = long_bus[12:5];`).
- Concatenation: Combining multiple signals into a larger bit vector using (e.g., `assign Y = {A2, A1, A0, A0};`).
- Duplication: Replicating a signal multiple times within a vector using (e.g., `assign X = {4{A0}};`).
- Reduction Operators: Condensing an entire bit vector into a single bit output using a specific logic operation (e.g., `assign Y = &A;` ANDs all bits of vector A together).
- Conditional Assignment: Functions like a ternary operator/multiplexer (e.g., `assign Y = S ? D1 : D0;`).

### Expressing Numbers
- Format: `<size>'<base><value>`
- **Size:** Number of bits. (Default is 32 bits if unspecified).
- **Base:** `b` (binary), `h` (hexadecimal), `d` (decimal), `o` (octal).
- **Value:** The numerical value. Can include `x` (invalid/don't care) or `z` (floating/high impedance).
- Example: `8'b1001` evaluates to `00001001` (padded with leading zeros).

### Defining Sequential Logic in Verilog
- Combinational logic concepts are insufficient to describe memory elements like latches and flip-flops, as those depend heavily on clock latency and edge triggers.
- **The `always` Block:**
  - Used heavily to model sequential logic. 
  - Triggers execution when signals in its **sensitivity list** change state.
  - Syntax: `always @(posedge clk or negedge reset) begin ... end`.
  - **Memory Inference:** The synthesizer infers a latch or flip-flop if the output is not fully defined across all possible input conditions.
  - **Registers (`reg`):** Any variable assigned a value *inside* an `always` block must be declared as a `reg`.

### Blocking vs. Non-Blocking Assignments
- **Blocking (`=`):**
  - Evaluates and assigns sequentially.
  - Used mostly for complicated combinational logic within an `always` block.
- **Non-Blocking (`<=`):**
  - Evaluates all right-hand expressions first, then updates all left-hand variables concurrently at the end of the block.

### Finite State Machines (FSMs) in Verilog
- Typically broken down into three distinct blocks (often implemented as a mix of `always` and `assign` blocks):
  1. **State Register:** A synchronous `always` block (triggered by `posedge clk`) that updates the current state to the next state, and handles resets.
  2. **Next State Logic:** A combinational `always @(*)` block utilizing `case` statements to calculate the next state. A `default` case is vital to handle unused encodings to prevent hardware lockup or unintended latches.
  3. **Output Logic:** Typically an `assign` statement or combinational `always` block that determines the output based on the current state (Moore) or current state + inputs (Mealy).

---
## Lecture 6: Timing & Verification II

### Combinational Circuit Timing
- Outputs are delayed and transitions are gradual.
- Sources of Delay:
  - Capacitance and Resistance (RC Delay): Transistors act as resistors when on, and wires/components have parasitic capacitance.
  - Physical Factors: Speed of light limits signal propagation. Temperature, supply voltage, implementation style, and aging all affect delays.
  - Input Characteristics: Different input transitions (0->1 vs. 1->0) have different latencies.

### Delay Definitions
- Contamination Delay: The minimum amount of time after an input changes until the output *starts* to change (becomes unstable).
  - Calculated using the shortest path through the combinational logic.
  - Used conservatively to ensure stability against rapid consecutive changes.
- Propagation Delay: The maximum amount of time after an input changes until the output *finishes* changing (becomes stable).
  - Calculated using the critical path (longest path) through the logic.
  - Determines the maximum operational speed of the circuit.

### Glitches
- A brief, unintended spike or drop.
- Cause: Occurs when an input drives the output through multiple paths with different delays (e.g., a fast path and a slow path).
- Impact: Glitches consume unnecessary dynamic power but eventually settle to the correct steady-state value.
- Fixing Glitches: Can be prevented by adding redundant logic gates. In most sequential designs, glitches are safely ignored if they settle before the clock edge.

### Sequential Circuit Timing constraints
- Data input to a flip-flop must be completely stable around the active clock edge.
- Setup Time: The time before the clock edge that data must be stable.
  - Fix: If the combinational logic is too slow (violates setup time), you can easily fix it by reducing the clock frequency (increasing $T_{cycle}$).
- Hold Time: The time after the clock edge that data must remain stable.
  - Violation Fix: If the combinational logic is too fast (violates hold time). The designer must artificially increase the delay of the shortest path (e.g., by adding buffers).

### Clock Skew
- Definition: The time difference between clock edges arriving at different flip-flops across the chip due to physical routing delays in the clock network.
- Impact:
  - Increases the effective setup time.
  - Increases the effective hold time.
- Result: Decreases the useful work done per cycle (sequencing overhead). Requires intelligent clock routing networks (e.g., H-trees) to minimize skew.

### Circuit Verification
- Verifying a large design takes up to 70% of the total design time.
- Testbenches: A Verilog module written specifically to test a Device Under Test (DUT).
  - Simple: Manually input vectors, manually check output waveforms. (Error-prone, unscalable).
  - Self-Checking: Hardcode input vectors and expected outputs in the testbench.
  - Test Vector Files: Read inputs and expected outputs from an external file on simulated clock edges.
  - Automatic Testbench (Golden Model): Compares the DUT's output against a "Golden Model" (a highly abstract, known-bug-free model written in C/C++ or high-level Verilog). Highly scalable and automated.
- Post-Synthesis Timing Simulation: Tools like Vivado synthesize the HDL into real logic gate models with annotated worst-case delays (min, typical, max) to simulate and catch real-world timing violations.
