# Communication Engineering — Complete Exam Notes

## 1. Communication System

### Communication
Communication is the process of establishing a connection or link between two points for information exchange.

### Basic communication-system block diagram

```text
Information Source
       ↓
Input Transducer
       ↓
Transmitter / Modulator
       ↓
     Channel  ← Noise
       ↓
     Receiver
       ↓
Output Transducer
       ↓
   Destination
```

### Functions of blocks

| Block | Function |
|---|---|
| Information source | Produces the message: voice, music, picture, words, code, temperature, pressure, etc. |
| Input transducer | Converts the physical message into a time-varying electrical signal. Examples: microphone, camera, scanner. |
| Transmitter | Performs frequency-range restriction, amplification and modulation. |
| Channel | Path through which the signal travels from transmitter to receiver. |
| Noise | Unwanted random signal added to the desired signal. |
| Receiver | Extracts the original message from the degraded received signal. |
| Output transducer | Converts the electrical message back to the original physical form. |
| Destination | Final point where the information is obtained. |

### Communication channels

Important channel characteristics:

- Power required for the desired S/N ratio
- Channel bandwidth
- Amplitude response
- Phase response
- Linear or nonlinear behavior
- External interference

#### Telephone channel
- Designed for voice signals.
- Bandpass approximately 300–3400 Hz.
- Signal-to-noise ratio about 30 dB.
- Approximately linear response.
- Twisted-pair cable is a common example.
- Advantage: relatively cheap.

#### Coaxial cable
- Developed as a backbone of analog telephone networks.
- Large bandwidth and low loss.
- Shielding gives good noise immunity.
- 50 Ω and 75 Ω types are available.
- Suitable for point-to-point and point-to-multipoint communication.
- More expensive than twisted pair, but cheaper than optical fiber.

#### Optical fiber
- Thin strand of glass or plastic that carries messages using light.
- High bandwidth.
- Suitable for long distance.
- Immune to electrical/electromagnetic interference.
- Used for voice, video and telemetry.
- Advantages: small size, light weight, large bandwidth, no electrical/electromagnetic interference.
- Disadvantages: high initial cost, maintenance/repair cost, jointing/testing difficulties, tensile stress and fiber losses.

#### Terrestrial microwave
- Requires line-of-sight path.
- Unidirectional.
- Repeaters are needed for long-distance communication.
- No cable-break problem.
- Less maintenance than cables.
- Used in cellular, satellite and wireless networks.
- Advantages: no cables, multiple channels, wide bandwidth.
- Disadvantage: signal strength can reduce because of multipath reception.

#### Satellite channel
- A communication satellite acts as a microwave relay station.
- Connects two or more ground stations.
- The earth-to-satellite signal is the uplink.
- The satellite-to-earth signal is the downlink.
- A transponder amplifies and changes the signal frequency.
- The lecture gives examples of 6 GHz uplink and 4 GHz downlink and a geostationary height of about 35,683 km at the equator.
- Main disadvantage: very high building and launching cost.

---

## 2. Direction of Communication

| System | Meaning | Example |
|---|---|---|
| Simplex | Communication in one direction only | Radio/TV broadcasting |
| Half duplex | Communication in both directions, but one direction at a time | Walkie-talkie |
| Full duplex | Communication in both directions simultaneously | Telephone |

```text
Simplex:       A ─────→ B

Half duplex:   A ─────→ B
               A ←───── B
               (not at the same time)

Full duplex:   A ─────→ B
               A ←───── B
               (simultaneously)
```

---

## 3. Signal and Signal Classification

### Signal
A signal is a function of one or more independent variables containing information or data.

### Classification

```text
Signals
├── Continuous-time / Discrete-time
├── Analog / Digital
├── Deterministic / Non-deterministic (Random)
├── Even / Odd
├── Periodic / Aperiodic
├── Energy / Power
└── Real / Imaginary
```

### Continuous-time signal
Defined at all instants of time.

### Discrete-time signal
Defined only at discrete instants of time.

### Analog signal
Its amplitude can take any value in a continuous range.

### Digital signal
Its amplitude has a finite number of values.

### Deterministic signal
There is no uncertainty in its value at any instant. It can be described exactly by a mathematical expression.

### Non-deterministic / random signal
There is uncertainty in its value at some instants; it is random in nature.

### Even signal

\[
\boxed{x(t)=x(-t)}
\]

It is symmetric about the vertical axis.

### Odd signal

\[
\boxed{x(t)=-x(-t)}
\]

### Periodic signal

\[
\boxed{x(t)=x(t+T)}
\]

where \(T\) is the period.

### Aperiodic signal
Does not repeat.

### Real signal

\[
\boxed{x(t)=x^*(t)}
\]

For a real signal, the imaginary part is zero.

### Imaginary signal

\[
\boxed{x(t)=-x^*(t)}
\]

For a purely imaginary signal, the real part is zero.

---

## 4. Singularity Functions

### Unit step

\[
u(t)=
\begin{cases}
0,&t<0\\
1,&t>0
\end{cases}
\]

### Unit impulse

\[
\delta(t)=0,\quad t\neq0
\]

and

\[
\boxed{\int_{-\infty}^{\infty}\delta(t)\,dt=1}
\]

Important Fourier-transform pair:

\[
\boxed{\delta(t)\leftrightarrow1}
\]

### Unit ramp

\[
r(t)=
\begin{cases}
0,&t\le0\\
t,&t\ge0
\end{cases}
\]

---

## 5. Energy and Power Signals

### Signal energy
For a real signal:

\[
\boxed{E_g=\int_{-\infty}^{\infty}g^2(t)\,dt}
\]

For a complex signal:

\[
\boxed{E_g=\int_{-\infty}^{\infty}|g(t)|^2\,dt}
\]

### Signal power
For a real signal, the lecture uses the time-average measure:

\[
\boxed{P_g=\lim_{T\to\infty}\frac1T\int_{-T/2}^{T/2}g^2(t)\,dt}
\]

For a complex signal, use \(|g(t)|^2\) inside the integral.

### Practical identification

- A signal whose amplitude approaches zero as \(|t|\to\infty\) can have finite energy and is treated as an **energy signal**.
- A non-decaying signal generally has infinite energy and is better described by its **average power**.

### Lecture example
For the shown decaying signal, the suitable measure is energy and the lecture obtains:

\[
E_g=\int_{-1}^{0}2^2dt+\int_0^{\infty}4e^{-t}dt=4+4=8
\]

For the periodic sawtooth signal, the suitable measure is power and the shown example gives:

\[
P_g=\int_{-1}^{1}t^2dt=\frac13
\]

---

# 6. Time-Domain and Frequency-Domain Representation

### Time domain
The signal is shown as a function of time.

### Frequency domain
The signal is represented by its frequency spectrum, also called a line spectrum.

To draw a line spectrum, identify for every component:

1. Amplitude
2. Frequency
3. Phase

A **double-sided line spectrum** shows both positive and negative frequencies.

---

# 7. Fourier Series

Fourier series is used for **periodic signals**.

It tells:

- Which frequency components are present
- Their amplitudes
- Their relative phase differences

For exam problems, be able to obtain the Fourier-series representation of a square wave and draw the amplitude and phase spectra.

---

# 8. Fourier Transform

Fourier Transform is used to represent non-periodic signals extending over time and to convert a signal from the time domain to the frequency domain.

### Fourier Transform

\[
\boxed{X(\omega)=\int_{-\infty}^{\infty}x(t)e^{-j\omega t}\,dt}
\]

### Inverse Fourier Transform

\[
\boxed{x(t)=\frac1{2\pi}\int_{-\infty}^{\infty}X(\omega)e^{j\omega t}\,d\omega}
\]

### Important transform pairs from the lecture

#### Unit impulse

\[
\boxed{\delta(t)\leftrightarrow1}
\]

#### Decaying exponential

\[
\boxed{e^{-at}u(t)\leftrightarrow\frac1{a+j\omega}},\qquad a>0
\]

Magnitude:

\[
\boxed{|G(\omega)|=\frac1{\sqrt{a^2+\omega^2}}}
\]

Phase:

\[
\boxed{\theta_G(\omega)=-\tan^{-1}\left(\frac{\omega}{a}\right)}
\]

#### Rectangular pulse

\[
\boxed{
\operatorname{rect}\left(\frac{t}{\tau}\right)
\leftrightarrow
\tau\operatorname{sinc}\left(\frac{\omega\tau}{2}\right)
}
\]

### Cosine transform and spectrum
The exam asks for the Fourier transform of a sinusoid. Use the standard line-spectrum form from the lecture's spectrum treatment: a cosine produces spectral impulses at equal positive and negative frequencies.

---

# 9. Signal Transmission Through a Linear System

A transmission system can introduce distortion.

### Distortionless transmission
A distortionless system requires the amplitude response to remain constant over the signal band and the phase response to have the appropriate linear/group-delay behavior.

### Main channel distortions in the lecture

1. Linear distortion
2. Channel nonlinearities
3. Multipath effects
4. Fading channels

---

# 10. Linear and Nonlinear Distortion

### Linear distortion
The channel can cause:

- Magnitude distortion
- Phase distortion

### Nonlinear distortion

For a nonlinear system:

\[
y(t)=f(g(t))
\]

The nonlinear function can be expanded as a Maclaurin series.

If the input bandwidth is \(B\), the nonlinear output may occupy a larger bandwidth, written in the lecture as \(kB\).

### Adjacent-band consequence
Nonlinear distortion creates unwanted spectral components. If two signals occupy adjacent bands, these extra components can fall into the neighboring signal band and interfere with it.

### Multipath distortion
Multiple propagation paths can cause **frequency-selective fading**.

The lecture notes that fading varies with time and mentions automatic gain control (AGC) as a way to overcome fading-related level variation.

---

# 11. Correlation and Autocorrelation

Autocorrelation is useful for signals that are random/probabilistic and cannot be represented by one deterministic mathematical formula.

The lecture gives the relationship:

\[
\boxed{\text{Autocorrelation} \leftrightarrow \text{Energy Spectral Density}}
\]

The autocorrelation of \(g(t)\) is related to convolution with \(g(-t)\).

---

# 12. Modulation

### Definition
Modulation is the process of varying one or more properties of a high-frequency periodic waveform, called the carrier, according to a separate information-bearing signal called the modulation or message signal.

### Signals used

**Message / modulating / baseband signal:** contains the information.

**Carrier:** high-frequency sinusoidal signal used to shift the message to a higher frequency range.

### Why modulation is necessary

1. Reduces antenna height
2. Avoids mixing of signals
3. Increases communication range
4. Makes multiplexing possible
5. Improves reception quality

### Antenna-height relation

\[
\lambda=\frac cf
\]

The lecture uses minimum antenna height approximately equal to \(\lambda/4\):

\[
\boxed{h=\frac{\lambda}{4}=\frac{c}{4f}}
\]

For \(10\,\text{kHz}\):

\[
h=\frac{3\times10^8}{4(10\times10^3)}=7500\,m=7.5\,km
\]

For \(1\,\text{MHz}\):

\[
h=\frac{3\times10^8}{4(10^6)}=75\,m
\]

So the higher carrier frequency gives a much more practical antenna height.

---

# 13. Analog vs Digital Communication

### Analog communication
A carrier characteristic such as amplitude, frequency or phase is varied according to the instantaneous value of the modulating signal.

Examples:

- AM
- FM
- PM
- PAM
- PWM
- PPM

Advantages stated in the lecture:

- Simple transmitter and receiver
- Low bandwidth requirement
- FDM can be used

Drawbacks:

- Noise strongly affects signal quality
- Difficult to separate signal and noise
- Repeaters cannot be used between transmitter and receiver
- Coding is not possible
- Not suitable for secret information

Applications:

- Radio broadcasting
- TV broadcasting
- Telephone

### Digital communication
The transmitted information is represented digitally.

Advantages stated in the lecture:

- Reliable communication
- Less sensitivity to environmental changes
- Easy multiplexing/signaling
- Voice and data integration
- Encryption and compression are easier
- Transmission and switching can be integrated
- Long-distance communication is possible
- Repeaters can reproduce the signal with less distortion
- Better security

Disadvantages stated in the lecture:

- Higher bit rates can require greater bandwidth
- Synchronization is required for digital detection
- Sampling error can occur
- A digitized voice signal may be an approximation of the analog signal

---

# 14. Amplitude Modulation (AM)

## Definition
In AM, the amplitude of a high-frequency carrier changes according to the instantaneous amplitude of the information signal while carrier frequency and phase remain constant.

### Basic signals

Carrier:

\[
c(t)=A\cos\omega_ct
\]

General AM signal:

\[
\boxed{s(t)=[A+x(t)]\cos\omega_ct}
\]

or

\[
\boxed{s(t)=x(t)\cos\omega_ct+A\cos\omega_ct}
\]

The envelope is:

\[
\boxed{E(t)=A+x(t)}
\]

---

# 15. Single-Tone AM

Let

\[
x(t)=V_m\cos\omega_mt
\]

Then

\[
s(t)=A\cos\omega_ct+V_m\cos\omega_mt\cos\omega_ct
\]

Using

\[
\cos\alpha\cos\beta=\frac12[\cos(\alpha+\beta)+\cos(\alpha-\beta)]
\]

we get

\[
\boxed{
 s(t)=A\cos\omega_ct+\frac{V_m}{2}
 [\cos(\omega_c+\omega_m)t+\cos(\omega_c-\omega_m)t]
}
\]

If

\[
m=\frac{V_m}{A}
\]

then

\[
\boxed{
 s(t)=A\cos\omega_ct+\frac{Am}{2}
 [\cos(\omega_c+\omega_m)t+\cos(\omega_c-\omega_m)t]
}
\]

### Spectrum

```text
LSB                 Carrier                 USB
 |                      |                     |
fc − fm                fc                   fc + fm
```

- Carrier: \(f_c\)
- LSB: \(f_c-f_m\)
- USB: \(f_c+f_m\)

If the message bandwidth is \(B\):

\[
\boxed{BW_{AM}=2B}
\]

---

# 16. DSB-SC

**DSB-SC = Double Sideband Suppressed Carrier**

It contains both sidebands but no discrete carrier component.

```text
AM:       Carrier + USB + LSB
DSB-SC:          USB + LSB
                    no carrier
```

Bandwidth:

\[
\boxed{BW_{DSB-SC}=2B}
\]

---

# 17. AM Modulation Index

The modulation index is the ratio of message amplitude to carrier amplitude:

\[
\boxed{m=\frac{E_m}{E_c}}
\]

Percentage modulation:

\[
\boxed{\%\text{ modulation}=m\times100}
\]

### From the AM envelope

\[
\boxed{
 m=\frac{E_{\max}-E_{\min}}
 {E_{\max}+E_{\min}}
}
\]

### From the trapezoidal display

\[
\boxed{m=\frac{A-B}{A+B}}
\]

### Modulation conditions

| Condition | Meaning |
|---|---|
| \(m<1\) | Linear / under modulation |
| \(m=1\) | 100% modulation |
| \(m>1\) | Over modulation |

Over modulation introduces envelope distortion.

---

# 18. AM Envelope Method — Derivation

Start with:

\[
m=\frac{E_m}{E_c}
\]

From the envelope:

\[
E_m=\frac{E_{max}-E_{min}}2
\]

and

\[
E_c=\frac{E_{max}+E_{min}}2
\]

Therefore:

\[
m=\frac{(E_{max}-E_{min})/2}{(E_{max}+E_{min})/2}
\]

Hence,

\[
\boxed{m=\frac{E_{max}-E_{min}}{E_{max}+E_{min}}}
\]

---

# 19. AM Power

General AM signal:

\[
s(t)=x(t)\cos\omega_ct+A\cos\omega_ct
\]

Carrier power:

\[
\boxed{P_c=\frac{A^2}{2}}
\]

For the general AM signal, the lecture gives:

\[
\boxed{P_{AM}=\frac12[A^2+\overline{x^2(t)}]}
\]

For a single-tone AM signal:

\[
\boxed{P_t=P_c\left(1+\frac{m^2}{2}\right)}
\]

---

# 20. AM Transmission Efficiency

Transmission efficiency is the ratio of the information-bearing sideband power to the total transmitted power:

\[
\eta=\frac{P_{USB}+P_{LSB}}{P_t}
\]

For a single-tone AM wave:

\[
\boxed{
\eta=\frac{m^2}{2+m^2}\times100\%
}
\]

Important idea: the sidebands carry the information; the carrier term does not carry the message information.

---

# 21. AM Demodulation

### Demodulation
Recovering the message signal from the modulated signal.

### Two important detection methods

1. Coherent / synchronous detection
2. Envelope detection

---

# 22. Coherent / Synchronous Detection of DSB-SC

### Block diagram

```text
Received DSB-SC
       ↓
    Multiplier  ←  Locally generated carrier
       ↓
       LPF
       ↓
    Baseband message
```

For a DSB-SC signal \(x(t)\cos\omega_ct\), multiply by the local carrier:

\[
e(t)=x(t)\cos^2\omega_ct
\]

Since

\[
\cos^2\omega_ct=\frac12[1+\cos2\omega_ct]
\]

then

\[
e(t)=\frac12x(t)+\frac12x(t)\cos2\omega_ct
\]

The LPF removes the high-frequency term:

\[
\boxed{\hat x(t)=\frac12x(t)}
\]

### Main difficulty
The receiver must generate a local carrier with the correct frequency and phase. An unknown frequency or phase shift in the received signal makes coherent detection difficult.

---

# 23. Envelope Detector

The envelope detector uses rectification followed by an RC/low-pass action.

```text
AM signal
   ↓
Rectifier
   ↓
RC / Low-pass
   ↓
Envelope / message
```

The lecture gives the useful condition:

\[
\boxed{\frac1{f_c}\ll RC<\frac1B}
\]

---

# 24. Coherent vs Non-Coherent Detector

| Detector | Main requirement |
|---|---|
| Coherent / synchronous | Requires a properly synchronized local carrier |
| Non-coherent / envelope | Does not require recovery of the carrier phase in the same way; envelope detection is used for ordinary AM |

For the ordinary AM case with \(0\le m\le1\), the envelope detector is the practical detector because the message is present directly in the envelope.

For DSB-SC, synchronous/coherent detection is the important method in the lecture.

---

# 25. Ring Modulator and Switching Modulator

### Switching modulator
Uses switching/nonlinear devices such as diodes to generate the required modulation components.

### Ring modulator
During the **positive half-cycle of the carrier**:

- \(D_1\) and \(D_3\) conduct.
- Output is proportional to \(m(t)\).

During the **negative half-cycle of the carrier**:

- \(D_2\) and \(D_4\) conduct.
- Output is proportional to \(-m(t)\).

The ring modulator is a double-balanced switching modulator.

---

# 26. SSB — Single Sideband

SSB removes either the USB or the LSB.

Therefore:

\[
\boxed{BW_{SSB}=B}
\]

Compared with ordinary AM:

\[
BW_{AM}=2B
\]

### Hilbert transform
The lecture defines the Hilbert transform as an ideal phase shifter that shifts every positive spectral component by \(-\pi/2\).

\[
\boxed{H(f)=-j\,\operatorname{sgn}(f)}
\]

and

\[
\boxed{h(t)=\frac1{\pi t}}
\]

The SSB time-domain forms shown in the lecture are:

\[
\boxed{\phi_{USB}(t)=m(t)\cos\omega_ct-m_h(t)\sin\omega_ct}
\]

\[
\boxed{\phi_{LSB}(t)=m(t)\cos\omega_ct+m_h(t)\sin\omega_ct}
\]

---

# 27. QAM

**QAM = Quadrature Amplitude Modulation**

The lecture describes QAM as transmitting two DSB signals using carriers of the same frequency but in phase quadrature.

\[
\boxed{
\phi_{QAM}(t)=m_1(t)\cos\omega_ct+m_2(t)\sin\omega_ct
}
\]

It has in-phase and quadrature channels.

---

# 28. VSB

**VSB = Vestigial Sideband**

Lecture points:

- Relatively easy to generate.
- Bandwidth is typically about 25% greater than SSB.
- Used in television broadcasting.

For the lecture's TV example:

\[
BW_{SSB}=4.5\,MHz
\]

\[
BW_{VSB}=6\,MHz
\]

\[
BW_{DSB}=9\,MHz
\]

---

# 29. Angle Modulation

Angle modulation changes the **frequency or phase** of the carrier according to the message signal.

Two types:

- Frequency Modulation (FM)
- Phase Modulation (PM)

### Main features

- Better noise/interference discrimination than AM.
- This improvement is obtained at the expense of increased transmission bandwidth.
- Bandwidth can be traded for improved noise performance.

---

# 30. Frequency Modulation (FM)

### Definition
In FM, carrier amplitude remains constant while carrier frequency varies according to the amplitude of the modulating signal.

### Important features

- Frequency varies.
- The rate of carrier-frequency variation follows the information-signal frequency.
- Amount of frequency change is proportional to information-signal amplitude.
- Carrier amplitude is constant.

---

# 31. FM Mathematical Analysis

Message:

\[
v_m(t)=V_m\cos\omega_mt
\]

Carrier:

\[
v_c(t)=V_c\cos(\omega_ct+\theta)
\]

Instantaneous frequency:

\[
\boxed{\omega_i=\omega_c+Kv_m(t)}
\]

For the sinusoidal message:

\[
\boxed{\omega_i=\omega_c+KV_m\cos\omega_mt}
\]

FM wave:

\[
\boxed{
v_{FM}(t)=V_c\cos[\omega_ct+m_f\sin\omega_mt]
}
\]

where

\[
\boxed{m_f=\frac{KV_m}{\omega_m}}
\]

---

# 32. Frequency Deviation

The peak frequency deviation is

\[
\boxed{\Delta f=\frac{KV_m}{2\pi}}
\]

Also,

\[
\boxed{m_f=\frac{\Delta f}{f_m}}
\]

### FM numerical example pattern
If

\[
K=5\,kHz,
\qquad
v_m(t)=2\cos(2\pi 2000t)
\]

then

\[
V_m=2,
\qquad f_m=2000\,Hz
\]

Find \(\Delta f\) from the given deviation sensitivity and then

\[
m_f=\frac{\Delta f}{f_m}
\]

Keep units consistent.

---

# 33. FM Bandwidth — Carson's Rule

The lecture gives

\[
B_{fm}=2nf_m
\]

with

\[
n\approx m_f+2
\]

Therefore:

\[
B_{fm}=2(m_f+2)f_m
\]

Since

\[
m_f=\frac{\Delta f}{f_m}
\]

we get:

\[
\boxed{B_{FM}=2(\Delta f+f_m)}
\]

This is **Carson's rule** used in the lecture.

---

# 34. Digital Communication

The lecture describes digital communication as useful because it can provide reduced distortion, improved signal-to-noise performance, multiplexing, coding, repeaters, security, storage and long-distance transmission.

### Basic digital communication chain shown in the lecture

```text
Message
  ↓
Source Encoder / A-D conversion
  ↓
Baseband Modulation / Line Coding
  ↓
Digital Carrier Modulation
  ↓
Multiplexer
  ↓
Channel
  ↓
Regenerative Repeater
```

---

# 35. Pulse Modulation

## Analog pulse modulation
Some property of each pulse changes continuously according to the sampled message value.

### Types

| Type | Quantity varied |
|---|---|
| PAM | Pulse amplitude |
| PWM | Pulse width |
| PPM | Pulse position |

---

# 36. PAM

**PAM = Pulse Amplitude Modulation**

The pulse amplitude is varied according to the sampled value of the message.

### Advantages
- Simple modulation/demodulation
- Easy transmitter/receiver construction

### Disadvantages
- Large bandwidth
- More noise
- More power is required because amplitude varies

---

# 37. PWM

**PWM = Pulse Width Modulation**

The pulse width varies according to the sampled message value.

Example from the lecture: DC-motor speed control.

### Advantages
- Low power consumption
- About 90% efficiency stated in the lecture
- Less noise interference
- High power-handling capacity

### Disadvantages
- More complex circuit
- Voltage spikes
- More expensive
- More switching loss at high PWM frequency

---

# 38. PPM

**PPM = Pulse Position Modulation**

The pulse position changes according to the sampled message value.

### Advantages
- Constant amplitude
- Less noise interference
- Signal can be separated more easily from noise
- Good power efficiency

### Disadvantages
- Highly complex
- Requires more bandwidth

---

# 39. Sampling Theorem

If the highest frequency component of the analog signal is \(f_m\), the sampling frequency must satisfy:

\[
\boxed{f_s\ge2f_m}
\]

where

- \(f_s\) = sampling frequency / Nyquist rate
- \(f_m\) = highest frequency component

Also:

\[
\boxed{f_s=\frac1{T_s}}
\]

### Three cases

| Condition | Result |
|---|---|
| \(f_s>2B\) | Spectra remain separated |
| \(f_s=2B\) | Nyquist limit |
| \(f_s<2B\) | Aliasing occurs |

---

# 40. Sampling in Frequency Domain

The sample signal is represented in the lecture as:

\[
g(t)=x(t)\delta_{T_s}(t)
\]

and

\[
\boxed{
G(\omega)=\frac1{T_s}
\sum_{n=-\infty}^{\infty}X(\omega-n\omega_s)
}
\]

The spectrum is repeated at integer multiples of the sampling frequency.

---

# 41. Aliasing

Aliasing occurs when the sampling frequency is too low:

\[
\boxed{f_s<2f_m}
\]

The repeated spectra overlap, producing aliasing error and preventing correct reconstruction.

### How to avoid aliasing

1. Increase the sampling rate.
2. Use an anti-aliasing filter before sampling/down-sampling.

The anti-aliasing filter restricts the signal bandwidth so that the sampling condition is satisfied.

---

# 42. PCM

**PCM = Pulse Code Modulation**

PCM converts an analog signal to digital form using:

```text
Analog signal
      ↓
   Sampling
      ↓
 Quantization
      ↓
   Encoding
      ↓
 Binary PCM
```

### Important points

- PCM is an A/D conversion method.
- Sampling and quantization are required before binary encoding.
- A binary digital signal is useful because of its simplicity and ease of engineering.

### Advantages

- Useful for long-distance communication
- Higher noise immunity than the other pulse methods listed in the lecture
- Higher transmitter efficiency

### Disadvantages

- More bandwidth than analog systems
- More complex because encoding, decoding and quantization are required

### Applications

- Satellite transmission
- Space communication
- Telephony
- Compact disc

---

# 43. Quantization

Quantization maps each sample to one value from a finite set of permissible levels.

It is effectively a rounding/approximation process.

### Result
The approximation introduces **quantization error / quantization noise**.

If the input range from \(V_L\) to \(V_H\) is divided into \(M\) levels, the step size is

\[
\boxed{S=\frac{V_H-V_L}{M}}
\]

### Types

```text
Quantization
├── Uniform
│   ├── Midrise
│   └── Midtread
└── Non-uniform
```

### Uniform quantizer
Has the same step size throughout the signal range.

### Non-uniform quantizer
Uses non-uniform spacing of quantization levels.

---

# 44. Quantization Error / Noise

The quantization error is

\[
\boxed{e=V-V_q}
\]

For linear quantization, the lecture gives the error range

\[
-\frac S2\le e\le\frac S2
\]

and average quantization-noise power (variance)

\[
\boxed{\sigma^2=\frac{S^2}{12}}
\]

---

# 45. SQR — Signal-to-Quantization-Noise Ratio

SQR is a performance measure of PCM for transmitting speech.

For a sinusoidal input, the lecture gives

\[
\boxed{SQR=1.76+6.02n\;dB}
\]

where \(n\) is the number of bits per quantization code word.

Also,

\[
M=2^n
\]

### Key result

\[
\boxed{\text{One extra bit gives about }6\text{ dB improvement in SQR}}
\]

### Lecture values

| Bits | Levels | SQR |
|---:|---:|---:|
| 2 | 4 | 13.8 dB |
| 4 | 16 | 25.8 dB |
| 6 | 64 | 37.8 dB |
| 7 | 128 | 43.8 dB |
| 8 | 256 | 49.8 dB |
| 9 | 512 | 55.8 dB |
| 10 | 1024 | 61.8 dB |

---

# 46. Binary PCM

The lecture's binary-PCM example follows this chain:

```text
Sample value
     ↓
Quantized value
     ↓
Code number
     ↓
Binary code
```

Each sample is assigned a finite quantization level and then represented by a binary code word.

---

# 47. Noise

### White noise
A random-noise concept used in communication-system noise analysis. The lecture includes white-noise power/spectral representation and band-limited white noise.

### Thermal noise
Noise associated with the thermal/random motion of charge carriers; it is included by the lecture under communication-system noise.

### Signal-to-noise ratio

\[
\boxed{SNR=\frac{P_s}{P_n}}
\]

In decibels:

\[
\boxed{SNR_{dB}=10\log_{10}\left(\frac{P_s}{P_n}\right)}
\]

Noise is unavoidable and is one of the main physical limitations of communication systems.

---

# 48. Essential Formula Sheet

## Basic communication

\[
\boxed{BW=f_2-f_1}
\]

\[
\boxed{\lambda=\frac cf}
\]

\[
\boxed{h=\frac{\lambda}{4}=\frac{c}{4f}}
\]

## Signal classification

\[
\boxed{x(t)=x(-t)}\quad\text{(even)}
\]

\[
\boxed{x(t)=-x(-t)}\quad\text{(odd)}
\]

\[
\boxed{x(t)=x(t+T)}\quad\text{(periodic)}
\]

## Energy

\[
\boxed{E=\int_{-\infty}^{\infty}|x(t)|^2dt}
\]

## Fourier Transform

\[
\boxed{X(\omega)=\int_{-\infty}^{\infty}x(t)e^{-j\omega t}dt}
\]

\[
\boxed{x(t)=\frac1{2\pi}\int_{-\infty}^{\infty}X(\omega)e^{j\omega t}d\omega}
\]

\[
\boxed{\delta(t)\leftrightarrow1}
\]

\[
\boxed{e^{-at}u(t)\leftrightarrow\frac1{a+j\omega}}
\]

\[
\boxed{rect(t/\tau)\leftrightarrow\tau sinc(\omega\tau/2)}
\]

## AM

\[
\boxed{s(t)=[A+x(t)]\cos\omega_ct}
\]

\[
\boxed{BW_{AM}=2B}
\]

\[
\boxed{m=\frac{E_m}{E_c}}
\]

\[
\boxed{m=\frac{E_{max}-E_{min}}{E_{max}+E_{min}}}
\]

\[
\boxed{m=\frac{A-B}{A+B}}
\]

\[
\boxed{P_c=\frac{A^2}{2}}
\]

\[
\boxed{P_t=P_c\left(1+\frac{m^2}{2}\right)}
\]

\[
\boxed{\eta=\frac{m^2}{2+m^2}\times100\%}
\]

## Synchronous detection

\[
\boxed{e(t)=x(t)\cos^2\omega_ct}
\]

\[
\boxed{e(t)=\frac12x(t)+\frac12x(t)\cos2\omega_ct}
\]

After LPF:

\[
\boxed{\hat{x}(t)=\frac12x(t)}
\]

## SSB

\[
\boxed{BW_{SSB}=B}
\]

## FM

\[
\boxed{\omega_i=\omega_c+Kv_m(t)}
\]

\[
\boxed{\Delta f=\frac{KV_m}{2\pi}}
\]

\[
\boxed{m_f=\frac{\Delta f}{f_m}}
\]

\[
\boxed{BW_{FM}=2(\Delta f+f_m)}
\]

## Sampling

\[
\boxed{f_s\ge2f_m}
\]

\[
\boxed{f_s=\frac1{T_s}}
\]

Aliasing:

\[
\boxed{f_s<2f_m}
\]

## Quantization

\[
\boxed{S=\frac{V_H-V_L}{M}}
\]

\[
\boxed{e=V-V_q}
\]

\[
\boxed{\sigma^2=\frac{S^2}{12}}
\]

\[
\boxed{SQR=1.76+6.02n\;dB}
\]

\[
\boxed{M=2^n}
\]

---

# 49. Exam-Answer Templates

## Define modulation

**Modulation is the process of varying one or more properties of a high-frequency carrier according to the information-bearing modulating signal.**

## Why is modulation necessary?

Write these five points:

1. Reduction in antenna height
2. Avoids mixing of signals
3. Increases communication range
4. Makes multiplexing possible
5. Improves reception quality

## Define AM

**AM is the process of varying the amplitude of a high-frequency carrier in accordance with the instantaneous amplitude of the information signal while carrier frequency and phase remain constant.**

## Define FM

**FM is the process in which the carrier frequency varies according to the amplitude of the modulating signal while carrier amplitude remains constant.**

## Define angle modulation

**Angle modulation is modulation in which the frequency or phase of the carrier is varied according to the message signal.**

## Define PCM

**PCM is a method of converting an analog signal into digital form by sampling, quantization and binary encoding.**

## Define quantization

**Quantization is the process of representing each sampled value by the nearest value from a finite set of permissible levels.**

## Define aliasing

**Aliasing is the error caused by spectral overlap when the sampling frequency is less than twice the highest frequency component of the signal.**

## Define SSB

**SSB is an amplitude-modulation technique in which either the upper or lower sideband is removed, giving a bandwidth of B for a message of bandwidth B.**

---

# 50. Compact Comparison Tables

## AM vs FM vs PM

| Feature | AM | FM | PM |
|---|---|---|---|
| Varying quantity | Amplitude | Frequency | Phase |
| Carrier amplitude | Varies | Constant | Constant |
| Information relation | Amplitude change follows message | Frequency change follows message amplitude | Phase change follows message amplitude |
| Lecture examples | Radio/TV | TV sound, FM radio, police wireless | Used for FM generation |

## Coherent vs Envelope Detection

| Feature | Coherent / synchronous | Envelope |
|---|---|---|
| Main method | Multiply by local carrier, then LPF | Rectify and low-pass/RC |
| Carrier requirement | Correct local carrier needed | Envelope is directly detected |
| Important use in lecture | DSB-SC | Ordinary AM |

## PAM vs PWM vs PPM

| Scheme | Changes | Key advantage from lecture |
|---|---|---|
| PAM | Amplitude | Simple |
| PWM | Width | Lower power / less noise |
| PPM | Position | Constant amplitude / good noise immunity |

## AM vs SSB vs DSB-SC vs VSB

| Scheme | Main idea | Bandwidth |
|---|---|---|
| AM | Carrier + both sidebands | \(2B\) |
| DSB-SC | Both sidebands, carrier suppressed | \(2B\) |
| SSB | One sideband only | \(B\) |
| VSB | One full sideband + vestige of the other | Greater than SSB; lecture TV example: 6 MHz |

---

# 51. Worked Mini-Examples

## Example A — AM modulation index

Given:

\[
E_{max}=9V,\qquad E_{min}=3V
\]

Then

\[
m=\frac{9-3}{9+3}=\frac6{12}=0.5
\]

Percentage modulation:

\[
0.5\times100=50\%
\]

---

## Example B — AM total power

Given:

\[
P_c=400W,\qquad m=0.75
\]

\[
P_t=400\left(1+\frac{0.75^2}{2}\right)
\]

\[
P_t=400(1+0.28125)=512.5W
\]

---

## Example C — AM efficiency

For \(m=0.75\):

\[
\eta=\frac{0.75^2}{2+0.75^2}\times100\%
\]

\[
\eta=\frac{0.5625}{2.5625}\times100\%
\approx21.95\%
\]

---

## Example D — FM deviation and modulation index

For

\[
K=5kHz,
\qquad V_m=2,
\qquad f_m=2kHz
\]

Using the lecture's deviation relation:

\[
\Delta f=\frac{KV_m}{2\pi}
\]

and then

\[
m_f=\frac{\Delta f}{f_m}
\]

For problems where the deviation sensitivity is already expressed directly in frequency-deviation units per input-voltage unit, use the units exactly as specified by the problem and multiply by \(V_m\) to obtain the peak deviation.

---

## Example E — Sampling

If

\[
f_m=5kHz
\]

minimum sampling frequency:

\[
\boxed{f_s\ge10kHz}
\]

Any value below 10 kHz causes aliasing for this band-limited signal.

---

## Example F — SQR

For an 8-bit PCM system:

\[
SQR=1.76+6.02(8)
\]

\[
\boxed{SQR=49.92\,dB\approx49.8\,dB}
\]

The lecture's table gives 49.8 dB.

---

# 52. Ultra-Quick Revision Page

### Communication

```text
Source → Transducer → Transmitter → Channel → Receiver → Destination
                                   ↑
                                  Noise
```

### Duplex

```text
Simplex     →
Half duplex → ← (one at a time)
Full duplex → ← (simultaneously)
```

### Modulation

```text
AM → amplitude changes
FM → frequency changes
PM → phase changes
```

### AM

\[
s(t)=[A+x(t)]\cos\omega_ct
\]

\[
BW=2B
\]

\[
m=\frac{E_{max}-E_{min}}{E_{max}+E_{min}}
\]

\[
P_t=P_c(1+m^2/2)
\]

\[
\eta=\frac{m^2}{2+m^2}\times100\%
\]

```text
m < 1 → under/linear
m = 1 → 100%
m > 1 → over modulation
```

### DSB-SC

```text
USB + LSB
No carrier
```

### Synchronous detector

```text
DSB-SC → Multiplier → LPF → Message
             ↑
        Local carrier
```

### SSB

```text
One sideband only
BW = B
```

### FM

\[
\Delta f=\frac{KV_m}{2\pi}
\]

\[
m_f=\frac{\Delta f}{f_m}
\]

\[
BW=2(\Delta f+f_m)
\]

### Sampling

\[
\boxed{f_s\ge2f_m}
\]

```text
fs < 2fm → aliasing
```

### PCM

```text
Sampling → Quantization → Encoding
```

### Quantization

\[
S=\frac{V_H-V_L}{M}
\]

\[
e=V-V_q
\]

\[
\sigma^2=\frac{S^2}{12}
\]

### SQR

\[
SQR=1.76+6.02n\;dB
\]

```text
+1 bit → about +6 dB SQR
```

### Noise

\[
SNR=\frac{P_s}{P_n}
\]

\[
SNR_{dB}=10\log_{10}(P_s/P_n)
\]

### Most repeated exam areas

AM equation/spectrum, modulation index, AM power and efficiency, DSB-SC detection, Fourier analysis, energy/power signals, FM deviation and Carson's rule, sampling/aliasing, quantization/SQR, PCM, communication-system basics and duplex systems.

---

## Source Boundary Note

This resource is intentionally limited to the material supported by the five uploaded lecture decks and the previous-year papers. The uploaded lecture decks list **line coding** in the course contents, and the 2024 paper asks about line coding/NRZ/RZ, but the five uploaded decks do not provide a developed line-coding lesson. Therefore no outside line-coding theory has been inserted here.
