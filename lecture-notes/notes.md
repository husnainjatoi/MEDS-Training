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
