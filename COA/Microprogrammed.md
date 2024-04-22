- An alternative to a hardwired control unit is a microprogrammed control unit, in which the logic of the control unit is specified by a microprogram.
- A microprogram consists of a sequence of instructions in a microprogramming language. These are very simple instructions that *specify micro-operations*.
- The term microprogram was first coined by M. V. Wilkes in the early 1950s

![[Pasted image 20240421135737.png]]

﻿**Microinstruction (Control Word)**: For each microoperation the CU generates a set of control signals. Thus for any microoperation, each control line emanating from CU is either on or off. This condition can be represented by a binary digit for each control line. *Each row of the previous table represents a microinstruction*. i.e., in a microinstruction, each bit position is fixed for a particular control signal, if a signal is generated in that particular micro-instruction, then that bit postion value will be 1 else it will be 0.

**Microroutine**: A *sequence of control words corresponding to a particular instruction* is called as microroutine for that instruction. The previous table represents the microroutine for the instruction `Add (R3), R1`.

**Control Store (Control Memory)**: The instruction set of any computer is finite. The microroutines for all the instructions in the *instruction set is stored in a special memory* called as Control store/ Control Memory.

#### Format of Microinstruction (Control Word)
![[Pasted image 20240421155211.png]]

- To execute this microinstruction, turn on all the control lines indicated by a bit value1; leave off all control lines indicated by a bit value 0. The resulting control signals will cause one or more *micro-operations* to be performed.
- If the condition indicated by the condition bits is false, execute the next microinstruction in sequence.
- If the condition indicated by the condition bits is true, the next microinstruction to be executed is indicated in the address field.
![[Pasted image 20240421164407.png]]
#### Organization of Control Memory

![[Pasted image 20240421154656.png]]

The fig. shows how the control words or microinstructions could be arranged in a control
memory.

- The microinstructions in each routine are to be executed sequentially.
- Each routine ends with a branch or jump instruction indicating where to go next.
- There is a special execute cycle routine whose only purpose is to signify that one of the machine instruction routines (AND, ADD, and so on) is to be executed next, depending on the current opcode.

### Block Diagram of Microprogrammed CU

![[Pasted image 20240421164545.png]]

To execute any instruction, the CU should first find the starting address of the corresponding microroutine and then can generate the control signals in sequence by reading the control words one by one.

To generate signal memory read operation is performed which makes microprgrammed CU slower than hardwired CU.

﻿The μPC is incremented every time a new microinstruction is fetched from the microprogram memory, except in the following situations:
1. When a **new instruction** is loaded into the IR, the μPC is loaded with the starting address of the microroutine for that instruction.
2. When a **Branch microinstruction** is encountered and the branch condition is satisfied, the μPC is loaded with the branch address.
3. When an **End microinstruction** is encountered the μPC is loaded with the address of the first CW in the microroutine for the instruction fetch cycle.

