- It is a special purpose CPU register. This register is used to keep track of information about the results of various operations.
- These information is used by the following(subsequent) conditional branch instruction to decide whether to take a branch or not.
- Like after an arithmetic operation is performed, whether the result is zero/ negative/ carry is generated/ overflow occurred, these various conditions are trapped inside the flag register. And looking at the content of flag register, branch has to be taken or not can be decided.
- Basically, a flag register contains a collection of bits, where each bit position is an indication of a particular condition that may occur due to an instruction is executed.

![[Pasted image 20240421190922.png]]
![[Pasted image 20240421191003.png]]
![[Pasted image 20240421191147.png]]

![[Pasted image 20240421191606.png]]

![[Pasted image 20240421191753.png]]

