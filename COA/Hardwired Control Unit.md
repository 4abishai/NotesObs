- Hardwired system can operate at high speed; but with little flexibility.
- It is a CU design that uses combinational and sequential circuits like gates, FFs, decoder, encoder and other digital circuits.
- For CU to perform it's function, it has some inputs that allows it to determine the state o the system and outputs that allows it to control the behaviour of the system.
- These inputs are the external specification of the CU.
- Internally, the CU must have some logic to perform its sequencing and execution function.
### Block Diagram for Hardwired Control Unit
![[Pasted image 20240420203004.png]]

Takes **four** Inputs to CU:
1. Clock (Contents of control step counter)
2. Instruction Register (Contents of IR)
3. Flags(Contents of condition codes)
4. Control Signals from External Bus (e.g., MFC)

External Inputs: MFC

Run = 1 => Increment the step
Run = 0 => Keep the step same (used when step waits for MFC)

End = 1 => Reset the counter
End = 0 => Don't do anything

### Logic function
#### Generating Zin control signal

Steps used in generating a control signal: (e.g., Zin)
1. Find out the control sequence for the instructions supported by the ISA.
2. Next find out, in which instructions the signal is appearing and then find out the step number of that instruction the signal is appearing.
3. Say, Zin is appearing in the 6th step of the instruction Add (R3), R1. It means Zin signal need to be generated for the step no 6 of ADD instruction, so when both the cases are true, Zin need to be generated, like that for JMP L1 instruction, Zin is generated in step no 4. i.e., in either of the two instructions Zin signal need to be generated.
4. So, the logic function, for Zin will be OR of the above two AND cases. `Zin=ADD.T6+ JMP.T4+....................................`indicates other possible
5. Again, we hve seen that Zin is required for all the instructions in the step no 1 during the fetch phase of any instruction, i.e., irrespective of any instruction, in the step no 1 Zin required. So, `Zin=TI+ADD.T6+ JMP.T4+...................................`

![[Pasted image 20240420225458.png]]
![[Pasted image 20240420231421.png]]

![[Pasted image 20240420231509.png]]

#### Logic Function for End signal
![[Pasted image 20240420231629.png]]

![[Pasted image 20240420233928.png]]
![[Pasted image 20240420234002.png]]

`S5 = T1 + I2.T3 + I4.T3`
`S10 = I1.T4 + I2.(T2 + T5) + I3.T4 + I4(T3 + T5)

Step decoder 3 : 8
as T1, T2,...T5 (8 - 5 = 3 lines will be ignored)

Instruction decoder 2 : 4
as I1, I2, I3, I4

![[Pasted image 20240420235817.png]]

Step decoder 4 : 16

Instruction decoder 6 : 64

![[Pasted image 20240421015726.png]]
![[Pasted image 20240421015819.png]]

