**Peripheral devices**: connect with a computer through several I/O interfaces.

### Why do we need peripherals
﻿

- Peripherals are *electromechanical* or electromagnetic devices, and their manner of operation is different from the operation of the CPU and memory, which are *electronic devices*. So conversion of signal is required.
- Data transfer rate of I/O is *slower* than CPU and memory.
- *Data codes and format in peripherals differ* from the word format in the CPU and memory. So conversion of formats is required.
- The *operating modes* of peripherals are different from each other and each must be controlled so a peripheral does not distrub the operation of other peripherals.

### I/O Interface
![[Pasted image 20240415202616.png]]

- CPU generates address for device. **Address decoder** decodes the address. 
- Data to be transferred in stored temporarily in the **data register**. 
- **Control circuits** controls the operation.
- To indicate the status of our operation (successful/error) the **status register** is used to tell the status of the operation.

### Connection:  Memory Buses vs I/O

#### Separate Buses for both Memory and I/O
![[Pasted image 20240415161145.png]]

#### Same Bus for both Memory and I/O
![[Pasted image 20240415161221.png]]


Control signal will decide which will respond.
**Memory Read**: Memory responds
**I/O Read**: I/O device responds

**Isolated I/O** or **Mapped I/O**: I/O devices and memory have *separate address space*. Control signal distinguishes between the address spaces.

### Common Address, Data Bus and Control Bus
![[Pasted image 20240415162651.png]]

### Common Address, Data Bus and Control Bus
![[Pasted image 20240415162711.png]]
**Memory Mapped I/O**:  From the available memory address space some addresses are reserved for I/O devices. Addresses are from same address space.

![[Pasted image 20240415162210.png]]

### Programmed I/O
![[Pasted image 20240415202731.png]]

**Note**: 
- ![[Pasted image 20240415202844.png]]
- ![[Pasted image 20240415203308.png]]

#### Program

![[Pasted image 20240415205301.png]]

#### Disadvantage:
**Polling method**: Processor waits for response from I/O device. Processor always checks for the vaild keystroke. `BRANCH`es to `WAITK` on `TESTBIT`. During wait period processor is not able to perform useful computation.

### Interrupt Driven I/O

Processor doesn't wait its CPU cycles waiting for I/O devices rather the I/O device sends interrupt `INTR` (interrupt request) to the processor to request an operation. 

#### Sequence of events involved in interrupt handling
1. The device raises an *interrupt request*(INTR).
2. The processor COMPLETES the execution of the current instruction and the program currently being executed is interrupted and saves the *contents of the PC* and *status/flag register*.
(( 3. Interrupts are disabled by clearing the *IF bit* in the status/flag register to *0*. ))
4. The action requested by the interrupt is performed by the interrupt-service routine, during which time the *device is informed that its request has been recognized, and in response, it deactivates the interrupt-request signal*.
5. Upon completion of the interrupt-service routine, the *saved contents of the PC and Status registers are restored* (enabling interrupts by setting the *IF bit to 1*), and execution of the interrupted program is resumed.

#### Example: 
﻿Consider a task that requires continuous extensive computations to be performed and the results to be printed on a printer.
﻿- COMPUTE produces a set of n lines of output.
- PRINT routine sends one line of text to the printer for printing.
- After printing one line printer sends Interrupt request signal to the processor. (At i th line.)
- So processor interrupt the execution of COMPUTE routine and transfers the control to PRINT routine.
![[Pasted image 20240415221512.png]]
#### Terminology
**Interrupt Request (INTR)**:
﻿The I/O devices can alert the processor when it becomes ready. It can do so by sending a hardware signal called an interrupt request to the processor.

**Interrupt Service Routine**:
The routine executed in response to an interrupt is called interrupt service routine. Eg - PRINT routine

**Interrupt Acknowledge (INTA)**:
Processor or interrupt request responds to the interrupted device by sending the control signal called interrupt acknowledge.

**Interrupt Latency**: ﻿
The process of saving and restoring registers involves *memory transfers that increase the total execution time*, and hence represent execution overhead. Saving registers also increases the *delay between the time an interrupt request is received and the start of execution of the interrupt-service routine (ISR)*. This delay is called interrupt latency. This delay must be avoided in real-time-processing.

#### Enabling and Disabling interrupts
﻿A single interrupt request from one device by activating the interrupt-request signal, remains activated until it learns that the processor has accepted its request. This means that the interrupt-request signal will be active during execution of the interrupt-service routine.
It is essential *to ensure that this active request signal does not lead to successive interruptions*, causing the system to enter an infinite loop from which it cannot
recover.
In some situations interrupt have to be ignored e.g. PRINT interrupt from the printer cannot be serviced by the the processor if COMPUTE is not ready with text to print.
##### First Possibility
- The processor hardware ignores the interrupt request line until the execution of the first instruction of the ISR has been completed.
i.e. first instruction of ISR is **Interrupt Disable** and last instruction is **Interrupt Enable**.
![[Pasted image 20240415231604.png]]
- **Interrupt Disable** will set IF flag to 0. 
- **Interrupt enable** will set IF flag to 1. 
##### Second Possibility:
The processor automatically disables interrupts before starting the ISR.
**On entry to an ISR**:
1. Processor first saves the contents of the program counter (PC) and the processor status (PS) register with IF=1 on stack
2. Automatically disable interrupts before starting the execution of the interrupt-service routine.
**On Exit from an ISR**:
When return from interrupt instruction is executed the contents of PC and PS is popped with IF=1 from stack.

##### Third Possibility:
![[Pasted image 20240415233725.png]]
﻿- INTR line must accept only at the *leading edge of the signal* (**Edge triggered line**).
- Processor receives only one request regardless of how long the line is activated.
- So there is no question of multiple interruption or no need of explicit instruction for enable/disable interrupt.
#### Interrupt Hardware
![[Pasted image 20240415215906.png]]
![[Pasted image 20240415215940.png]]
- `INTR` is active low signal.

﻿When a number of devices can send interrupt requests on a common line INTR connected to the processor, How can the processor determine which device is requesting an interrupt?
**Solution**: POLLING
##### Polling
- When an interrupt request is received it is necessary to identify the particular device that has raised the request. The information needed to determine whether a device is requesting an interrupt is available in its status register. When a device raises an interrupt request, it sets one of the bits in it's *status register* to 1(one).
- The simplest way to identify the interrupting device is to have the *ISR poll all the I/O devices connected to the bus*.
- The first device encountered with it's ISR bit set is the device that should be serviced.
- ﻿**Drawback**: is the time spent in interrogating the IRQ bits of all the devices that may not be requesting any service. CPU cycles will be wasted in polling.

Given that different devices are likely to require different interrupt-service-routines how can the processor obtain the starting address of the appropriate routine in each case?
**Solution**: Vectored Interrupt
##### Vectored Interrupt
- To reduce the time involved in the polling process, a *device requesting an interrupt may identify itself directly to the processor*. Then, the processor can immediately start executing the corresponding ISR.
- A device requesting an interrupt can identify itself if it has its own interrupt-request signal, or if it can send a **vector code** (like *memory address of ISR*) to the processor
- A commonly used scheme is to *allocate permanently an area in the memory to hold the addresses of interrupt-service routines*. These addresses are usually referred to as **interrupt vectors**, and they are said to constitute the **interrupt-vector table**.
- ﻿When an interrupt request arrives, the information provided by the I/O device is used as a *pointer into the interrupt-vector table*, and the address in the corresponding interrupt vector is automatically loaded into the program counter.

**Note**:
IO devices send interrupt vector code over the data bus.
When a device sends an interrupt request, the processor may not be ready to receive the interrupt-vector code.
May be interrupt is disabled by the processor at that moment.
or, the data bus may be used by the processor to complete the execution of the current instruction.
So, *when the processor is ready to receive the interrupt vector code, it sends the INTA signal to the device*.
Only after receiving the INTA signal, the device places the interrupt vector code on the bus.

### Nested Interrupts / Simultaneous Interrupt Requests

- ﻿If request comes from more than one device then sometimes some device need immediate response from processor eg: System Clock, Real time system.
- Interrupt requests from higher-priority devices will be accepted.
- This can be resolved by using **Priority Based Interrupt**.
- It uses Multiple-level priority
- Interrupt requests will be accepted from some devices but not from others, depending upon the device's priority. To implement this scheme, we can assign a priority level to the processor that can be changed under program control.
- *Processor's priority can be encoded in a few bits of the processor status register*.

#### Multiple Priority Scheme
![[Pasted image 20240416014028.png]]
**Priority arbitration circuit**: A logic circuit which combines all interrupts but allows only highest priority request.

##### Two types of Priority Scheme 
1. **Fixed Priority**: Lower number indicates highest priority.
2. **Rotating Priority**: Once one device is serviced, that device will become the lowest priority device and the device next in sequence will become the highest priority device.

#### Daisy chain
![[Pasted image 20240416014514.png]]
﻿- INTR line is common to all the devices.
- INTA is connected in daisy chain fashion where *INTA signal propagates serially* through the devices.

#### Multiple Priority and Daisy Chain
- Combines the multiple priority and daisy chain.
- Devices are organized into group and each have different priority level.
![[Pasted image 20240416014831.png]]
