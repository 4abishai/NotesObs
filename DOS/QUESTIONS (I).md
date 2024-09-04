![[Pasted image 20240829014441.png]]
I understand you'd like help with this problem about an ATM system transmitting cells at the OC-3 rate. I'll be happy to assist you while being careful not to reproduce any copyrighted material.

The problem asks us to calculate the fraction of CPU devoted to interrupt handling for two scenarios:
1. When each packet is 48 bytes long
2. When each packet is 1024 bytes long

To solve this, we'll need to:
1. Determine the OC-3 transmission rate
2. Calculate how many cells are transmitted per second
3. Determine how much CPU time is spent on interrupts per second
4. Calculate the fraction of CPU time devoted to interrupt handling

Let's start with the first scenario (48-byte packets):

1. The OC-3 rate is approximately 155.52 Mbps (megabits per second).

2. For 48-byte packets:
   Cells per second = (155.52 * 10^6 bits/s) / (48 * 8 bits/cell) = 405,000 cells/s

3. Each interrupt takes 1 μs (microsecond), so:
   Time spent on interrupts = 405,000 * 1 μs = 405,000 μs/s = 0.405 s/s

4. Fraction of CPU time = 0.405 / 1 = 0.405 or 40.5%

For the second scenario (1024-byte packets):

2. For 1024-byte packets:
   Cells per second = (155.52 * 10^6 bits/s) / (1024 * 8 bits/cell) = 18,984.375 cells/s

3. Time spent on interrupts = 18,984.375 * 1 μs ≈ 0.018984375 s/s

4. Fraction of CPU time ≈ 0.018984375 / 1 ≈ 0.019 or 1.9%

In conclusion, for 48-byte packets, about 40.5% of the CPU is devoted to interrupt handling. For 1024-byte packets, only about 1.9% of the CPU is devoted to interrupt handling.

This demonstrates how packet size can significantly affect the CPU overhead for interrupt handling in network systems.
To solve this problem, we'll break it into steps:

### Step 1: Understand the OC-3 Rate
An OC-3 (Optical Carrier 3) line has a data transmission rate of 155.52 Mbps (megabits per second).

### Step 2: Determine the Number of Packets per Second for 48-byte Packets
- The size of each packet is 48 bytes.
- Convert 48 bytes to bits:
  \[
  48 \text{ bytes} = 48 \times 8 = 384 \text{ bits}
  \]
  
- Now, calculate how many packets are transmitted per second:
  \[
  \text{Packets per second} = \frac{155.52 \times 10^6 \text{ bps}}{384 \text{ bits per packet}} \approx 404,167 \text{ packets per second}
  \]

### Step 3: Time Per Packet
- The time per packet is the inverse of the number of packets per second:
  \[
  \text{Time per packet} = \frac{1}{404,167} \approx 2.474 \times 10^{-6} \text{ seconds per packet} = 2.474 \mu s
  \]

### Step 4: Interrupt Time and CPU Usage for 48-byte Packets
- An interrupt takes 1 µs.
- The fraction of the CPU time devoted to handling interrupts is the ratio of the interrupt time to the time per packet:
  \[
  \text{Fraction of CPU} = \frac{1 \mu s}{2.474 \mu s} \approx 0.404
  \]
  Therefore, about **40.4%** of the CPU is devoted to handling interrupts for 48-byte packets.

### Step 5: Recalculate for 1024-byte Packets
- Convert 1024 bytes to bits:
  \[
  1024 \text{ bytes} = 1024 \times 8 = 8192 \text{ bits}
  \]

- Calculate how many packets are transmitted per second:
  \[
  \text{Packets per second} = \frac{155.52 \times 10^6 \text{ bps}}{8192 \text{ bits per packet}} \approx 18,990 \text{ packets per second}
  \]

- Time per packet:
  \[
  \text{Time per packet} = \frac{1}{18,990} \approx 5.266 \times 10^{-5} \text{ seconds per packet} = 52.66 \mu s
  \]

- The fraction of CPU time devoted to handling interrupts:
  \[
  \text{Fraction of CPU} = \frac{1 \mu s}{52.66 \mu s} \approx 0.019
  \]
  Therefore, about **1.9%** of the CPU is devoted to handling interrupts for 1024-byte packets.

### Summary
- For 48-byte packets, about 40.4% of the CPU time is used for interrupt handling.
- For 1024-byte packets, about 1.9% of the CPU time is used for interrupt handling.
![[Pasted image 20240829014605.png]]
In an ATM (Asynchronous Transfer Mode) network, each ATM cell has a 5-byte header, and errors in the header can be detected through the use of a Header Error Control (HEC) field. This field consists of an 8-bit cyclic redundancy check (CRC) that protects the header's 4 preceding bytes.

If the ATM cell header becomes completely garbled (i.e., random bits), the probability that it will be accepted as correct depends on the chance that a random sequence of bits happens to satisfy the CRC check in the HEC field.

Since the HEC is 8 bits long, there are 28=2562^8 = 25628=256 possible combinations of the HEC field. Therefore, for a garbled header, the probability that the random bits match a valid HEC codeword by pure chance is:

P(HEC check passed)=1256=0.0039P(\text{HEC check passed}) = \frac{1}{256} = 0.0039P(HEC check passed)=2561​=0.0039

Thus, the probability that a completely random ATM header will pass the error detection and be accepted as correct is approximately **0.39%**.

![[Pasted image 20240829014840.png]]
This problem revolves around parameter passing techniques, specifically call-by-reference and copy/restore (also known as call-by-value-result). Here's how the behavior of the procedure will differ for each case:

### 1. **Call-by-Reference:**
- In this method, both `i` parameters refer to the same memory location. Any changes made to one will affect the other since they are essentially the same variable.
- The function `incr(i, i)` is called with two references to `i`. When the procedure increments the first parameter by 1, the variable `i` becomes 1. Then, when it increments the second parameter (which is also `i`), the value increases by another 1, making `i = 2` at the end of the procedure.
- **Final value of `i`: 2**

### 2. **Copy/Restore (Call-by-Value-Result):**
- In this method, the parameters are passed by value, meaning that each parameter gets its own copy of the value of `i`. At the end of the procedure, the updated values are copied back into the original variable.
- Initially, both parameters receive a copy of `i` (which is 0). When the first parameter is incremented, it becomes 1. When the second parameter is incremented, it also becomes 1. At the end of the procedure, both of these values (1 and 1) are written back into `i`, but since both parameters refer to the same variable, the last copied value overwrites the previous one.
- **Final value of `i`: 1**

Thus, under **call-by-reference**, `i` will be 2, while under **copy/restore**, `i` will be 1.

![[Pasted image 20240829020100.png]]

![[Pasted image 20240829020113.png]]
I'll be happy to help you solve this problem step-by-step, using the information provided in the image. Let's break it down:

Given information:
- Time for a null RPC (0 data bytes): 1.0 msec
- Additional time for every 1K of data: 1.5 msec
- We need to calculate for 32K of data

Step 1: Calculate the time for a single 32K RPC

1. Base time (null RPC): 1.0 msec
2. Additional time for 32K:
   32 * 1.5 msec = 48 msec
3. Total time for a single 32K RPC:
   1.0 msec + 48 msec = 49 msec

So, it takes 49 msec to read 32K from the file server in a single 32K RPC.

Step 2: Calculate the time for 32 1K RPCs

1. Base time for each RPC: 1.0 msec
2. Additional time for 1K: 1.5 msec
3. Time for each 1K RPC: 1.0 msec + 1.5 msec = 2.5 msec
4. Total time for 32 1K RPCs:
   32 * 2.5 msec = 80 msec

Therefore, it takes 80 msec to read 32K from the file server using 32 1K RPCs.

In conclusion:
- A single 32K RPC takes 49 msec
- 32 1K RPCs take 80 msec

The single 32K RPC is more efficient in this case, taking about 61% of the time required for multiple smaller RPCs.