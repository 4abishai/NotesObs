- In data communication, a signal is a *variation in a physical quantity* that *carries information*. 
- Signals are used to *transmit data* over various types of communication channels. 

### Analog Signal
Signals can be either analog or digital:
- An **analog signal** takes *many values*; a **digital signal** takes a *limited number of values*.
		![[Pasted image 20240809232404.png]]
- Can take many values and can be either *periodic* or *aperiodic*. In data communication, periodic analog signals are commonly used, which can be simple or composite.
  - A **simple periodic analog signal** is exemplified by a sine wave, which cannot be broken down into simpler signals. 
  - A sine wave, the most fundamental periodic analog signal, is characterized by three parameters: **period**, **peak amplitude**, and **phase**. The frequency of the sine wave is derived from the period, as it is the inverse of the period.

#### Peak Amplitude
The peak amplitude of a signal is the absolute value of its highest intensity. For electrical signals, peak amplitude is normally measured in volts.
#### Period and Frequency
- The period (T ) refers to the amount of *time*, in seconds, a signal needs to *complete one cycle*. 
- The frequency ( f ), measured in hertz (Hz), refers to the *number of periods in 1 s*.
- Period and frequency are just one characteristic defined in two ways. Period and frequency are inverses of each other, in other words (f = 1/ T).

#### Phase
- The term phase describes the position of the waveform relative to time 0. 
- Phase describes the amount of that shift. 
- Phase is measured in degrees or radians (360° is 2π rad).

#### Wavelength
- The wavelength is the *distance a simple signal can travel in one period*.
- While the frequency of a signal is independent of the medium, the *wavelength depends on both the frequency and the medium*. 
- `λ = c / f = c × T`
	- **λ** is the wavelength,	
	- **c** is the propagation speed in the medium,
	- **f** is the frequency,
	- **T** is the period of the signal.

#### Time and Frequency Domains
In the frequency domain, a sine wave is represented by one spike. The position of the spike shows the frequency; its height shows the peak amplitude.
![[Pasted image 20240809230352.png]]
#### Composite Signals
- A composite signal is made up of many simple sine waves.
- We need to send a composite signal to communicate **data**.
##### Bandwidth
The bandwidth of a composite signal is the difference between the highest and the lowest frequencies contained in that signal.

### Digital Signals
- A **digital signal** takes a *limited number of values*.
- Digital signals can have more than two levels, allowing them to encode multiple bits per level. If a signal has L levels, each level can encode log⁡2(L) bits. For instance, a signal with two levels encodes 1 bit per level, while a signal with four levels encodes 2 bits per level.

#### Bit Rate
 - The bit rate is defined as the *number of bits transmitted per second*, measured in bits per second (bps).
 - Since most digital signals are nonperiodic, bit rate is a more relevant characteristic than period or frequency.
![[Pasted image 20240809234759.png]]
#### Bit Length
 The bit length is the distance 1 bit occupies on the transmission medium.
`Bit length = 1 / (bit rate)`

#### Transmission of Digital Signals
Digital signals can be transmitted using two main approaches: **baseband transmission** and **broadband transmission**.

- **Baseband Transmission:** This method involves sending the digital signal directly over a channel without converting it to an analog signal. The signal is transmitted in its original digital form.

- **Broadband Transmission (Modulation):** In this approach, the digital signal is converted into an analog signal before transmission. This process, known as modulation, allows the digital information to be sent over channels that are designed for analog signals.

The choice between these methods depends on the nature of the channel and the requirements of the communication system.

### Signal Impairment
#### Attenuation and Amplification
- Attenuation means a *loss of energy*. 
- When a signal, simple or composite, travels through a medium, it loses some of its energy in overcoming the resistance of the medium. 
- To compensate for this loss, we need amplification.
- The **decibel (dB)** is a unit used by engineers to measure the *relative strength of signals*, either by comparing two different signals or by comparing the strength of the same signal at two different points. 
	- If the signal loses strength (attenuates), the decibel value is **negative**.
	- If the signal gains strength (is amplified), the decibel value is **positive**.
	
		The relationship between the powers of the signal at two points (P1 and P2) is expressed by the formula:
	
$$ \text{dB} = 10 \log_{10} \left(\frac{P2}{P1}\right) $$

#### Distortion
- Distortion means that the *signal changes its form or shape*.
- Differences in phase.
- Distortion can occur in a composite signal made up of different frequencies. Each signal component has its own propagation speed through a medium and, therefore, its own delay in arriving at the final destination.

#### Noise
- *Drop in quality*
- Noise is a significant factor that can impair signal transmission, and it comes in various forms:
	
	1. **Thermal Noise:** This type of noise is caused by the random motion of electrons within a wire. It generates an additional signal that wasn't originally transmitted, leading to interference.
	
	2. **Induced Noise:** This occurs when external sources like motors or appliances emit electromagnetic signals. These devices act as sending antennas, while the transmission medium (e.g., a wire) acts as the receiving antenna, picking up the unwanted noise.
	
	3. **Crosstalk:** Crosstalk happens when a signal in one wire affects another nearby wire. One wire acts as a sending antenna and the other as a receiving antenna, leading to interference between the two signals.
	
	4. **Impulse Noise:** Impulse noise is characterized by a sudden spike in energy, usually of very short duration. It can be caused by power lines, lightning, and other sudden electrical disturbances. This type of noise can significantly distort the signal.

#### Signal-to-Noise Ratio (SNR)
To determine the theoretical *bit-rate limit* in a communication system, the **Signal-to-Noise Ratio (SNR)** is an essential factor. SNR is defined as the ratio of the average signal power to the average noise power:

$$ \text{SNR} = \frac{\text{average signal power}}{\text{average noise power}} $$

Since both signal power and noise power can vary over time, the average values are used in this calculation. SNR provides a measure of how much stronger the signal is compared to the noise.

To express SNR in decibels (dB), which is a logarithmic scale often used in communication systems, the following formula is used:

$$ \text{SNR}_{\text{dB}} = 10 \log_{10}(\text{SNR}) $$

This decibel representation, helps in quantifying the strength of the signal relative to the noise in a more manageable form.

### Data Rate Limits
Data rate depends on three factors:
1. The bandwidth available
2. The level of the signals we use
3. The quality of the channel (the level of noise)

Two theoretical formulas were developed to calculate the data rate: one by Nyquist
for a noiseless channel, another by Shannon for a noisy channel.

### Noiseless Channel: Nyquist Bit Rate
For a noiseless communication channel, the **Nyquist bit rate** formula gives the theoretical maximum data rate that can be achieved:

$$ \text{BitRate} = 2 \times B \times \log_2 L $$
- **B** is the bandwidth of the channel in hertz.
- **L** is the number of distinct signal levels used to represent data.
- **BitRate** is the maximum number of bits per second that can be transmitted.

When we increase the number of signal levels, we impose a burden on the receiver.
This formula assumes an ideal, noise-free environment where the only limitation is the channel bandwidth and the number of signal levels.
![[Pasted image 20240810020625.png]]
### Noisy Channel: Shannon Capacity
In a real-world scenario where noise is present, the **Shannon capacity** formula, developed by Claude Shannon, provides the theoretical maximum data rate for a noisy channel:

$$ C = B \times \log_2 \left(1 + \text{SNR}\right) $$
- **B** is the bandwidth of the channel in hertz.
- **SNR** is the signal-to-noise ratio.
- **C** is the channel capacity in bits per second.

Formula defines a characteristic of the channel, not the method of transmission. The capacity defines the upper boundary of the channel bit rate.

![[Pasted image 20240810022046.png]]

This means that the highest bit rate for a telephone line is 34.881 kbps. If we want to send data faster than this, we can either increase the bandwidth of the line or improve the signal-to-noise ratio.
![[Pasted image 20240810024820.png]]

### Performance
#### Throughput
- The throughput is a measure of how fast we can actually send data through a network.
- A link may have a bandwidth of B bps, but we can send only T bps through this link, with T always less than B. In other words, the *bandwidth is a potential measurement* of a link; the *throughput is an actual measurement* of how *fast* we can send data.

#### Latency / Delay
- The latency or delay defines *how long it takes for an entire message to completely arrive at the destination* from the time the first bit is sent out from the source. 
- `Latency = propagation delay + transmission delay + queuing delay + processing delay`
#### Bandwidth-Delay Product
- The bandwidth-delay product defines the *number of bits that can fill the link*.
![[Pasted image 20240810121100.png]]

#### Jitter
- Jitter occurs if *different packets of data encounter different delays* and the application using the data at the receiver site is time-sensitive (audio and video data, for example).

### Digital Transmission
#### 1. Digital-to-Digital Conversion
- If we use digital transmission and our data are already digital, we need digital-to-digital conversion. 
##### Line Coding
- Line coding is the process of converting **digital data to digital signals**. 
- At the sender, digital data are *encoded into a digital signal*; at the receiver, the digital data are re-created by *decoding the digital signal*.
![[Pasted image 20240810123705.png]]
#### 2. Analog-to-Digital Conversion
- Sometimes we have an analog signal such as one created by a microphone or camera. The tendency today is to change an **analog signal to digital data** because the *digital signal is less susceptible to noise*.
##### Pulse Code Modulation (PCM)
A PCM encoder has three processes:
1. The analog signal is sampled every T s.
2. The sampled signal is quantized, which means every sample is considered as a pulse.
3. The quantized values (pulses) are encoded as streams of bits.
![[Pasted image 20240810131355.png]]
![[Pasted image 20240810130726.png]]
	![[Pasted image 20240810131233.png]]

###### PCM Bandwidth
It can be proved that the minimum bandwidth of the digital signal is
$$ B_{\text{min}} = n_b \times B_{\text{analog}} $$
This means the minimum bandwidth of the digital signal is nb times greater than the
bandwidth of the analog signal. This is the price we pay for digitization.

### Analog transmission
In communication systems, the choice between analog and digital transmission depends on the type of channel available:

- **Digital Transmission** requires a **low-pass channel**, which starts from 0 frequency. This method is preferred for transmitting digital data.

- **Analog Transmission** is necessary when using a **bandpass channel**, which does not start from 0 frequency. This method involves converting signals to fit the channel's frequency range.

  1. **Digital-to-Analog Conversion:** This process converts digital data into a bandpass analog signal for transmission over a *bandpass channel*.
  
  2. **Analog-to-Analog Conversion:** This process converts a low-pass analog signal into a bandpass analog signal, also for transmission over a *bandpass channel*. 

These conversions are essential for transmitting signals over channels that cannot directly carry the original digital or low-pass analog signals.

#### 1. Digital-to-Analog  conversion
A sine wave is defined by three characteristics: *amplitude, frequency,* and *phase*. **Digital-to-analog conversion** involves modifying one of the characteristics of an analog signal (typically a sine wave), giving us at least three mechanisms for modulating digital data into an analog signal
 encode digital data. Modulation techniques:

-  **Amplitude Shift Keying (ASK)**
- **Frequency Shift Keying (FSK)**
- **Phase Shift Keying (PSK)** 

Additionally, a more advanced and efficient technique called **Quadrature Amplitude Modulation (QAM)** combines changes in both amplitude and phase.

##### i. Amplitude Shift Keying (ASK)
**Amplitude Shift Keying (ASK)** is a digital modulation technique in which the amplitude of a carrier signal is varied to represent digital data, while the frequency and phase of the carrier remain constant. 

- **Binary Amplitude Shift Keying (BASK):** In its simplest form, ASK uses *two amplitude levels to represent binary data*. This is known as Binary ASK or **On-Off Keying (OOK)**. 
	In BASK:
	  - The other signal level has the same amplitude as the carrier frequency (representing a binary 1).
	  - One signal level has a peak amplitude of 0 (representing a binary 0).

**Bandwidth in ASK:** Although the carrier signal is a simple sine wave, the modulation process produces a nonperiodic composite signal, which contains a continuous set of frequencies. The bandwidth (B) of this signal is proportional to the signal rate (S, in baud), but also depends on a factor \( d \), which is determined by the modulation and filtering process. The bandwidth can be calculated using the formula:

$$
B = (1 + d)S
$$

- **Minimum Bandwidth:** \( S \) (when \( d = 0 \))
- **Maximum Bandwidth:** \( 2S \) (when \( d = 1 \))

The key point is that the **bandwidth is centered around the carrier frequency**  $$f_c$$ 
his allows the *modulated signal to be shifted to occupy a specific bandpass channel*, which is a major advantage of digital-to-analog conversion. By choosing an appropriate carrier frequency, the signal can be made to fit within the available channel bandwidth.
![[Pasted image 20240810183055.png]]

##### ii. Frequency Shift Keying (FSK)
**Frequency Shift Keying (FSK)** is a modulation technique where the frequency of the carrier signal is varied to represent digital data, while the amplitude and phase of the signal remain constant. In FSK, the carrier frequency stays constant for each signal element but changes when the data element changes.

- **Binary Frequency Shift Keying (BFSK):** In BFSK, two different carrier frequencies are used to represent binary data:
  - **\( f1 \)** is used to represent a binary 0.
  - **\( f2 \)** is used to represent a binary 1.
![[Pasted image 20240810181938.png]]

##### iii. Phase Shift Keying
In phase shift keying, the phase of the carrier is varied to represent two or more different signal elements.

#### 2. Analog-to-Analog Conversion
A common example is radio broadcasting, where each station is assigned a specific narrow bandwidth. The original low-pass analog signals from the stations need to be shifted to different frequency ranges (bandpass) so that listeners can tune into different stations.

There are three primary methods of analog-to-analog conversion:

1. **Amplitude Modulation (AM)**
2. **Frequency Modulation (FM)**
3. **Phase Modulation (PM)**

##### i. Amplitude Modulation (AM)
- In AM transmission, the carrier signal is modulated so that its *amplitude varies with the changing amplitudes of the modulating signal*. 
- The frequency and phase of the carrier remain the same; only the amplitude changes to follow variations in the information.
![[Pasted image 20240810195828.png]]
- The modulated signal covers the carrier signal.
- The modulation creates a *bandwidth that is twice the bandwidth of the modulating signal* and covers a *range centered on the carrier frequency*. However, the signal components above and below the carrier frequency carry exactly the same information. For this reason, some implementations discard one-half of the signals and cut the bandwidth in half.

##### ii. Frequency Modulation
- In FM transmission, the *frequency of the carrier signal* is modulated to follow the changing voltage level *(amplitude) of the modulating signal*.
![[Pasted image 20240810201126.png]]
- FM is normally implemented by using a **voltage-controlled oscillator (VCO)** as with FSK.
- Bandwidth is around,
$$ 
B_{FM} = 2(1 + \beta)B
$$
Beta is a factor the depends on modulation technique with a common value of 4. 

##### ii. Phase Modulation
- In FM transmission, the *phase of the carrier signal* is modulated to follow the changing voltage level *(amplitude) of the modulating signal*.
-  In FM, the instantaneous change in the carrier frequency is proportional to the amplitude of the modulating signal; in PM the instantaneous *change* in the carrier frequency is proportional to the *derivative of the amplitude* of the modulating signal.
- PM is implemented by using a *voltage-controlled oscillator* along with a derivative.
![[Pasted image 20240810202002.png]]
- Bandwidth is around,
$$ 
B_{PM} = 2(1 + \beta)B
$$

		Beta is:
			1 for narrowband		
			3 for wideband 