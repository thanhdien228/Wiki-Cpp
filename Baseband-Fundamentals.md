## Basic Samplings

- **Sample period**: the time interval between 2 consecutive sampling times
- **Sample rate**: the number of samplings performed in one second (the inverse of sample period)

## Nyquist Samplings

**Nyquist Rate**: the minimum rate in which we can sample the given signal without leading to ambiguity (more than one signal can produce the same sample), a.k.a aliasing.


The sample rate must be at least twice as much as the highest frequency component:
$$f_s >= 2B$$
where:
- $f_s$: the NyQuist Rate
- $B$: the highest frequency component

## Quadrature Samplings

**Quadrature in DSP**: referring to 2 waves that are 90 degrees out of phase, or orthogonal

The idea of Quadrature sampling (IQ sampling) is to use 2 quadrature sinusoidal wave component (generated by 1 sine wave), to create an arbitrarily phase-shifted sine wave, just by adjusting the amplitudes of 2 components. The transmitted wave can be represent as:

$$x(t) = Icos(2\pi.f.t) + Qsin(2\pi.f.t)$$

where $I$ and $Q$ is the amplitude of the in-phase and the quadrature components' amplitudes respectively

Here we can easily remember this:
$I$ - in-phase - the $cos()$ component
$Q$ - quadrature - the $sin()$ component

The important takeaways are that when we add the cos() and sin(), we get another pure sine wave with a different phase and amplitude. Also, the phase shifts as we slowly remove or add one of the two parts. The amplitude also changes. This is all a result of the trig identity: a \cos(x) + b \sin(x) = A \cos(x-\phi), which we will come back to in a bit. The “utility” of this behavior is that we can control the phase and amplitude of a resulting sine wave by adjusting the amplitudes I and Q (we don’t have to adjust the phase of the cosine or sine). For example, we could adjust I and Q in a way that keeps the amplitude constant and makes the phase whatever we want.

## Complex number to represent IQ:

In trigonometry, the x-axis is used to denoted for cosine or $cos()$, the y-axis is used to denoted for sine or $sin()$, the quadrature component of $cos()$  
Hence, the y-axis now is also used to represent the Q, the imaginary portion, and the x-axis is used to represent the I, the real portion

Let's take a look of this example:

$$x(t) = 0.7cos(2\pi.f.t) + 0.4sin(2\pi.f.t)$$

It is obvious that $I = 0.7$ and $Q = 0.4$

With just I and Q, we can easily calculate the magnitude and the phase of $x(t)$

Finally, we have the new representation of $x(t)$:

$$x(t) = 0.806cos(2\pi.f.t + 0.519)$$

## Carrier and Down/Up Conversion

Carrier refer to the sinusoidal wave that carries no information that has been modified by information-bearing signal for transmitting the information throughout space. 

The 'f' we denoted in those previous equation is the center frequency of the signal we actually send it through the air. I and Q are the information we want to transmit, and we tuned them to the carrier.

Modulation of the carrier simply refers to the change we made to IQ values that make a change to the phase or/and the amplitude of the carrier. Sometimes the option is to change the frequency of the carrier, which is called Frequency Modulation, as in FM radio does.

**The implication of direct sampling:**

- As mentioned, the sample rate must be at least twice as much as the maximum frequency component. The maximum component often refers to the carrier frequency, as it is much higher than the information-bearing signal.

- Say the carrier frequency is 2.4 GHz, like WiFi or Bluetooth. That means we would have to sample at 4.8 GHz. An ADC that can sample that fast costs thousands of dollars.

- For that reason, down conversion comes into play. The component that handles the down conversion (and the up conversion) is the mixer. It simply performs multiplication between sinusoidal waves (one received by RF filter and  and other  use a filter to choose the right IF for the down conversion (low-pass) or the up conversion (high-pass).

## Receiver Architectures

The figure in the “Receiver Side” section demonstrates how the input signal is down converted and split into I and Q. This arrangement is called “direct conversion”, or “zero IF”, because the RF frequencies are being directly converted down to baseband. Another option is to not down convert at all and sample so fast to capture everything from 0 Hz to 1/2 the sample rate. This strategy is called “direct sampling” or “direct RF”, and it requires an extremely expensive ADC chip. A third architecture, one that is popular because it's how old radios worked, is known as “superheterodyne”. It involves down conversion but not all the way to 0 Hz. It places the signal of interest at an intermediate frequency, known as “IF”. A low-noise amplifier (LNA) is simply an amplifier designed for extremely low power signals at the input. Here are the block diagrams of these three architectures, note that variations and hybrids of these architectures also exist:

<p align="center"><img src="https://pysdr.org/_images/receiver_arch_diagram.svg"/></p>

## What is baseband unit (BBU) in RAN?

BBU is the short form of baseband unit. As I said, a BBU processes baseband signals. In 5G networks, it is responsible for managing all 5G protocols and managing connectivity to the 5G core.
BBU performs multiple vital functions:

**Signal Processing**

The BBU handles the digital processing of information between a Base Station (BS) and mobile devices. It enables voice and data transmission through processing. This involves interpreting and managing the baseband frequencies.

**Communication**

The Baseband unit interfaces with both the radio network and the core network. It facilitates data transmission and reception. However, the BBU connects to the core network through fiber optic cables to support the execution of management functions. 

**Control Functions**

Any BBU manages radio resources and system maintenance for the efficient operation of the base station.

**Integration with Remote Radio Head (RRH)**

BBUs typically work with Remote Radio Heads (RRH) to facilitate communication between the base station and mobile devices. This setup is essential for efficient signal processing and transmission.

**C-RAN Architecture**

The Cloud Radio Access Network (C-RAN) architecture separates the BBU and RRU. It centralizes BBUs in a data center or cloud environment and distributes RRUs at cell sites. This setup allows for more flexible and scalable network management.

## References

- [Clarification between up conversion and down conversion](https://electronics.stackexchange.com/questions/475377/clarifications-about-up-and-down-converting-mixers)
- [PySDR - A guide to SDR and DSP using Python](https://pysdr.org/content/sampling.html#receiver-architectures)
- [Superheterodyne - Wikipedia](https://en.wikipedia.org/wiki/Superheterodyne_receiver#Demodulator)
- [Baseband - Wikipedia](https://en.wikipedia.org/wiki/Baseband)